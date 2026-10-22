---
title: Hedra is a 3D open world game I wrote from scratch while in highschool
link: https://github.com/maxilevi/project-hedra/tree/pre-ai
source: hnrss-org
published: 2026-10-22T18:44:36Z
updated: 2026-10-22T18:44:36Z
first_seen: 2026-10-22T23:57:38.352236389Z
authors:
- maxilevi
content: extracted
html: 2026-10-22-hedra-is-a-3d-open-world-game-i-wrote-from-scratch-while-in.html
remote_preview:
  url: https://opengraph.githubassets.com/7431a1fd9233b831feed3c3a59bdcc04c1c5e2909d7e3f64e425a7fdf6c54fda/maxilevi/project-hedra
  alt: Hedra is a 3D open world game I wrote from scratch while in highschool. The entire codebase is around +100k LOC - maxilevi/project-hedra
---

[![](https://github.com/maxilevi/project-hedra/actions/workflows/main.yml/badge.svg)](https://github.com/maxilevi/project-hedra/actions/workflows/main.yml/badge.svg) [![](https://github.com/maxilevi/project-hedra/actions/workflows/deploy.yml/badge.svg)](https://github.com/maxilevi/project-hedra/actions/workflows/deploy.yml/badge.svg)

[Project Hedra](https://store.steampowered.com/app/1009960/) is an ambitious open-world, low-poly RPG. Choose your class, explore endless procedurally generated worlds, unlock different skills and complete procedurally generated quests.

[![](https://camo.githubusercontent.com/ae72be1dd477d1f5acbae14f1f03f7d856b2a995b811a10aeba8392780b5e3cc/68747470733a2f2f63646e2e616b616d61692e737465616d7374617469632e636f6d2f737465616d2f617070732f313030393936302f73735f346431313030376631356364306231623566643130653563333436343238316230323064353861642e6a70673f743d31363735373737383939)](https://camo.githubusercontent.com/ae72be1dd477d1f5acbae14f1f03f7d856b2a995b811a10aeba8392780b5e3cc/68747470733a2f2f63646e2e616b616d61692e737465616d7374617469632e636f6d2f737465616d2f617070732f313030393936302f73735f346431313030376631356364306231623566643130653563333436343238316230323064353861642e6a70673f743d31363735373737383939)

More screenshots at ([Screenshots](https://github.com/maxilevi/project-hedra/tree/pre-ai#Screenshots))

## Timeline/Context

I started this project when I was in highschool (around 15 yo) in order to learn how OpenGL worked.

The scope creep quickly arrived and after a few years it turned into a fullfledged 3D game and game engine. The codebase has around 100k LOC and the game has first class support for Linux and Windows.

## Features

The million lines of code are not in vain. The game and engine are both full of features :)

### Game Features

- Beatiful procedurally generated worlds
- Quests and bosses
- 100+ items with rarities system
- Skills and skill tree system with N+ skills
- 4 different classes with 2 specializations each ()
- Procedurally placed structures generated dynamically
- Fishing, crafting, etc
- Trading and NPCs
- Companions and pets
- Beatiful soundtrack
- Navigation, maps
- Different dimensions
- More stuff I don't recall :)

### Engine Features

- Fully fledged renderer using modern OpenGL
- Skinned mesh rendering
- Support for loading and playing generic animations
- Collada (.dae) parser
- Static mesh rendering
- Screen space reflections
- Screen space ambient occlusion
- Bloom, FXAA & MSAA
- Modding support
- CI for automatically deploying to a Steam beta branch
- Localization support
- Sound support using OpenAL
- Python scripting support
- Module system for loading data oriented objects
- Steam integration
- Fully fledged UI & controls system (dropdowns, buttons, etc.) written from raw OpenGL
- Dynamic shader system for reloading during runtime
- Frustum culling, occlusion culling,
- Mesh simplification for runtime level of detail (LOD)
- Skybox support
- Shadows
- 3D trails renderering support
- Particle system using object instancing in opengl
- bulletphysics integration
- Chunk system for infinite worlds support
- .ply parsing support
- Dynamic meshing support using MarchingCubes
- Custom implementation of basic C# primitives and allocators (like Lists) for supporting stack allocated lists and minimizing the automatic runtime allocations (and thus the GC).
- 1000+ plus test cases
- Framework for mocking game components and doing integration testing on games
- Image and asset compression for faster loading and smaller size
- Entity component system
- More stuff I don't recall :)

## Interesting bits

- [WorldRenderer.cs](https://github.com/maxilevi/project-hedra/blob/master/Hedra/Engine/Rendering/WorldRenderer.cs). Renders the entire world in just 3 (actually it depends on how partitioned is the data buffer, but it was designed to be in 3) draw calls (water, terrain, instanced) using glMultiDrawElements.

- [Block.cs](https://github.com/maxilevi/project-hedra/blob/master/Hedra/Engine/Generation/Block.cs). Core component of the world. Read more at [https://maxilevi.com/blog/procedural-meshing-hedra](https://maxilevi.com/blog/procedural-meshing-hedra)

- [Fishing.py](https://github.com/maxilevi/project-hedra/blob/master/Hedra/Scripts/Fishing.py). Written in Python as a mod script using IronPython, directly interacts with the C# API.

- [NativeArray.cs](https://github.com/maxilevi/project-hedra/blob/master/Framework/NativeArray.cs) Implementation of an array primitive using a custom allocator. The same project provides heap based allocators as well as stac based ones using `stackalloc`. The idea was to minimize the automatic heap allocations (and thus avoiding the GC) during the game frames.

## Interesting facts

- The world is fully procedurally generated, there is no world save file. Only the player contents are saved and the world seed. The rest is created at runtime.
- The world is voxel based but an isosurface extraction algorithm called [Marching Cubes](https://en.wikipedia.org/wiki/Marching_cubes) is applied to extract an implicit continous surface from the discrete voxels.
- The name "Hedra" comes from tetrahedra which is the shape used in the original iso surface extraction algorithm I implemented [Marching Tetrahedra](https://en.wikipedia.org/wiki/Marching_tetrahedra)
- The game only supports loading .PLY and .DAE models, both which are in plaintext and the models have no textures. Therefore if you put this into perspective, this means I wrote a bunch of text that reads text and creates interactive images from this. I think this is pretty cool.

## Screenshots

[![image](https://camo.githubusercontent.com/e80c7dbaf53fe7b61b2ace88b8f90fa1ed4373894d1227dee51d0f8f605291d6/68747470733a2f2f63646e2e616b616d61692e737465616d7374617469632e636f6d2f737465616d2f617070732f313030393936302f73735f623336343834653333643363396631383535356133663535643630313439643339313361643733612e6a70673f743d31363735373737383939)](https://camo.githubusercontent.com/e80c7dbaf53fe7b61b2ace88b8f90fa1ed4373894d1227dee51d0f8f605291d6/68747470733a2f2f63646e2e616b616d61692e737465616d7374617469632e636f6d2f737465616d2f617070732f313030393936302f73735f623336343834653333643363396631383535356133663535643630313439643339313361643733612e6a70673f743d31363735373737383939) [![image](https://camo.githubusercontent.com/6375928d0fdaf4321073a091996040c6d267213504f892abb6b93f2a18a10eb1/68747470733a2f2f63646e2e616b616d61692e737465616d7374617469632e636f6d2f737465616d2f617070732f313030393936302f73735f653561373835663765656434663565316336353262386637333638663132626266343266313531652e6a70673f743d31363735373737383939)](https://camo.githubusercontent.com/6375928d0fdaf4321073a091996040c6d267213504f892abb6b93f2a18a10eb1/68747470733a2f2f63646e2e616b616d61692e737465616d7374617469632e636f6d2f737465616d2f617070732f313030393936302f73735f653561373835663765656434663565316336353262386637333638663132626266343266313531652e6a70673f743d31363735373737383939) [![image](https://camo.githubusercontent.com/4e5d78c9238fcf51c5d532d16f8d63259485d3580f96d61377ae252eb47b74ba/68747470733a2f2f63646e2e616b616d61692e737465616d7374617469632e636f6d2f737465616d2f617070732f313030393936302f73735f623861626561396634393234336431643466353134356532363237303139633736623661643532302e6a70673f743d31363735373737383939)](https://camo.githubusercontent.com/4e5d78c9238fcf51c5d532d16f8d63259485d3580f96d61377ae252eb47b74ba/68747470733a2f2f63646e2e616b616d61692e737465616d7374617469632e636f6d2f737465616d2f617070732f313030393936302f73735f623861626561396634393234336431643466353134356532363237303139633736623661643532302e6a70673f743d31363735373737383939) [![image](https://camo.githubusercontent.com/b65b077febdce3b9c9af8ccfc7062957603dc871a0d87f87237a046015f4acd6/68747470733a2f2f63646e2e616b616d61692e737465616d7374617469632e636f6d2f737465616d2f617070732f313030393936302f73735f306535646535646466336338623233643365666336346131393132646434326232666465613331632e6a70673f743d31363735373737383939)](https://camo.githubusercontent.com/b65b077febdce3b9c9af8ccfc7062957603dc871a0d87f87237a046015f4acd6/68747470733a2f2f63646e2e616b616d61692e737465616d7374617469632e636f6d2f737465616d2f617070732f313030393936302f73735f306535646535646466336338623233643365666336346131393132646434326232666465613331632e6a70673f743d31363735373737383939)

TODO add more

## Code Structure

### Overall Architecture

The game is a mod to a base engine. The idea of architecturing this way was to force myself to maintain the mod API up to date, therefore encouraging access to make mods.

#### Hedra

Basically the entire game. This includes AI logic, components, world generation, module systems, object placement and lots of other stuff.

##### Hedra.Engine

The core parts of the engine, this defines all the abstractions to the world. It handles rendering, sound, different base systems as well as the general windowing, game loop and platform features. The split was not done at the start so some stuff might still be outside.

#### Framework

Core primitives used in the engine. This includes heap and stack allocators, custom implementations of lists and arrays as well as wrappers to pointers to native and some other useful primitives

#### HedraTests

Test suite that

#### Asset Builder

Command line tool to compress the game assets into convenient binary files. Combines images, models, sound files, shaders into a single binary file with a structure designed for fast data retrieval.

#### Server

Once upong a time Hedra used to support multiplayer. It worked by doing [UDP Hole Punching](https://en.wikipedia.org/wiki/UDP_hole_punching) and having clients communicate P2P, this server was in charge of that. You might also see some networking remnants in the Engine code.

## How to build

The build process is pretty straightforward.

1. Checkout all the submodules
2. Run the build depedencies scripts
3. Compile the C# solution

You can check the GitHub actions workflows for some examples on Linux and Windows.
