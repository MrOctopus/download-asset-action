# MrOctopus/download-asset-action
Downloads an asset from a repository release.

This actions allows you to download a specific asset
from either the latest release in a repository, or
from a specified tagged version.

Downloading release assets from a private repository
is supported as long as you specify a PAT token with
the necessary permissions.

NOTE:
All runners are supported.

## Inputs
| Input      | Description                                                | Required | Default                  |
|------------|------------------------------------------------------------|----------|--------------------------|
| repository | The repository to download from                            | No       | github.repository        |
| excludes   | The release types to exclude                               | No       | None                     |
| tag        | The full/substring release tag                             | No       | Latest                   |
| asset      | The full/substring name of the asset to download           | Yes      |                          |
| target     | The target path to save the asset at                       | No       | .                        |
| token      | The personal access token (PAT) used to download the asset | No       | github.token             |

## Outputs
| Ouput | Description                      |
|-------|----------------------------------|
| name  | The downloaded asset's full name |

## Example usage

### Download a specific Texconv build
```yaml
name: Download specific Texconv.exe build
on: [workflow_dispatch]

jobs:
  download:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2

      - id: latest_asset
        uses: MrOctopus/download-asset-action@main
        with:
          repository: microsoft/DirectXTex
          excludes: prerelease, draft
          tag: jan2021
          asset: texconv
          target: "./texconv"
```

### Download the lastest pyro build and unpack
```yaml
name: Download latest pyro build and unpack
on: [workflow_dispatch]

jobs:
  download:
    runs-on: windows-latest
    steps:
      - uses: actions/checkout@v2

      - id: latest_asset
        uses: MrOctopus/download-asset-action@main
        with:
          repository: fireundubh/pyro
          asset: pyro

      - run: 7z x ${Env:file_name} -opyro
        env: 
          file_name: ${{ steps.latest_asset.outputs.name }}

```

## License
The scripts and documentation in this project are released under the [GPL-3.0 License](LICENSE)