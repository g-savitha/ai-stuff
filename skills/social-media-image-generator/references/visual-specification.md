# Educational Technical Comic Visual Specification

## Success Criteria

The infographic must:

- teach one central concept or mental model;
- use accurate technical terminology;
- pair metaphorical language with the real technical term;
- follow an obvious visual sequence;
- use concise, readable text;
- maintain one coherent visual world;
- contain one memorable final takeaway;
- fit the requested canvas without clipping or overcrowding;
- include branding only when `--brand` is present.

## Scope and Accuracy

Apply the 80/20 rule. Exclude secondary mechanisms, edge cases, history, and adjacent concepts unless they are necessary for the central idea. Do not summarize an entire broad system in one image.

Simplify the explanation, not the terminology. Introduce a technical term together with its metaphor:

```text
Technical Term
(Metaphor)
```

For example:

```text
Self-Attention
(Relevance Voting Room)
```

Treat examples as illustrative rather than mandatory. Invent a better metaphor when it improves accuracy or understanding. Never add unsupported technical claims.

## Teaching Sequence

Build a connected visual story that generally progresses through:

1. A relatable question or confusion
2. The problem shown visually
3. The missing technical idea
4. The idea explained through one metaphor
5. The original problem resolved
6. One memorable “Aha!” takeaway

Use approximately 3–8 panels, adjusted for the canvas and content. Make each panel advance one idea or causal step. Do not begin with a dictionary definition; lead the reader toward the concept through cause and effect.

## Metaphor Rules

Choose one primary visual world, such as a workshop, railway, library, city, factory, control room, kitchen, postal system, or another suitable physical setting.

Use it consistently. Do not:

- switch to an unrelated metaphor midway;
- imply technically false equivalences;
- replace the real technical term with the metaphor;
- add metaphorical elements that do not aid understanding.

The metaphor teaches intuition; the technical term teaches vocabulary.

## Art Direction

Use a minimalist hand-drawn webcomic style:

- pure white `#FFFFFF` background;
- loose black ink lines;
- imperfect boxes and uneven borders;
- rough arrows and sketch-like diagrams;
- expressive stick figures or tiny workers when useful;
- large illustrations with short supporting labels;
- generous whitespace;
- one consistent visual language.

Make the result resemble a senior engineer sketching an explanation on a whiteboard for a curious friend.

Avoid:

- polished corporate illustration;
- photorealism or stock imagery;
- 3D rendering;
- gradients and glossy effects;
- dense paragraphs;
- decorative objects that do not teach;
- unrelated visual styles.

Require every visual element to clarify meaning, direct attention, or reinforce memory.

## Characters and Tone

When characters help, give them an emotion that supports the learning sequence: confusion, curiosity, thought, surprise, excitement, or relief. Prefer a diagram when it communicates more clearly.

Keep the tone friendly, curious, clear, patient, technically credible, approachable but not childish, personal rather than corporate, and simple without becoming simplistic.

## Text

Use concise handwritten-style lettering that remains highly legible.

Prefer:

- 3–15 words per text element;
- short labels;
- speech bubbles;
- sentence fragments;
- one short sentence when necessary.

Avoid paragraphs and repeated explanations. Keep critical terminology prominent and correctly spelled. Minimize text that must be rendered inside the image.

## Layout

Create a clear reading path appropriate to the chosen aspect ratio. Include one strong headline, visually connected sections, arrows where sequence or causality is unclear, sufficient spacing, large primary visuals, and safe margins around text and branding.

Compose for the selected canvas rather than cropping another layout. For compact formats, reduce panel count or supporting detail before reducing legibility.

## Colour

Use mostly black ink with restrained, muted accents:

- blue: information;
- green: success or resolution;
- yellow: insight or emphasis;
- red: problem or failure.

Use colour to communicate meaning, never merely to decorate.

## Branding

When `--brand` is present, include exactly:

- a small hand-drawn Instagram icon immediately beside `@explainedbysavi`;
- a small hand-drawn web/globe icon immediately beside `gsavitha.in`.

Keep both icons simple, recognizable, and consistent with the infographic's loose black-ink style. Treat them as functional labels rather than decorative illustrations. Place both branded items unobtrusively in one corner with adequate spacing and safe margins. Do not substitute emoji, add other logos, or alter the handle or website text. When `--brand` is absent, include neither the icons nor their text.

## Final Takeaway

End with one short, repeatable sentence that expresses the central insight accurately without depending entirely on the metaphor.

Examples such as “Tokens aren’t words” or “Attention is relevance-weighting” demonstrate the desired brevity. Reuse them only when they fit the requested topic.

## Inspection Checklist

Inspect the rendered image for:

- correct headline and panel order;
- correct spelling of technical terms;
- readable text at normal viewing size;
- no clipping or overlap;
- one consistent metaphor;
- meaningful, restrained colour;
- exact branding behavior, including the Instagram icon beside the handle and the web/globe icon beside the website when `--brand` is present;
- one clear and accurate takeaway.
