# Sister-project ideas

A catalogue of scene-types we could build on top of the RayBlob engine
(SDF → raymarched buffer → quantized scan-line hatching → plotter-ready
SVG). Each entry is a self-contained scene; most reduce to:

> *Place N SDF primitives, optionally subtract a few, and let the
> existing hatching + sky/ground/horizon pipeline render it.*

---

## Status of the underlying engines

These are the cross-cutting helpers that unlock most of the scenes
below. **Already implemented** in `index.html`:

- `makeBlobSDFAt(rng, { center, nSpheres, smoothK, spread, verticality, pointiness, scale })` — position-aware blob primitive.
- `sdfUnion(sdfs, smoothK)` — combines N SDFs; smooth-min if `smoothK > 0`, sharp `Math.min` if `0`.
- `sdfSubtract(base, carvers)` — `max(base, -c0, -c1, …)`.
- `makeSphere`, `makeCylinderY`, `makeCapsuleX`, `makeCapsuleZ`.
- `SCENES` registry (extend by adding an entry).

**Currently registered scenes**: `single`, `monoliths`, `arch`,
`lighthouse`, `iceberg`.

Useful primitives we *don't yet have* but several scenes below want:

- `makeBox(cx, cy, cz, hx, hy, hz)` — axis-aligned rounded box.
- `makeTorus(cx, cy, cz, R, r)` — torus.
- `makeConeY` — finite cone (apex up or down).
- `makePlane(normal, d)` — half-space plane.
- Domain ops: `twistY(amount)`, `bendY(amount)`, `mirror(axis)`, `revolve(profileFn)`, `displacementNoise(scale, amp)`.
- A "scene-level" facility for **multiple distinguishable surfaces**
  (e.g. cloud vs rock vs water) so different parts get different hatching
  — currently the buffer only carries `surface ∈ {0=sky, 1=blob, 2=ground}`.

---

## Original list (20 sister-project sketches)

### Standing forms

1. **Monoliths** *(implemented)* — A handful of Stonehenge / Easter-Island monoliths on a barren plain. Tapered-box / tall-blob SDFs at varied heights and small Y rotations, lined up in shallow perspective.
2. **Lighthouse** *(implemented)* — Cylindrical tower + cone roof + a long thin beam SDF carving through the sky hatching (subtract or invert hatch on the beam's path). The beam is the most distinctive visual.
3. **Cacti** — 2–4 saguaros (cylinder + capsule arms) rooted in a sand-dune ground. Verticality and pointiness reused; instead of perspective lines, dune *contour* shading on the ground.
4. **Cathedral** — Interior view: rows of perspective columns + arched ceiling SDF. Convergence to a vanishing point. Hatching on column shafts reads strongly because they're cylindrical.
5. **Bonsai** — A single sculptural tree (cylinder trunk with branching capsules + canopy blob). All the focus on form variation — the artwork is the gesture of the branches.

### Water scenes

6. **Iceberg** *(implemented)* — One ice shape above the waterline, a larger one below, the boundary right at the horizon. Wave-hatched water passes through; the underwater portion is fainter/dotted.
7. **Wave** — A single breaking wave SDF: long curl + foam dashes at the crest. Almost no sky — the wave fills the frame. Engraving-style water lines around the spume.
8. **Whale breach** — Cetacean body emerging from a horizontal water plane, splash dashes near the contact line. Strong silhouette work, body almost solid black.
9. **Sailboat** — Triangular sail + simple hull on a horizon. Tiny subject against enormous sky — practically all of the visual weight is in the sky treatment.
10. **Submerged** — Camera underwater: kelp/seaweed SDF columns reach up to a horizon (the waterline overhead), light streaming down in diagonal hatched beams.

### Atmosphere / sky

11. **Aurora** — Flat tundra ground + curving aurora bands in the sky (parametric ribbons drawn as multi-stroke polylines, no SDF needed for the aurora). Solar gradient at the horizon.
12. **Hot air balloons** — Three or four sphere-on-basket forms at varied depths against a gradient sky. Strings tether to baskets. Verticality re-used inverted: subject mid-frame, not anchored to horizon.
13. **Tornado** — A twisting funnel SDF (smooth-min of vertically-stacked spheres with horizontal jitter) connecting a cloud band to the ground; dust dashes at the base.
14. **Volcano** — Cone with a smoke plume rising into the sky (noise-distorted vertical column, dashed by lightness). The plume is rendered as sky-pattern *plus* SDF.

### Landscape

15. **Mountains** — Three ridge SDFs at three distances. Closest is fully hatched, middle is lighter (fewer crosshatch directions), farthest is just silhouette + atmospheric haze — atmospheric perspective via *hatching density*.
16. **Hilltop tree** — Single rolling hill (low-frequency sinusoidal ground) with one tree at the crest. Extremely calm composition; the whole drawing is about the *space* around the subject.
17. **Pyramid** — Geometric pyramid (triangular silhouette) on a dune-field ground. Most geometric option in the list, would let the gradient/wobbly skies shine.
18. **Cliff face** — Vertical wall close to camera with horizontal sedimentary layer-bands of different hatch directions. A pure texture/material study.

### Constructed / abstract

19. **Skyline** — Row of rectangular building boxes of varied heights in shallow perspective. Each building's "window grid" comes from a stipple field. Urban analog of the Monoliths.
20. **Astronaut on a tiny planet** — Camera close to a small sphere ground (Saint-Exupéry-style); a tiny figure SDF stands on the curve, stars stippled in the sky. The horizon *curves* downward — most novel composition in the set.

---

## New ideas (Universal Rayhatcher–inspired)

Drawn from the URH project gallery, the *Remnants / Travelers /
Symbiosis* series, and the tutorial articles ("Rayhatching a UFO
scene", "Revolving, twisting and transforming SDFs"). These tend to
lean further into **mathematical operators** (twist, bend, revolution,
displacement, recursive substructure) than the first 20.

### Geometric / mathematical

21. **Schotter 3D** — Vera Molnar / Georg Nees homage as Piter Pasma did: a tilted grid of small box SDFs where the rotation amount per cell increases as you scan one direction. Pure ordered-to-disordered gradient. Plotter-style perfect for engraving.
22. **Recursive tower** — A tall column built from copies of itself at progressively smaller scales stacked vertically (like a fractal stupa). Single recursive SDF function applied N times.
23. **Twisted column** — Single tall box / cylinder with a domain-twist operator applied around the Y axis. Hatching wraps around the twist beautifully.
24. **Bent rod** — A cylinder smoothly bent through 90° using a domain-bend operator. Reads as a hook or staff.
25. **Spiral staircase** — Stacked thin discs offset by a small Y rotation per step. Drama from the helix when viewed from below.
26. **Revolution profile** — An arbitrary 2-D profile curve revolved around the Y axis (vase, urn, chess piece, banister). Single SDF op produces a wide family of vessel shapes.
27. **Polyhedron** — A single regular polyhedron (dodecahedron / icosahedron) floating at the centre of frame. Pure shading study — let the hatching read the facets.
28. **Tessellation field** — Identical small SDFs tiled on a hexagonal or square lattice; closer ones rendered fully, distant ones reducing to outline only. Atmospheric perspective via *recursion depth*.
29. **Domain repetition + offset** — Same scene as Tessellation but with a per-cell Y-axis rotation that wobbles around its centre. Generates the "wiggle field" look common in URH.
30. **Floating metaballs** — Several SDF spheres slowly smooth-min'd together at random heights against a sky gradient — the most direct test of the smoothing parameter as the protagonist.

### From the URH gallery (Remnants / Travelers / Symbiosis / etc.)

31. **Cargo ships on rails** *(Industrial Devolution motif)* — Multiple box / capsule forms moving along straight tracks that converge to a vanishing point. The whole scene partly eroded by displacement noise.
32. **Eroded structure** — A monumental form (column, archway) with high-frequency noise *subtracted* from its surface — gives the time-worn / weathered surface character of Pasma's *Remnants* series.
33. **Coral / branching organic** — Recursive branching cylinder/capsule structure growing toward the viewer from a "floor", inside a darkened cave (heavy hatching in the surround). Inspired by *Symbiosis*.
34. **Distorted space-time** — A simple scene (e.g. a row of cubes) but with a global domain-warp noise applied to coordinates, producing rippled / melting versions. The warp is the subject.
35. **UFO over landscape** — Saucer (revolved profile) hovering above a low fractal terrain. Beam (long thin cone or capsule) projecting down. The hatched ground reads as terrain, the saucer reads as a *thing*.
36. **Object on rails** — Two parallel infinite rails converging to a vanishing point, with a single object (cube, sphere) sitting on them. Strong perspective, almost graphic-design composition.
37. **Tower of cargo containers** — Stacked staggered boxes of varying widths with overhangs. Reads as an industrial / brutalist tower. Each container's *visible face* gets its own hatch direction.
38. **Hovering platforms** — Several flat thin discs at varied heights, perfectly horizontal, all overlapping in z. Reads as a Magritte-like floating-island scene.
39. **Mirrored / reflected form** — An object plus its perfect reflection across the horizon plane (the iceberg's logical extreme). Wave hatching at the boundary line.
40. **Twin towers, displaced** — Two tall thin cylindrical towers at left and right, with a domain-displacement so they curve slightly toward / away from each other. The negative space between them carries the composition.

### Atmospheric / textural

41. **Smoke column** — A vertical column of stacked spheres with horizontal jitter scaled by Y (wider as it rises). Hatching density falls off with height — closest thing to "smoke" we can render in line work.
42. **Dust cloud** — A puffy ground-level mass made of many overlapping smooth-min'd spheres, low + wide. Reads as fog, sandstorm, or explosion aftermath.
43. **Rain / falling marks** — Not an SDF at all — just a noise-driven scatter of short diagonal dashes everywhere in the sky region. Combine with any of the standing-form scenes.
44. **Stars / night sky** — Sparse stippling on the sky region (Poisson-disc points of varied lengths). Pairs with the *Astronaut* and *Iceberg* scenes especially well.
45. **Searchlight** — Single thin cone or wedge SDF cutting through the sky from a ground source, rendered as a *light-up* zone (invert the hatching density inside the cone — bright instead of dark).

### Architectural / constructed

46. **Megalith circle (perspective)** — Stonehenge-style ring of vertical stones, with the camera off-axis so the perspective compresses the back of the ring. Pure SDF placement + camera composition.
47. **Stepwell** — A descending series of stepped boxes (like the Chand Baori in India). Mathematical pattern, dramatic shading where each step receives less light.
48. **Doorway** — A heavy box with a tall arch carved out of it; through the arch we see a second scene (another blob, a distant moon). The carver SDF is doing double-duty as a *window*.
49. **Bridge** — Long horizontal capsule supported by two vertical cylinder pylons at known x-positions. Optional carved arches under the deck.
50. **Wireframe / lattice** — A regular lattice of thin cylinders forming a 3-D mesh in space. Most lines come from the lattice itself; sky/ground recede.

### Living / figural

51. **Single tree, gnarled** — One tree but with recursive branching SDF (cylinder + sphere joints) for several levels. The whole image is the silhouette study of a single old organism.
52. **Forest at night** — Multiple `Hilltop tree` instances + stars sky + a low moon. Combines tessellated standing forms with atmospheric.
53. **Animal silhouette** — A simple recognisable creature outline (rabbit, fish, bird) built from 4–8 sphere/capsule SDFs smooth-min'd. Single subject, pure silhouette.
54. **Hand / arm** — A stylised hand: 5 capsules emerging from a palm box, smooth-min'd, the form rising from below the frame. Reads as figure / portrait.

### Surreal / experimental

55. **Floating island** — A blob floating in mid-air, optionally with thin "roots" hanging below. The ground level shifts up so it appears suspended.
56. **Sphere with hole** — Single large sphere with a cylindrical tunnel through it (carver + base). Pasma's tutorial-style geometric puzzle.
57. **Möbius strip / topological** — A revolved-and-twisted half-cylinder profile. Hatching across a non-orientable surface — pure showpiece.
58. **Cube of cubes** — A larger box subdivided into N³ smaller boxes with random ones removed (Menger-sponge-like). Recursive carving of the same primitive.
59. **Liquid drop** — Single tall blob smoothly tapering at the top, slightly wider at the bottom — water-droplet silhouette. Very simple, very effective with subtle hatching.
60. **Cracked sphere** — Sphere with a plane subtracted to reveal a *flat slice*. The inside face shows different hatching from the outer surface — would need the multi-surface infrastructure.

---

## Cross-cutting engines still to build

If we want to comfortably reach the rest of the list, the codebase
still wants:

- **`makeBox` / `makeTorus` / `makeConeY`** — primitive geometry.
- **Domain operators**: `twistY(sdf, amount)`, `bendY(sdf, amount)`, `mirror(sdf, axis)`, `repeat(sdf, period)`, `displace(sdf, noiseFn)`.
- **Profile revolution**: `revolveY(profileFn)` — turns a 2-D distance function (in xy) into a 3-D revolved SDF.
- **Multi-surface buffer**: extend `buf.surface` so cloud/water/highlight surfaces can carry their own hatch parameters. Currently we have rock-vs-ground-vs-sky; many of the scenes above (Iceberg, Smoke, Searchlight, Cracked sphere) want at least one more category.

Each engine unlocks roughly **5–10** of the entries above.

---

## Sources

- [Universal Rayhatcher (fxhash project)](https://www.fxhash.xyz/generative/slug/universal-rayhatcher)
- [Piter Pasma — Rayhatching Evolution](https://www.fxhash.xyz/article/rayhatching-evolution)
- [Piter Pasma — Getting Started with Universal Rayhatcher](https://www.fxhash.xyz/article/getting-started-with-universal-rayhatcher)
- [Piter Pasma — Rayhatching a UFO scene](https://www.fxhash.xyz/article/rayhatching-a-ufo-scene)
- [Piter Pasma — Revolving, twisting and transforming SDFs](https://www.fxhash.xyz/article/revolving-twisting-and-transforming-sdfs!)
- [Industrial Devolution (fxhash project)](https://www.fxhash.xyz/generative/slug/industrial-devolution)
- [Monokai — Art inside the Universal Rayhatcher algorithm](https://monokai.com/work/universal-rayhatcher)
- [Piter Pasma — Schotter 3D (Artsy)](https://www.artsy.net/artwork/piter-pasma-schotter-3d)
- [Gazelli Art House — First vector based raymarcher output](https://art.gazelliarthouse.com/products/first-vector-based-raymarcher-output)
- [Piter Pasma — fxhash profile](https://www.fxhash.xyz/u/Piter%20Pasma)
- [paolocurtoni.com — Universal Rayhatcher #231/#192](https://www.paolocurtoni.com/portfolio/universal-rayhatcher/)
