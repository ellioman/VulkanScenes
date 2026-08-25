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

For the scene-loading library itself (`Scene`, `SceneLoader`, `SceneObject`,
the JSON schema) see `Renderer/README.md`, which documents the
library/content boundary. `Renderer/Scene/` provides the loader, types, and
schema; `Scenes/` (this directory) and each host project's own content
directory both supply scene content on the same terms.
