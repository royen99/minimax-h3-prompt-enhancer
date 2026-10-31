---
license: apache-2.0
base_model: huihui-ai/Huihui-Qwen3-VL-8B-Instruct-abliterated
pipeline_tag: image-text-to-text
tags: [prompt-enhancer, text-to-video, image-to-video, reference-to-video, minimax-h3, ollama, qwen3-vl]
---

# MiniMax H3 Prompt Enhancer

An [Ollama](https://ollama.com) model that turns a short idea, and optionally a reference image, into a full audiovisual prompt for the [MiniMax H3](https://huggingface.co/MiniMaxAI/MiniMax-H3) video model.

H3 generates video together with sound, so a good prompt describes both what is seen and what is heard. This model writes prompts in the structure from MiniMax's official [video prompt writing guide](https://huggingface.co/MiniMaxAI/MiniMax-H3/blob/main/docs/VIDEO_PROMPT_WRITING_GUIDE_base_en.md): a timeline of shots with camera moves, speaker IDs and dialogue, a soundscape, and background music.

It is a prompt and parameter layer on top of an existing vision model. It contains no new weights and no MiniMax code or weights.

There are two variants, built from two Modelfiles:

| Model | Modelfile | Use it for |
|-------|-----------|------------|
| `minimax-prompt-enhancer` | `Modelfile` | Text to video, image to video, first and last frame |
| `minimax-prompt-enhancer-ref` | `Modelfile.ref` | Reference to video: images are references, not frames ([jump to it](#reference-to-video-modelfileref)) |

## What it does

Give it an idea (and attach an image if you have one), get back only the finished prompt.

| Task | You provide | Output starts with |
|------|-------------|--------------------|
| **Text to video** | Text only | `integrated_multimodal_description:` |
| **Image to video** | One image (used as the first frame) | The first-frame alignment line, then the three fields |
| **First and last frame** | Two images | The first-and-last-frame alignment line |
| **Last frame** | One image, plus "ending frame" in your text | The last-frame alignment line |

Every prompt has the three core fields from the guide:

- `integrated_multimodal_description`: visuals, shots, camera, speech and sounds made by things in the scene, along a timeline.
- `overall_soundscape`: ambience, physical sounds, and non-verbal human sounds.
- `non_diegetic_music`: background music the characters cannot hear.

Behavior:

- **Timeline format:** `[Shot 1]` has no timestamp; later shots read `[Shot 2] At 00:04.500, the camera cuts to ...` with strictly increasing times inside the duration.
- **Camera language:** motion type, amplitude and speed, for example "The camera pushes in with small amplitude at slow speed".
- **Speech:** speakers get IDs such as `(S1)`, and the words go in `<d>[English] ...</d>`, kept exactly as you wrote them. Voiceover and lip handling follow the guide.
- **Image tasks:** the style, subjects and composition come from your image, and the video develops forward from it (or lands on it, for a last frame).
- **People:** every person is described with an apparent age and ethnicity. If you give them, they are used exactly; otherwise they come from the image or the setting.
- **No invented text:** signs and labels appear only if you asked for them or they are in your image.
- **Duration:** use the one you state ("8 seconds"). The default is 8 seconds. Short clips are a single shot; longer clips get cuts.

See [examples/outputs.md](examples/outputs.md) for real outputs.

## Requirements

- [Ollama](https://ollama.com/download)
- The base model `huihui_ai/qwen3-vl-abliterated:8b-instruct` (about 6 GB). It is downloaded on first build. It fits on a 16 GB GPU with room to spare.

## Install

```bash
git clone https://github.com/royen99/minimax-h3-prompt-enhancer
cd minimax-h3-prompt-enhancer
ollama create minimax-prompt-enhancer -f Modelfile
ollama create minimax-prompt-enhancer-ref -f Modelfile.ref
```

Build only the one you need. Re-run the same command after editing a Modelfile; unchanged layers are reused.

## Usage

Put the duration in your request. With `ollama run`, the image path goes inside the prompt text.

**Text to video**

```bash
ollama run minimax-prompt-enhancer "a woman walks her dog through a rainy Tokyo street at night, then enters a small cafe. 12 seconds"
```

**Image to video** (the image is the first frame)

```bash
ollama run minimax-prompt-enhancer "the cats wake up one by one ./cats.png"
```

**Last frame** (say it is the ending frame)

```bash
ollama run minimax-prompt-enhancer "ending frame of the video: the cats are all asleep, the earlier part shows them settling down. 6 seconds ./cats.png"
```

**First and last frame** (two images, in order)

```bash
ollama run minimax-prompt-enhancer "the cyclist opens her umbrella. 8 seconds ./first.png ./last.png"
```

**Dialogue:** write the exact words in quotes and they are kept verbatim:

```bash
ollama run minimax-prompt-enhancer "two kids in a treehouse; the older boy says \"Do you hear that?\" and the girl whispers \"It is coming closer\". 6 seconds"
```

Or through the API, with images as base64:

```bash
curl http://127.0.0.1:11434/api/generate -d '{
  "model": "minimax-prompt-enhancer",
  "prompt": "the camera slowly pushes in as a breeze moves the sign. 5 seconds",
  "images": ["<base64 image>"],
  "stream": false
}'
```

Paste the result into MiniMax H3 as the prompt, and attach the same image(s) as the reference picture(s), in the same order.

### In ComfyUI

Use the `comfyui-ollama` node with the model name `minimax-prompt-enhancer`. For image-to-video, connect the same image to the node and to the H3 reference input.

## Reference-to-video (`Modelfile.ref`)

In H3's reference-to-video mode, an image is only a **reference**: it supplies a character, a place, an object, or a style for the new video, and it is not automatically the first frame. This needs a different prompt format, so it has its own model, built from `Modelfile.ref` and based on MiniMax's [reference-mode guide](https://huggingface.co/MiniMaxAI/MiniMax-H3/blob/main/docs/VIDEO_PROMPT_WRITING_GUIDE_ref_en.md).

```bash
ollama create minimax-prompt-enhancer-ref -f Modelfile.ref
```

### What it writes

Six sections, in this order:

| Section | Content |
|---------|---------|
| `subject_definitions` | One line per reusable item (`<Subject 1>`, `<Subject 2>` ...) saying which image it comes from and which features to follow. A frame anchor is written as `<Picture 1> is the first frame of [Shot 1], showing ...` |
| `summary` | A short paragraph that starts with a task type: `[reference generation]` (default) or `[keyframe completion]`, or both joined with `+` |
| `retention_analysis` | One line per label with how it is kept: `fully_preserved`, `partially_preserved`, `attribute_transfer` or `weak_reference` |
| `detailed_description` | The shot-by-shot timeline, with the labels inserted where they apply. Same shot, camera, speech and sound rules as the main model |
| `overall_soundscape` | Ambience and physical sounds |
| `non_diegetic_music` | Audience-only background music |

### Usage

Attach one or more images and say what each is for. The images are numbered in the order you give them. If you do not say, the model decides from the picture (a person is a character, a place is a scene). The default duration is 10 seconds.

**One image as a character**

```bash
ollama run minimax-prompt-enhancer-ref "use the image as the character. She walks through a rainy night market and says \"Almost home.\" 8 seconds ./woman.png"
```

**Two images: a character and a location**

```bash
ollama run minimax-prompt-enhancer-ref "image 1 is the character, image 2 is the location. He climbs the spiral stairs and looks out over the sea. 10 seconds ./man.png ./lighthouse.png"
```

**An image as the first frame** (together with the reference format)

```bash
ollama run minimax-prompt-enhancer-ref "image 1 is the first frame; the baker unlocks the door and says \"Fresh bread in ten minutes.\" 6 seconds ./bakery.png"
```

Paste the result into MiniMax H3's reference-to-video prompt, and attach the same images in the same order, so that `<Picture 1>` and `<Picture 2>` match.

### Limits of the reference model

- **Images only.** Ollama cannot attach video or audio, so reference videos and reference audio (`<Video N>` and `<Audio N>`, and the audio retention markers) are not supported. Edit and continuation tasks that need a source video are out of scope.
- `detailed_description` is often shorter than the 350 to 500 words the guide recommends (typically 200 to 280 words). Ask for more detail, or extend it by hand.
- A person in a reference image is sometimes described without an age or ethnicity.
- A silent character occasionally gets a speaker ID such as `(S1)`. Remove it when nobody speaks, because IDs belong only to people who have a `<d>` dialogue block.
- Section names are sometimes followed by a blank line, and there can be extra blank lines between sections. They are harmless.

## Settings

Defined in the [Modelfile](Modelfile) and, with the same values, in [Modelfile.ref](Modelfile.ref):

| Parameter | Value | Why |
|-----------|-------|-----|
| `temperature` | 0.2 | Follows the strict prompt format closely |
| `top_p` / `top_k` | 0.8 / 20 | Qwen3 recommended non-thinking sampling |
| `num_ctx` | 16384 | Room for the system prompt, examples, and images |
| `num_predict` | 4096 | Caps runaway output |

The `Modelfile` holds the full system prompt and several example conversations (few-shot) that teach the exact format.

The base model must be the `-instruct` variant. The default `qwen3-vl-abliterated` tag is a thinking model that can spend its whole output budget on reasoning and return nothing.

## Project layout

```text
Modelfile             system prompt, parameters, few-shot examples
Modelfile.ref         reference-to-video variant (six-section format)
examples/
  inputs.jsonl        test prompts with what to check
  outputs.md          real model outputs
  images/             reference images used in the examples
```

## Known limitations

This runs on an 8B model, so rule-following is good but not perfect. Check the result before generating:

- A speaker who first appears in a later shot sometimes gets no speaker ID.
- The model sometimes writes speech you did not ask for (it should only add words when you ask for people to talk).
- Cut times are often 4.5 s and 9.0 s, copied from the built-in example, and are rarely tuned to your story.
- A person in a reference image is sometimes described without an age or ethnicity.
- For image-to-video, the first-frame alignment line is occasionally missing. If it is, add this as the first line, followed by a blank line:
  `For the target video, at 0.00 seconds into the target video, <Picture 1> (from [Shot 1]) is fully referenced.`
- Some outputs have extra blank lines between the three fields. They are harmless.

## About the base model

The default base model, `huihui_ai/qwen3-vl-abliterated:8b-instruct`, is a Qwen3-VL variant with its refusal behavior removed. This keeps it from refusing ordinary creative prompts, but it also means the model has **no built-in safety filtering**. The system prompt does not add content restrictions. Add your own if you plan to expose the model to other people.

## Credits

- Prompt structure, field definitions and camera vocabulary: the [MiniMax H3 video prompt writing guide](https://huggingface.co/MiniMaxAI/MiniMax-H3/blob/main/docs/VIDEO_PROMPT_WRITING_GUIDE_base_en.md) by MiniMax.
- Reference-to-video format (`Modelfile.ref`): the [MiniMax H3 full-reference mode guide](https://huggingface.co/MiniMaxAI/MiniMax-H3/blob/main/docs/VIDEO_PROMPT_WRITING_GUIDE_ref_en.md) by MiniMax.
- Base model: [huihui_ai/qwen3-vl-abliterated](https://ollama.com/huihui_ai/qwen3-vl-abliterated), derived from [Qwen3-VL](https://github.com/QwenLM/Qwen3-VL).

This project is not affiliated with MiniMax or Alibaba. It does not include or redistribute any MiniMax model, code, or documentation. It only encodes the structure the guide describes into a system prompt, with its own wording and examples.

## License

Licensed under the [Apache License 2.0](LICENSE).
