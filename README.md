# CI-CD Reusable Workflows and Composite Actions

## BuildMSYS2CMakeVCPkgProject (Composite Action)

Example usage:

```yaml
      - name: Build Native Library
        uses: Squirrelies/CI-CD/BuildMSYS2CMakeVCPkgProject@main
        id: build-native-library
        with:
          root-path: ./src/ProcessMemoryNative
          msys2-msystem: clang64
          msys2-packages: base-devel mingw-w64-clang-x86_64-toolchain mingw-w64-clang-x86_64-cmake mingw-w64-x86_64-clang-tools-extra
          #vcpkg-root: C:/VCPkg
          preset-configuration: Release
          preset-configuration-tidy: Release-Tidy
          run-unit-tests: true
          additional-cmake-args: -DARCH=x64
```

## GetCSProjVersion and GetUTCDateTime (Composite Actions)

Example where `<Version>2.4.1</Version>` is set in the csproj file, `-beta` is supplied to the `inputs`.`releaseTypeTag`, and the current UTC DateTime is `20250312232605` (in yyyyMMddHHmmss):

```yaml
      - name: Get C# project version
        uses: Squirrelies/CI-CD/GetCSProjVersion@main
        id: Get-CSProj-Version
        with:
          repository: ${{github.repository}}
          ref: ${{github.ref}}
          csproj-path: src/Project/Project.csproj

      - name: Get UTC DateTime
        uses: Squirrelies/CI-CD/GetUTCDateTime@main
        id: Get-UTC-DateTime

      - name: Set VERSION environment variable
        run: echo 'VERSION=${{steps.Get-CSProj-Version.outputs.Version}}${{inputs.releaseTypeTag}}.${{steps.Get-UTC-DateTime.outputs.UTC-DateTime}}' >> $env:GITHUB_ENV
```

The environment variable `VERSION` would then equal `2.4.1-beta.20250312232605`. Other outputs supported by `GetCSProjVersion` are `Version-Major`, `Version-Minor`, and `Version-Patch` for retrieving each individual component.
