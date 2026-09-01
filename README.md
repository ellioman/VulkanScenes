# Scenes/

A shareable collection of sample/test scene JSON content, plus the small
models/textures those scenes reference (`Assets/models/`,
`Assets/textures/`). This directory contains no compiled code and no build
target (`.h`/`.cpp`/`.vcxproj`/`.props`) — only scene content, its assets,
and this documentation.

Large, not-committable asset bundles (e.g. Sponza's glTF set) are
deliberately NOT here — they stay project-local (gitignored, fetched
on-demand via `dev.py fetch-assets`) since committing them would bloat
every consumer's clone whether or not they touch that scene. Only the
small (a few MB, no LFS needed), genuinely shareable assets live under
`Scenes/Assets/`.

Any project that depends on `Renderer/` can load a scene from here by
pointing `SCENE_PATH` (or an equivalent explicit path) at a `.json` file
under `Scenes/`, the same way it loads a scene from its own content
directory. `SceneLoader::load` / `loadOrDefault` don't distinguish
between the two locations — there is no `Scenes/`-specific code path.

This repository is a git submodule (`https://github.com/ellioman/VulkanScenes`)
checked out at `Scenes/` in each host project. VulkanShadowBible's own
scene content (`sponza.json`, `default.json`, and the per-algorithm test
scenes) lives here rather than in a project-local directory, so it can be
reused unchanged by other projects that depend on `Renderer/`.

`spinning_fan_demo.json`, `light_test_directional.json`,
`light_test_spot.json`, and `light_test_point.json` are light-type
verification scenes. The fan demo authors all three light types
(directional/spot/point), only one enabled by default, switchable via each
light's `Enabled` checkbox in the debug UI — useful for visually comparing
how a moving shadow looks under each light type. The three `light_test_*`
scenes each author exactly one light type (the others present but
disabled, since `directionalLight` is a required top-level field) reusing
`default.json`'s grid-of-boxes geometry, for scripted per-algorithm
verification (`dev.py scene <path>`) with no other light type's
contribution to confound the result.

`gltf_scene_import_test.json` exercises the `"gltfScene"` scene-JSON block
(openspec `gltf-2-0-asset-import`): its camera and all three light types
come from `Assets/models/gltf_scene_import_test.gltf`'s own scene graph, not
this JSON's `"camera"`/`"directionalLight"` fields, which are present only
because the schema still requires them and are overridden/supplemented at
load time. Regenerate the glTF fixture with
`python dev.py make-gltf-scene-import-test-asset`; regression-covered by
`python dev.py gltf-scene-import-smoke`.

For the scene-loading library itself (`Scene`, `SceneLoader`, `SceneObject`,
the JSON schema) see `Renderer/README.md`, which documents the
library/content boundary. `Renderer/Scene/` provides the loader, types, and
schema; `Scenes/` (this directory) and each host project's own content
directory both supply scene content on the same terms.
