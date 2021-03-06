---
id: build-overview
title: Overview
---

BuckleScript comes with a build system, bsb, that's meant to be fast, lean and used as the build system of the BuckleScript/Reason community.

Bsb provides a few templates to quickly start a new project:

```sh
bsb -init my-directory-name
```

To generate a Reason project:

```sh
bsb -init my-directory-name -theme basic-reason
```

Feel free to inspect the various files in the newly generated directory. To see all the templates available, do:

```sh
bsb -themes
```

<!-- TODO: clean up themes -->

The build description file is called `bsconfig.json`. Every BuckleScript project needs one.

To build a project, run:

```sh
bsb -make-world
```

Add `-w` to keep the built-in watcher running. Any new file change will be picked up and the build will re-run. **Note** that third-party libraries in `node_modules` aren't watched as it may exceed the node.js watcher count limit.

To build only yourself, use `bsb -make`.

`bsb -help` to see all the available options.

## Artifacts Cleaning

If you ever get into a stable build for edge-case reasons, use:

```sh
bsb -clean-world
```
jj
Or `bsb -clean` to clean only your own artifacts.

## Editor Support

Bsb generates a `.merlin` file, used by various [editor plugins](https://reasonml.github.io/guide/editor-tools/editors-plugins) under the hood to power e.g. autocomplete, type hint, diagnosis, etc.

**Bonus**: you can directly pipe the bsb terminal error messages into VSCode by setting the config [here](https://github.com/reasonml-editor/vscode-reasonml#bsb).

### Tips & Tricks

A typical problem with traditional build systems is that they're not resilient against the user moving/deleting source files. Most don't clean up the old artifacts correctly after such user action\*. Bsb is unfortunately no different, **unless** you turn on `"suffix": ".bs.js"` in `bsconfig.json`, in which case we can track which JS artifact belongs to which source file correctly, even against source file moving/deletion.

## Design Decisions

\* One such build system that tracks these correctly & efficiently is [Tup](http://gittup.org/tup/). See the (rather accessible!) paper [here](http://gittup.org/tup/build_system_rules_and_algorithms.pdf). Unfortunately, Tup's implementation uses FUSE and other systems, which we can't safely use on every platform.
