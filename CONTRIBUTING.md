# Contributing

About how to write [registry.yaml](registry.yaml), please see [Registry Configuration](https://aquaproj.github.io/docs/reference/registry-config).

## Add the packages

Please create directories in [pkgs](pkgs) per package and add `pkg.yaml` and `registry.yaml`.

e.g. cli/cli

- [pkg.yaml](pkgs/cli/cli/pkg.yaml)
- [registry.yaml](pkgs/cli/cli/registry.yaml)

And please run `bash generate-registry.sh` to update [registry.yaml](registry.yaml).

```console
$ bash generate-registry.sh
```

## Run generate-registry.sh to update reigstry.yaml

[registry.yaml on the repository root directory](registry.yaml) and [registry.json](registry.json) are generated with [generate-registry.sh](generate-registry.sh).
Don't edit it manually, and if you update `registry.yaml` in [pkgs](pkgs) directory, don't forget to run generate-registry.sh.

```console
$ bash generate-registry.sh
```

## Style Guide

- Format with [prettier](https://prettier.io/) ([GitHub Actions](.github/workflows/prettier.yaml))
- Remove spaces in the template `{{ ` and ` }}`
- Remove a period from the end of the description
- `link` is unneeded if `repo_owner` and `repo_name` are set

### Remove spaces in the template `{{ ` and ` }}`

:thumbsup:

```yaml
asset: "tfcmt_{{.OS}}_{{.Arch}}.tar.gz"
```

:thumbsdown:

```yaml
asset: "tfcmt_{{ .OS }}_{{ .Arch }}.tar.gz"
```

### Remove a period from the end of the description

:thumbsup:

```yaml
description: A command-line tool that makes git easier to use with GitHub
```

:thumbsdown:

```yaml
description: A command-line tool that makes git easier to use with GitHub.
```

## Test multiple versions

If the package has the field [version_overrides](https://aquaproj.github.io/docs/reference/registry-config#version_constraint-version_overrides),
please add not only the latest version but also old versions in `pkg.yaml` to test if old versions can be installed properly.

e.g. [pkg.yaml](pkgs/scaleway/scaleway-cli/pkg.yaml) [registry.yaml](pkgs/scaleway/scaleway-cli/registry.yaml)

```yaml
packages:
  - name: scaleway/scaleway-cli@v2.5.1
  - name: scaleway/scaleway-cli
    version: v2.4.0
```

:warning: Don't use the short syntax `<package name>@<version>` for the old version to prevent Renovate from updating the old version.

:thumbsdown:

```yaml
packages:
  - name: scaleway/scaleway-cli@v2.5.1
  - name: scaleway/scaleway-cli@v2.4.0
```

## How to test in your localhost

```console
$ cp aqua.yaml.tmpl aqua.yaml
$ vi aqua.yaml # Add tested packages
$ aqua i --test
```

## Change `GOOS` and `GOARCH` for testing

Please see https://aquaproj.github.io/docs/reference/change-os-arch-for-test
