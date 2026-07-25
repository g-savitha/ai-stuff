---
name: gpt-image-prompt-generator
description: Create and refine production-ready prompts for OpenAI GPT image generation and editing. Use when a user wants an image prompt, ad, logo, infographic, photorealistic scene, text-in-image layout, translation/edit, compositing instruction, model/quality/size recommendation, or an iterative prompt revision.
---

# GPT Image Prompt Generator

Turn an informal visual brief into a clear, controllable prompt for GPT image models. Produce the prompt first, then include only the settings and brief rationale needed to use it.

## Workflow

1. Extract the intent: generation or edit, intended use, audience, subject, environment, visual medium, aspect ratio, required text, references, and must-preserve elements.
2. Fill gaps with sensible assumptions. Ask a concise question only when an omission would materially change the image; otherwise state the assumption.
3. Compose the prompt in this order: scene/background → subject → action/pose → composition/camera → lighting/color/mood → materials/style → literal text and typography → constraints and exclusions.
4. Make important requirements explicit. For edits, say “change only X” and list what must remain unchanged. For multi-image work, identify inputs by index and describe how they interact.
5. Recommend settings: default to `gpt-image-2`; use `quality: low` for speed/volume, `medium` for general production, and `high` for dense text, infographics, close-up portraits, identity-sensitive edits, or maximum fidelity. Recommend `1024x1024`, `1024x1536`, or `1536x1024` unless the brief calls for another valid size.
6. End with a small iteration plan containing single-change follow-ups, not an overloaded first prompt.

## Prompt rules

- Prefer concrete nouns and visible details over abstract adjectives. Specify framing, viewpoint, scale, gaze, object interactions, materials, texture, and negative space when they matter.
- For photorealism, explicitly use “photorealistic” and describe natural imperfections, lighting, and photographic feel.
- Put literal in-image copy in quotation marks or ALL CAPS. Specify exact spelling, placement, hierarchy, color, and legibility; request no extra text. Use `medium` or `high` for small or dense typography.
- Preserve brand elements, geometry, layout, identity, and surrounding objects explicitly when editing. Repeat critical invariants on each follow-up.
- Use exclusions selectively: no watermark, no extra text, no unrelated logos, and no unintended changes when relevant.
- Do not invent a brand name, slogan, identity, source image, or factual detail that the user did not provide; mark assumptions.
- Do not promise pixel-perfect text, exact camera physics, or guaranteed identity preservation. Frame these as targets and recommend iteration when precision matters.

## Output format

Return:

**Prompt**

```text
[ready-to-paste prompt]
```

**Suggested settings**

`model`, `quality`, `size`, and—only when relevant—`n` or input-image handling.

**Assumptions / iteration**

List at most three important assumptions or focused follow-up edits.

## Reference

Load [image-prompting-guide.md](references/image-prompting-guide.md) when detailed model parameters, size constraints, or use-case-specific examples are needed. Use it as guidance, while following the current user brief when it conflicts with stale examples.
