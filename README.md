# Scenes/

A shareable collection of sample/test scene JSON content. This directory
contains no compiled code and no build target (`.h`/`.cpp`/`.vcxproj`/
`.props`) — only scene content and this documentation.

Any project that depends on `Renderer/` can load a scene from here by
pointing `SCENE_PATH` (or an equivalent explicit path) at a `.json` file
under `Scenes/`, the same way it loads a scene from its own content
directory (e.g. this project's `Assets/Scenes/`). `SceneLoader::load` /
`loadOrDefault` don't distinguish between the two locations — there is no
`Scenes/`-specific code path.

For the scene-loading library itself (`Scene`, `SceneLoader`, `SceneObject`,
the JSON schema) see `Renderer/README.md`, which documents the
library/content boundary. `Renderer/Scene/` provides the loader, types, and
schema; `Scenes/` (this directory) and each host project's own content
directory both supply scene content on the same terms.
