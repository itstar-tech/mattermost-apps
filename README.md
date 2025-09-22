# Mattermost Plugins Monorepo

[![Build Status](https://img.shields.io/circleci/project/github/mattermost/mattermost-plugin-demo/master.svg)](https://circleci.com/gh/mattermost/mattermost-plugin-demo)
[![Code Coverage](https://img.shields.io/codecov/c/github/mattermost/mattermost-plugin-demo/master.svg)](https://codecov.io/gh/mattermost/mattermost-plugin-demo)
[![Release](https://img.shields.io/github/v/release/mattermost/mattermost-plugin-demo)](https://github.com/mattermost/mattermost-plugin-demo/releases/latest)
[![HW](https://img.shields.io/github/issues/mattermost/mattermost-plugin-demo/Up%20For%20Grabs?color=dark%20green&label=Help%20Wanted)](https://github.com/mattermost/mattermost-plugin-demo/issues?q=is%3Aissue+is%3Aopen+sort%3Aupdated-desc+label%3A%22Up+For+Grabs%22+label%3A%22Help+Wanted%22)

**Maintainer:** [@hanzei](https://github.com/hanzei)
**Co-Maintainer:** [@jfrerich](https://github.com/jfrerich)

This is a monorepo containing multiple Mattermost plugins demonstrating various capabilities. It uses Bazel as the build
system for efficient builds and dependency management across all plugins.

## Plugins

### Demo Plugin (`demo/`)

The original demo plugin demonstrating the capabilities of a Mattermost plugin. It implements all supported server-side
hooks and registers a component for each supported client-side integration point.

### Demo2 Plugin (`demo2/`)

A second demo plugin with identical functionality but unique plugin ID for testing and development purposes.

Each plugin has its own directory with:

- `server/` - Go server-side code
- `webapp/` - React/TypeScript client-side code
- `assets/` - Plugin assets (icons, etc.)
- `public/` - Static public files
- `plugin.json` - Plugin manifest

## Building the Plugins

This monorepo uses Bazel as the build system. To build the plugins:

### Prerequisites

- [Bazel](https://bazel.build/install)
- Go 1.19+
- Node.js 20.x

### Building

1. **Build all plugins:**
    ```bash
    bazel build //:all_plugins
    ```
   This generates `.tar.gz` files for all plugins ready for distribution.

2. **Build individual plugins:**
    ```bash
    # Build demo plugin
    bazel build //demo:plugin_bundle
    
    # Build demo2 plugin  
    bazel build //demo2:plugin_bundle
    ```

3. **Build specific components:**
    ```bash
    # Build only a server component
    bazel build //demo/server:server_binaries
    
    # Build only a webapp component
    bazel build //demo/webapp:webapp_bundle
    ```

4. **Run tests:**
    ```bash
    # Run Go tests for a specific plugin
    bazel test //demo/server:server_test
    bazel test //demo2/server:server_test
    
    # Run webapp tests
    bazel test //demo/webapp:test_config
    bazel test //demo2/webapp:test_config
    ```

## Monorepo Structure

```
├── demo/                    # Demo Plugin
│   ├── assets/
│   ├── public/
│   ├── server/
│   ├── webapp/
│   ├── plugin.json
│   └── BUILD.bazel
├── demo2/                   # Demo2 Plugin
│   ├── assets/
│   ├── public/  
│   ├── server/
│   ├── webapp/
│   ├── plugin.json
│   └── BUILD.bazel
├── BUILD.bazel              # Root build configuration
├── MODULE.bazel            # Bazel module configuration
└── package.json           # Shared Node.js dependencies
```

## Adding New Plugins

To add a new plugin to the monorepo:

1. Create a new directory (e.g., `my-plugin/`)
2. Copy the structure from `demo/` or `demo2/`
3. Update the `plugin.json` with unique ID and name
4. Create a `BUILD.bazel` file following the existing pattern
5. Add the new plugin to `//:all_plugins` target in root `BUILD.bazel`

Note that plugins are authored for the Mattermost version indicated in the `min_server_version` within each
`plugin.json`, and may not be compatible with earlier releases of Mattermost.

## Releasing this plugin

A new minor version of this plugin is released with every feature release of Mattermost. The new version should be cut until [Code complete](https://docs.mattermost.com/process/feature-release.html#f-t-minus-14-working-days-code-complete).
