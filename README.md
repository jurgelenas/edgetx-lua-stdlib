# edgetx-lua-stdlib

[LuaLS](https://luals.github.io/) type definitions for the [EdgeTX](https://edgetx.org/) Lua API. Provides IDE autocompletion, inline documentation, and type checking for any EdgeTX Lua project.

## Branches and tags

| Branch | Tracks |
|--------|--------|
| `main` | Latest stable EdgeTX release |
| `edgetx-2.12` | EdgeTX 2.12.x |
| `edgetx-3.0` | EdgeTX 3.0.x |
| `nightly` | EdgeTX nightly builds |

Semantic version tags (e.g. `v1.0.0`) are available to pin a specific stdlib version.

## Usage as a git submodule

### Adding to your project

```sh
git submodule add https://github.com/<owner>/edgetx-lua-stdlib.git edgetx-lua-stdlib
```

### Installing after cloning

```sh
git submodule update --init
```

### Updating to the latest version

```sh
git submodule update --remote edgetx-lua-stdlib
```

### Pinning a specific version

```sh
cd edgetx-lua-stdlib && git checkout v1.0.0 && cd ..
git add edgetx-lua-stdlib
```

### Tracking a specific EdgeTX branch

```sh
git submodule set-branch --branch edgetx-2.12 edgetx-lua-stdlib
git submodule update --remote edgetx-lua-stdlib
```

## Configuring LuaLS

Add a `.luarc.json` to the root of your project:

```json
{
  "runtime.version": "Lua 5.3",
  "runtime.builtin": {
    "os": "disable",
    "debug": "disable",
    "package": "disable",
    "coroutine": "disable",
    "utf8": "disable",
    "io": "disable",
    "bit32": "disable"
  },
  "workspace.library": ["edgetx-lua-stdlib"],
  "workspace.ignoreDir": ["bin", "edgetx-lua-stdlib"],
  "workspace.checkThirdParty": false,
  "diagnostics.disable": ["lowercase-global"]
}
```

The key setting is `"workspace.library": ["edgetx-lua-stdlib"]` which tells LuaLS to load the type definitions from the submodule directory.

## Makefile targets for consuming repos

You can add LuaLS type checking to your project's Makefile:

```makefile
LUALS_VERSION := 3.17.1
LUALS_DIR := bin/lua-language-server
LUALS := $(LUALS_DIR)/bin/lua-language-server
LUALS_URL := https://github.com/LuaLS/lua-language-server/releases/download/$(LUALS_VERSION)/lua-language-server-$(LUALS_VERSION)-linux-x64.tar.gz

install-luals:
	mkdir -p $(LUALS_DIR)
	curl -fSL $(LUALS_URL) | tar xz -C $(LUALS_DIR)

typecheck:
	$(LUALS) --check .
```

See the [CI workflow](.github/workflows/ci.yml) for a GitHub Actions example.

## Development

### Formatting

All Lua files are formatted with [StyLua](https://github.com/JohnnyMorganz/StyLua):

```sh
make format        # format all files
make format-check  # check without modifying
```

### Running all checks

```sh
make install-tools  # one-time setup
make check          # format-check + typecheck
```

## License

[GPL-3.0](LICENSE)
