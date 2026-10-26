# Example outputs

Real outputs from `minimax-prompt-enhancer` (one run each) for the cases in [inputs.jsonl](inputs.jsonl). Output varies between runs. Extra blank lines between the fields were removed; the text is otherwise unedited.

## 01-t2va-dialogue

Input: `two kids in a treehouse; the older boy says "Do you hear that?" and the girl whispers "It is coming closer". 6 seconds`

```text
integrated_multimodal_description: [Shot 1] Live-action, cinematic, a high-angle medium shot from inside a weathered wooden treehouse, where a Black boy of about ten in a red shirt (S1) leans over the railing and says with a curious tone: <d>[English] Do you hear that?</d> Beside him, a Black girl of about seven in a yellow dress (S2) presses her ear to the wood and whispers: <d>[English] It is coming closer.</d> The camera pans slightly right with small amplitude at slow speed as both children glance toward the forest below, their eyes wide and focused on something unseen.

overall_soundscape: Wind rustles through the tree branches and leaves creak underfoot. Distant animal calls echo faintly through the woods.

non_diegetic_music: A slow acoustic guitar pattern with sparse high-pitched harmonics, gradually increasing in volume.
```

## 02-t2va-multishot

Input: `a chef prepares a dish in a busy kitchen, then serves it to a customer who tastes it. 12 seconds`

```text
integrated_multimodal_description: [Shot 1] Live-action, cinematic, a medium shot of a bustling professional kitchen with stainless steel counters and hanging pots. A White European chef in her late thirties, wearing a white chef's coat and a tall toque, stands at a stove and stirs a simmering sauce with a wooden spoon. The camera pushes in with small amplitude at slow speed as she adds a pinch of salt, then lifts the lid to reveal a steaming dish. [Shot 2] At 00:04.500, the camera cuts to a close-up of her hand placing the dish on a white plate and drizzling a final garnish. [Shot 3] At 00:09.000, the camera cuts to a medium shot of a customer, a Black woman in her early forties wearing a gray blazer, seated at a table. The chef places the plate before her, and the customer picks up her fork, takes a bite, and closes her eyes with a satisfied sigh while the camera holds a static shot.

overall_soundscape: Sizzling pans, clinking cutlery, and the chef's knife scraping against a cutting board. The customer's fork scrapes the plate and her lips pop with a soft crunch.

non_diegetic_music: A steady acoustic guitar pattern with light brushed snare, gradually increasing in volume and fading out at the end.
```

Generated video with Minimax LLM:


https://github.com/user-attachments/assets/ad00564b-919b-4131-aa1a-2a2ca54d6e34



## 03-t2va-dialogue-multishot

Input: `a girl and her grandfather fish from a pier; she says "I got one!" and he laughs and says "Careful, hold on tight." 12 seconds`

```text
integrated_multimodal_description: [Shot 1] Live-action, cinematic, a medium shot of a wooden pier at dawn, where a White European girl of about eight with a red cap and striped shirt (S1) stands beside her grandfather, a White European man in his late seventies with a fishing vest and a salt-streaked beard (S2). The camera pushes in with small amplitude at slow speed as she pulls a small silver fish from the water and shouts with excitement: <d>[English] I got one!</d> [Shot 2] At 00:04.500, the camera cuts to a close-up of her grandfather's face as he laughs, eyes crinkled, and says with a warm, gravelly voice: <d>[English] Careful, hold on tight.</d> The camera pulls back with small amplitude at slow speed as he adjusts her grip on the fishing rod and she grins, the fish still dangling in the line. [Shot 3] At 00:09.000, the camera cuts to a wide shot as the grandfather lifts the rod and casts the line back into the water, the girl watching with wide eyes as the line disappears into the waves.

overall_soundscape: Water splashes against the pier and the rod clicks as it's reeled. The girl's breath is sharp with excitement and the grandfather's laugh is loud and warm.

non_diegetic_music: A gentle acoustic guitar pattern with light fingerpicking, gradually increasing in volume and fading out at the end.
```

## 04-i2va-cats

Reference image (first frame):

![cats](images/08-counts.png)

Input: `the cats wake up one by one`

```text
For the target video, at 0.00 seconds into the target video, <Picture 1> (from [Shot 1]) is fully referenced.

integrated_multimodal_description: [Shot 1] Live-action, cinematic, a wide shot of a sunlit living room with warm golden light streaming through a window. Three cats are asleep on a blue couch: a tabby on the left, a black cat in the middle, and a calico on the right. The camera holds a static shot as the tabby cat stirs first, stretching slowly with a soft yawn. Then the black cat opens its eyes and stretches, followed by the calico, who rolls onto its back and stretches with a contented purr. The camera pans slightly right with small amplitude at slow speed to follow the calico's movement as it settles back down, and the shot ends with all three cats awake and resting on the couch.

overall_soundscape: Gentle sunlight filters through the window. Soft purring and a light rustle of fur as the cats stretch.

non_diegetic_music: A soft acoustic guitar pattern with light fingerpicking, fading out at the end.
```

Generated video with Minimax LLM:



https://github.com/user-attachments/assets/333d2cfd-507a-4ca1-baad-d73cf17d54f9



## 05-i2va-sign

Reference image (first frame):

![sign](images/12-visible-text-caption.png)

Input: `the camera slowly pushes in as a breeze moves the sign, 5 seconds`

```text
For the target video, at 0.00 seconds into the target video, <Picture 1> (from [Shot 1]) is fully referenced.

integrated_multimodal_description: [Shot 1] Live-action, cinematic, a static medium shot of a wooden sign hanging from a wrought-iron bracket outside a warm-toned wooden storefront. The sign reads "Mill & Marrow" in carved dark letters. A gentle breeze lifts the sign slightly, causing the wood to creak softly. The camera pushes in with small amplitude at slow speed, focusing on the texture of the wood and the shadow of the bracket. The background remains softly blurred, with warm interior lights visible through the glass door and a brass doorbell hanging below. The shot holds steady as the breeze continues to sway the sign gently.

overall_soundscape: A soft breeze rustles the sign and a faint creaking sound comes from the wood. The doorbell glints slightly as it catches the light.

non_diegetic_music: A single warm acoustic guitar note with a soft, sustained reverb, fading out at the end.
```

Generated video with Minimax LLM:


https://github.com/user-attachments/assets/cc42a1c0-652c-4c7a-95c0-d28f8f89d5dd



## 06-l2va-cats

Reference image (last frame):

Input: `ending frame of the video: the cats are all asleep, the earlier part shows them settling down. 6 seconds`

```text
How the reference pictures align with the target video — <Picture 1> (from [Shot 1]) aligns with the 6.00-second mark of the target video.

integrated_multimodal_description: [Shot 1] Live-action, cinematic, a wide shot of a sunlit living room with a blue sofa bathed in golden morning light. Three cats — a tabby, a black cat, and a calico — are initially awake, stretching and settling onto the sofa. The camera holds a static shot as the tabby curls up on the left, the black cat settles in the middle, and the calico rolls onto its back on the right. The shot transitions to a slow fade as all three cats drift into sleep, their breathing slow and steady, and the final frame settles on them all asleep exactly as in <Picture 1>.

overall_soundscape: Gentle sunlight filters through the curtains. A faint purr from the calico and soft breathing from the black cat. A distant bird chirps outside the window.

non_diegetic_music: A soft acoustic guitar pattern with sparse piano notes, fading out at the end.
```

Generated video with Minimax LLM:



https://github.com/user-attachments/assets/052cdc7a-c412-4676-b142-eba43e35957d



## Known issues seen in testing

These are the slips observed across repeated runs. They are not in the examples above.

- A person in a reference image was described only as "a young woman", without an age or ethnicity.
- The first-frame alignment line was missing for the cafe-woman image in 2 of 2 runs with the final prompt, while it was present for the cats and the sign.
- A speaker who first appears in a later shot sometimes had no speaker ID.
- Speech that the user never asked for was added to the harbor-market scene.
- A silent character was given a speaker ID, and Tokyo scenes often got people who are not Japanese.
