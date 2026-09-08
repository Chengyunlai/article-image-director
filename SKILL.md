---
name: article-image-director
description: Turn an article, tutorial, or technical explainer into a coherent series of content-faithful visuals, choosing among explanatory diagrams, comparisons, code-relationship graphics, and editorial illustrations. Use when the user wants images that clarify specific passages rather than generic decoration or a single cover image.
---

# Article Image Director

Turn an article into a small visual explanation system. Select passages that benefit from visualization, decide what readers must understand from each image, choose the appropriate visual form, define a shared visual language, then generate each image as a separate asset.

The primary goal is fidelity to the article. Style and series consistency support comprehension; they must not replace concrete subject matter with generic metaphors.

## Inputs and defaults

- Accept full text, Markdown, or a readable URL.
- Treat user-supplied style references and recurring-character images as optional overrides.
- Default to 4 images, 4:3 landscape, low-to-medium visual density, and only the minimum text needed for comprehension.
- Treat the requested count, ratio, text policy, and supplied references as authoritative.

When the user does not provide overrides, use the bundled defaults:

- Visual style references: `assets/style-reference-01.png` through `assets/style-reference-04.png`.
- Default protagonist: `assets/default-protagonist.png`.

The bundled protagonist is a reusable narrative guide, not a required subject in every frame. Replace it when the user supplies a character, asks for another protagonist, or when the article clearly benefits from a different person, profession, age presentation, creature, or non-human guide. Preserve the new protagonist consistently across the series.

If a URL or style reference cannot be read, say so. Do not invent article details or claim to have analyzed an unavailable reference. Ask for pasted content only when the missing source prevents useful work.

## Editorial planning

Before generating, summarize the article as:

```yaml
ARTICLE_BRIEF:
  thesis: "..."
  audience: "..."
  tone: "..."
  emotional_arc: ["...", "...", "..."]
  key_metaphors: ["..."]
```

Choose visual beats by explanatory value, not by paragraph count. Useful roles include overview, mechanism, invariant, failure mode, comparison, tradeoff, and conclusion. Each image needs one dominant communication goal; supporting details are allowed only when they reinforce that goal.

For every candidate image, identify:

1. The exact passage or section it belongs after.
2. The article-specific entities that must appear, such as named functions, components, actors, states, or values.
3. The relationship the reader must see, such as call order, containment, transformation, propagation, contrast, or causality.
4. What would make the image misleading or so generic that it could illustrate an unrelated article.

Reject an image concept if its prompt could still work after replacing the article's topic with a different subject. Revise it until article-specific entities and relationships are visible.

Build a storyboard with one entry per requested asset:

```yaml
- id: image_01
  role: hook
  article_point: "..."
  reader_takeaway: "..."
  visual_metaphor: "..."
  visual_form: "technical diagram | comparison | code-relationship graphic | conceptual illustration"
  required_entities: ["..."]
  required_relationships: ["..."]
  scene: "..."
  composition: "..."
  continuity: "..."
  placement: "article section or paragraph"
  avoid: ["collage", "tiny text", "decorative clutter"]
```

Make the sequence progress as a visual essay—for example, tension → mechanism → tradeoff → resolution—without forcing this exact structure when the article suggests a better one.

## Style and continuity

Create a `STYLE_LOCK` before generation and reuse it in every prompt. Include only traits that affect the result:

```yaml
STYLE_LOCK:
  medium: "..."
  line_and_shape: "..."
  palette: "..."
  lighting: "..."
  background: "..."
  composition: "..."
  texture: "..."
  whitespace: "..."
  text_policy: "minimum labels required for comprehension"
```

Unless the user requests another direction, derive the lock from the bundled style references. Preserve their shared visual grammar rather than copying any single composition:

- warm, softly lit home-studio or learning environment;
- polished anime-influenced editorial illustration with painterly softness;
- warm cream, wood brown, muted olive, pale sage, amber-gold highlights, and restrained Python blue;
- a human learner observing a large wall-mounted explanatory board;
- technical content integrated into paper cards, boards, ribbons, transparent tubes, arrows, and simple physical metaphors;
- shallow depth, sunlit atmosphere, tactile paper and wood textures, rounded forms, calm optimistic mood;
- clear hierarchy with the instructional graphic occupying most of the upper or central field and the protagonist acting as the reader's point of view;
- diagrams that remain specific to the article rather than becoming generic room decoration.

Do not copy incidental phrases, book titles, wall notes, logos, or exact layouts from the references. Reconstruct the visual language around the current article's real content.

When references are supplied, extract transferable traits such as medium, palette behavior, shape language, texture, composition, and mood. Do not reproduce a reference literally or imitate a living artist by name.

Create `CHARACTER_LOCK` whenever a recurring protagonist is used. If no protagonist override is supplied, inspect `assets/default-protagonist.png` and preserve these core cues: youthful male-presenting learner, tousled dark-brown hair, warm fair skin, large dark eyes, soft rounded anime facial design, oversized dark olive crew-neck sweatshirt over a narrow white collar, loose dark trousers, white sneakers, approachable and curious demeanor.

Keep identity cues stable, but change pose, camera angle, expression, hand action, and placement to serve each image. The protagonist may sit at a desk, stand at a board, point, inspect with a magnifying glass, take notes, compare alternatives, or appear in profile/back view. Do not force a full-body pose when an over-the-shoulder or cropped view explains the content better.

The protagonist supports the technical idea and should not cover essential labels or relationships. Omit the protagonist from a frame when their presence would materially reduce clarity, unless the user explicitly requires the character in every image.

For mobile readability, use a clear focal hierarchy, few object groups, generous negative space, and labels large enough to read. These are defaults, not reasons to omit article-critical entities or relationships.

## Choose the visual form

Select the form from the content rather than applying one illustration style to everything:

- Use a **technical diagram** for sequence, data flow, nesting, propagation, or architecture.
- Use a **comparison** for before/after behavior, correct/incorrect outcomes, costs, or alternatives.
- Use a **code-relationship graphic** when names or syntax such as a decorator, wrapper, return value, or exception are essential to understanding.
- Use a **conceptual illustration** for the opening thesis, emotional tension, or conclusion when exact mechanics are not the main point.

When using the bundled style, embed these forms inside the illustrated learning scene instead of switching to a sterile standalone infographic. The technical board remains the explanatory core; the room and protagonist provide warmth, attention direction, and narrative continuity.

Technical visuals may contain short labels copied exactly from the article. Prefer real identifiers over invented symbols. Do not render paragraphs, long code samples, or decorative pseudo-code. When the image model is likely to corrupt important text, simplify to a few large labels; if exact text remains critical, recommend a deterministic diagram instead of pretending the generated text is reliable.

## Prompt and generation

Generate each storyboard frame independently with the available image-generation capability. Never combine the requested series into a single contact sheet or collage.

For every generation call, include the relevant images as actual visual inputs rather than relying only on prose:

- Include one or two bundled style references that best match the planned composition; rotate references across the series when useful.
- Include `assets/default-protagonist.png` whenever the default protagonist appears.
- If the user supplies a replacement protagonist, use that image instead and stop using the bundled protagonist for that series.
- Label the role of each input in the prompt: `style reference` or `character identity reference`. A style reference controls rendering, atmosphere, materials, and layout language; a character reference controls identity and clothing cues, not the exact pose or background.
- Use the smallest reference set that contains everything needed for the frame. Do not treat an earlier finished article image as the identity source when the dedicated protagonist reference is available.

Use this prompt scaffold and omit empty sections:

```text
Use case: illustration-story
Asset type: editorial article illustration
Primary request: Explain this article-specific point: {reader_takeaway}
Scene/backdrop: {scene}
Subject: {required_entities and dominant subject}
Style/medium: {STYLE_LOCK}
Continuity: {CHARACTER_LOCK, current pose/action, or recurring object cues}
Composition/framing: {composition}; {aspect_ratio}; mobile-readable focal hierarchy
Lighting/mood: {lighting and emotional tone}
Color palette: {palette}
Text: {minimum exact labels, or no visible text when labels add no value}
Constraints: clearly show {required_relationships}; preserve the reference-informed warm illustrated learning environment; standalone image; preserve series visual language and protagonist identity; mobile-readable hierarchy
Avoid: sterile standalone corporate infographic unless requested, generic technology symbolism, unrelated decoration, copied reference wording, collage, watermark, logos, fake code, illegible labels
```

Do not make later prompts progressively denser. For technical articles, preserve real entities and causal structure. Metaphors may support the explanation but must not substitute for the mechanism. If exact labels are essential, request only the minimum verbatim text and verify it carefully.

## Visual QA

Inspect every result against its assigned job and the series lock:

- The main idea should read quickly at article width and on mobile.
- The image should match its article point without introducing a competing idea.
- A reader should be able to name the relevant article concept from the image and caption together.
- Every required entity and relationship should be present and unambiguous.
- The image should not be reusable unchanged for an unrelated article.
- Check accidental text, unexpected logos, clutter, anatomy, repeated compositions, and style or character drift.
- Regenerate only a failed frame, using one targeted correction at a time.

## Deliverables

Return the article brief, style lock, storyboard with placement notes, each generated image as a separate asset, and the final prompt set. Save all requested final assets in the workspace when they are intended for publication, and report their paths.
