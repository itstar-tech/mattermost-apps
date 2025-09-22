# Demo Plugin

[![Build Status](https://img.shields.io/circleci/project/github/mattermost/mattermost-plugin-demo/master.svg)](https://circleci.com/gh/mattermost/mattermost-plugin-demo)
[![Code Coverage](https://img.shields.io/codecov/c/github/mattermost/mattermost-plugin-demo/master.svg)](https://codecov.io/gh/mattermost/mattermost-plugin-demo)
[![Release](https://img.shields.io/github/v/release/mattermost/mattermost-plugin-demo)](https://github.com/mattermost/mattermost-plugin-demo/releases/latest)
[![HW](https://img.shields.io/github/issues/mattermost/mattermost-plugin-demo/Up%20For%20Grabs?color=dark%20green&label=Help%20Wanted)](https://github.com/mattermost/mattermost-plugin-demo/issues?q=is%3Aissue+is%3Aopen+sort%3Aupdated-desc+label%3A%22Up+For+Grabs%22+label%3A%22Help+Wanted%22)

**Maintainer:** [@hanzei](https://github.com/hanzei)
**Co-Maintainer:** [@jfrerich](https://github.com/jfrerich)

This plugin demonstrates the capabilities of a Mattermost plugin. It uses Bazel as the build system and implements all
supported server-side hooks and registers a component for each supported client-side integration point.
See [server/README.md](server/README.md) and [webapp/README.md](webapp/README.md) for more details. The plugin also
doubles as a testbed for verifying plugin functionality during release testing.

Once installed and enabled, you can specify both the channel and user for the demo plugin. If the specified channel or user doesn't exist, the plugin creates it for you.

Feel free to base your own plugin off this repository, removing or modifying components as needed.

Note that this plugin is authored for the Mattermost version indicated in the `min_server_version` within [plugin.json](https://github.com/mattermost/mattermost-plugin-demo/blob/master/plugin.json), and is not compatible with earlier releases of Mattermost.

## Building the Plugin

This plugin uses Bazel as the build system. To build the plugin:

### Prerequisites

- [Bazel](https://bazel.build/install)
- Go 1.19+
- Node.js 20.x

### Building

1. **Build the plugin bundle:**
    ```bash
    bazel build //:plugin_bundle
    ```
   This generates a `.tar.gz` file ready for distribution.

2. **Build specific components:**
    ```bash
    # Build only the server
    bazel build //server:server_binaries
    
    # Build only the webapp
    bazel build //webapp:webapp_bundle
    ```

3. **Run tests:**
    ```bash
    # Run Go tests
    bazel test //server:server_test
    
    # Run webapp tests
    bazel test //webapp:test_config
    ```

## Releasing this plugin

A new minor version of this plugin is released with every feature release of Mattermost. The new version should be cut until [Code complete](https://docs.mattermost.com/process/feature-release.html#f-t-minus-14-working-days-code-complete).

## How to Release

To trigger a release, follow these steps:

1. **For Patch Release:** Run the following command:
    ```
    make patch
    ```
   This will release a patch change.

2. **For Minor Release:** Run the following command:
    ```
    make minor
    ```
   This will release a minor change.

3. **For Major Release:** Run the following command:
    ```
    make major
    ```
   This will release a major change.

4. **For Patch Release Candidate (RC):** Run the following command:
    ```
    make patch-rc
    ```
   This will release a patch release candidate.

5. **For Minor Release Candidate (RC):** Run the following command:
    ```
    make minor-rc
    ```
   This will release a minor release candidate.

6. **For Major Release Candidate (RC):** Run the following command:
    ```
    make major-rc
    ```
   This will release a major release candidate.

