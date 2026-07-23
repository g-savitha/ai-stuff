---
name: social-media-image-generator
description: Create beginner-friendly social-media images that explain one technical concept as a minimalist hand-drawn comic infographic. Use when the user asks to turn a technical topic, question, source, or lesson into an educational Instagram portrait, square, or landscape image; requests a visual explainer with metaphors and accurate engineering vocabulary; or invokes /social-media-image-generator.
---

# Social Media Image Generator

## Goal

Create a validated comic infographic that teaches one technical mental model in under two minutes. Optimize for comprehension, technical accuracy, and one memorable takeaway.

Read [references/visual-specification.md](references/visual-specification.md) before planning or generating the image.

## Parse the Request

Treat non-flag text as the topic. Accept source material and factual constraints when supplied.

Support:

- `--portrait`: 1080 × 1350, 4:5; default
- `--square`: 1080 × 1080, 1:1
- `--landscape`: 1080 × 566, 1.91:1
- `--brand`: include a small Instagram icon beside `@explainedbysavi` and a small web/globe icon beside `gsavitha.in`

Ask only when competing interpretations would materially change the explanation. Otherwise, select a reasonable beginner-friendly interpretation and proceed.

## Workflow

1. Narrow broad topics to one useful beginner question using the 80/20 rule.
2. Verify essential claims from supplied sources. Research current, niche, disputed, or unfamiliar claims when reliable retrieval is available. If verification is unavailable, narrow the content to well-supported fundamentals and keep uncertainty out of the image.
3. Plan the central question, minimum essential explanation, exact technical terms, one coherent physical metaphor, causal panel sequence, concise panel copy, and final takeaway.
4. Compose specifically for the selected aspect ratio. Reduce panels or secondary detail before reducing readability.
5. Generate the infographic with an available image-generation tool. Do not return only a generation prompt when direct image generation is available.
6. Inspect the generated image at sufficient detail. Check spelling, legibility, sequence, clipping, overlap, metaphor consistency, colour meaning, and branding behavior. With `--brand`, verify that each icon is recognizable, paired with the correct text, and rendered in the infographic's hand-drawn style.
7. Revise and regenerate once when a material defect is visible. If an issue remains, return the best result and name the limitation briefly.

Do not claim to have completed checks that were not performed.

## Output

When image generation is available, return:

1. The completed image
2. A concise caption naming the central question, technical concept, final takeaway, and any unresolved limitation

When image generation is unavailable, return:

1. The selected central question
2. The chosen metaphor
3. A numbered storyboard with exact panel copy
4. A copy-ready image-generation prompt
5. The final takeaway

Do not expose lengthy internal reasoning.

## Completion Bar

Finish only when one question is taught, terminology is accurate, the metaphor is coherent, copy is minimal and readable, the layout fits the canvas, the takeaway is memorable and accurate, branding follows the flag exactly, and the image has been inspected when inspection is available.
