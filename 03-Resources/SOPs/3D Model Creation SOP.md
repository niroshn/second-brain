# 3D Model Creation SOP

Applies to [[Figuro 3D Modeling]] — the figurine reference generation and quality-control pipeline.

## 1. Purpose

The purpose of this SOP is to ensure every custom figurine is created with a high level of similarity to the customer's original photo.

The most important requirements are:
- Keep the character/person highly similar to the original photo.
- Preserve facial features and identity.
- Correctly reproduce clothing, hairstyle, pose, and important details.
- Create each character separately when there is more than one person.
- Regenerate images whenever the result is not sufficiently similar to the original.
- Maintain consistent quality throughout the entire modeling process.

## 2. Initial Photo Assessment

Before generating anything, carefully inspect the customer's original image.

Check:
- Number of people/characters
- Face visibility
- Body position and pose
- Clothing
- Hair style
- Accessories
- Shoes
- Important objects
- Relative height and proportions
- Image quality
- Lighting
- Background

### Important
Do not start generating immediately. First understand exactly what needs to be reproduced.

## 3. Multiple Characters in One Image

If the customer's image contains more than one character/person, each character must be isolated before generating the models.

### Process
1. Upload the original image into the approved image-editing/isolation tool.
2. Use the PrintApp tools to isolate each individual character.
3. Create a separate image for each character.
4. Check that the entire body of each character has been isolated correctly.
5. Make sure important clothing, accessories, shoes, hair, and other details have not been accidentally removed.

### Example
If there are 3 people in the original photo:

Original Image → Character 1 → Character 2 → Character 3

Each character should then be processed individually.

## 4. Generate Individual Character Images

After isolation, generate a separate figurine reference image for each individual character.

Do not generate all characters together when creating the individual models.

For each character:
- Use the isolated character image.
- Generate the figurine/reference image.
- Compare the generated result against the original character.
- Check the face, hairstyle, clothing, pose, proportions, and accessories.
- Regenerate if the result is not sufficiently similar.

## 5. Face Detail Generation

For standard figurines, the face is one of the most important areas.

For every individual character, create a separate high-detail face/figurine reference.

### Process
1. Take a close-up screenshot or crop the isolated character's face.
2. Make sure the face is large and clearly visible.
3. Use this close-up reference to generate a higher-detail face figurine/reference image.
4. Compare it directly with the customer's original face.
5. Check:
   - Face shape
   - Eyes
   - Eyebrows
   - Nose
   - Mouth
   - Hairline
   - Facial proportions
   - Distinctive features
6. Regenerate if the face looks incorrect or substantially different.

### Key Principle
The face should be treated as a separate quality-control step.

Do not rely only on a full-body generation when the face needs higher detail.

## 6. Similarity Check

After every image generation, compare the generated image with the original reference.

Ask:

**Face**
- Does the person look like the original person?
- Are the major facial characteristics preserved?
- Are the eyes, nose, mouth, and face shape reasonably accurate?

**Hair**
- Is the hairstyle similar?
- Is the hair length and general shape correct?

**Clothing**
- Is the clothing the same or sufficiently similar?
- Are important patterns, colors, logos, or accessories preserved?

**Body & Pose**
- Is the body position correct?
- Is the pose similar?
- Are proportions reasonable?

**Accessories**
- Glasses
- Watches
- Jewelry
- Bags
- Hats
- Shoes
- Other distinctive items

**Overall**
- Does the generated character immediately resemble the person in the customer's original photograph?

If the answer is No, regenerate.

## 7. Regeneration Rule

Do not accept an image simply because it looks visually good.

The generated image must be close and similar to the original customer photo.

If there are major differences:

Reject → Adjust prompt/reference → Regenerate → Compare again

Continue until an acceptable result is achieved.

Do not prioritize:
- Beautiful AI-generated faces
- Generic attractive people
- Artistic interpretation
- Different poses
- Different clothing

Prioritize:
- Similarity to the customer's original photograph.

## 8. Keychain Figurine SOP

Keychain figurines follow a slightly different process.

### Face Generation
A separate face generation is normally not required for keychain figurines.

However:
- If the generated face looks odd, distorted, or significantly different from the original, regenerate it.
- Do not accept a poor-quality face just because the product is a keychain.

## 9. Remove the Keychain From Reference Images

When generating the keychain figurine reference:
- The keychain hardware itself should NOT appear in the character image/reference.
- The final model should represent the figurine/character, not the metal keyring or attachment.

### Process
1. Check the generated image.
2. If a keyring, chain, loop, or other keychain hardware appears:
   - Remove it using an appropriate image-editing tool.
   - You may use Gemini Nano Banana or another suitable image-editing model for this task.
3. Check that the character itself has not been altered during the edit.
4. Continue only after the keychain hardware has been removed.

## 10. Image Editing Quality Control

When using AI editing tools, always verify:
- Face has not changed.
- Hair has not changed.
- Clothing has not changed.
- Body proportions have not changed.
- Accessories have not disappeared.
- Important details have not been invented.
- Background changes have not affected the character.

AI editing should only make the required correction.

Do not allow unnecessary changes to the customer representation.

## 11. Multiple Character Final Check

For orders containing multiple people, before moving to final modeling, confirm:
- Every character has been isolated.
- Each character has an individual reference image.
- Each character has been reviewed separately.
- Face detail has been generated for each standard figurine character.
- Clothing is correctly represented.
- Pose is correct.
- Accessories are preserved.
- Characters remain recognizable compared with the original photograph.
- No unwanted person/object has been included.

## 12. Final Approval Checklist

Before sending the reference to the 3D modeling/printing stage:

**Character**
- Correct number of characters
- Correct character separation
- Correct pose
- Correct body proportions
- Correct clothing
- Correct hairstyle
- Correct accessories

**Face**
- Face resembles original
- Facial proportions are acceptable
- No obvious AI distortion
- No strange eyes/teeth/mouth
- No unintended facial changes

**Image**
- Original photo compared side-by-side
- Generated image is sufficiently similar
- No unnecessary AI modifications
- No unwanted objects
- Keychain hardware removed where applicable

### Final Decision
If the reference is not sufficiently similar to the original → regenerate.

Do not send an obviously incorrect reference to the modeling team.

## 13. Golden Rule

ORIGINAL PHOTO → ISOLATE → GENERATE → FACE DETAIL → COMPARE → REGENERATE IF NEEDED → FINAL APPROVAL → 3D MODEL

The goal is not to create the most beautiful AI image.

The goal is to create a reference that allows the 3D model to be as close as reasonably possible to the customer's original photograph.

Similarity comes first. Quality comes second. Artistic interpretation comes last.
