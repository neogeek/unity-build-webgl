# neogeek/unity-build-webgl

> Build Unity project as WebGL

## Usage

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: neogeek/unity-build-webgl@v0
        with:
          # The serial key found at https://id.unity.com/en/subscriptions.
          # Keys are only avalible with a Plus or Pro Subscription.
          #
          # Required
          UNITY_SERIAL: ${{ secrets.UNITY_SERIAL }}

          # Your email address used to log into https://unity.com/
          #
          # Required
          UNITY_USERNAME: ${{ secrets.UNITY_USERNAME }}

          # Your password used to log into https://unity.com/
          #
          # Required
          UNITY_PASSWORD: ${{ secrets.UNITY_PASSWORD }}
          # The method used to build the project as WebGL. See Unity documentation
          # for more information https://docs.unity3d.com/Manual/EditorCommandLineArguments.html
          # See example script https://github.com/neogeek/unity-build-webgl/blob/main/examples/Build.cs
          #
          # Required
          UNITY_BUILD_EXECUTE_METHOD: 'Build.BuildWebGL'

          # If the action should cache the installer files.
          #
          # Values: 'true' | 'false'
          # Optional
          CACHE_INSTALLERS: 'true'

          # The directory that contains the Unity project
          #
          # Optional
          WORKING_DIRECTORY: '.'

          # Unity installer hash. See editor installers URLs at
          # https://github.com/neogeek/get-unity/blob/main/data/editor-installers.json
          #
          # Optional
          UNITY_INSTALLER_HASH: '35713cd46cd7'

          # Unity installer version. See editor installers URLs at
          # https://github.com/neogeek/get-unity/blob/main/data/editor-installers.json
          #
          # Optional
          UNITY_INSTALLER_VERSION: '2022.3.4f1'

          # URL for the editor installer. See editor installers URLs at
          # https://github.com/neogeek/get-unity/blob/main/data/editor-installers.json
          #
          # Optional
          UNITY_INSTALLER_URL: 'https://download.unity3d.com/download_unity/35713cd46cd7/LinuxEditorInstaller/Unity.tar.xz'

          # URL for the editor WebGL plugin installer
          # Optional
          UNITY_PLUGIN_INSTALLER_URL: 'https://download.unity3d.com/download_unity/35713cd46cd7/LinuxEditorTargetInstaller/UnitySetup-WebGL-Support-for-Editor-2022.3.4f1.tar.xz'
```

## Scenarios

- [Simple Usage](#simple-usage)
- [Store Build Artifacts](#store-build-artifacts)

### Simple Usage

**.github/workflows/build.workflow.yml**

```yaml
name: Build

on:
  workflow_dispatch:
  push:
    branches:
      - main

concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}
  cancel-in-progress: true

jobs:
  build:
    runs-on: ubuntu-latest
    timeout-minutes: 30

  steps:
    - name: Check out repository
      uses: actions/checkout@v3

    - name: Build Unity WebGL
      uses: neogeek/unity-build-webgl@v0
      with:
        UNITY_SERIAL: ${{ secrets.UNITY_SERIAL }}
        UNITY_USERNAME: ${{ secrets.UNITY_USERNAME }}
        UNITY_PASSWORD: ${{ secrets.UNITY_PASSWORD }}
        UNITY_BUILD_EXECUTE_METHOD: 'Build.BuildWebGL'
```

### Store Build Artifacts

**.github/workflows/build.workflow.yml**

```yaml
name: Build

on:
  workflow_dispatch:
  push:
    branches:
      - main

concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}
  cancel-in-progress: true

jobs:
  build:
    runs-on: ubuntu-latest
    timeout-minutes: 30

    steps:
      - name: Check out repository
        uses: actions/checkout@v3

      - name: Build Unity WebGL
        uses: neogeek/unity-build-webgl@v0
        with:
          UNITY_SERIAL: ${{ secrets.UNITY_SERIAL }}
          UNITY_USERNAME: ${{ secrets.UNITY_USERNAME }}
          UNITY_PASSWORD: ${{ secrets.UNITY_PASSWORD }}
          UNITY_BUILD_EXECUTE_METHOD: 'Build.BuildWebGL'

      - name: Upload Build Artifacts
        uses: actions/upload-artifact@v3
        with:
          name: build-artifact
          path: Builds
          if-no-files-found: error
```
