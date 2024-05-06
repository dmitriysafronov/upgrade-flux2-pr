# upgrade-flux2-pr

GitHub Action to upgrade FluxCD 2 in cluster directory.

Example:

```yaml
name: Upgrade Flux

"on":
  workflow_dispatch:
  schedule:
    - cron: "0 0 * * *"

jobs:

  list-clusters:
    name: List clusters
    runs-on: ubuntu-latest
    outputs:
      directories: ${{ steps.get-flux-directories.outputs.directories }}
    steps:
      - name: Find cluster directories with FluxCD 2 installed
        uses: dmitriysafronov/get-flux2-directories@2.0.1
        id: get-flux-directories

  upgrade-flux:
    name: Upgrade FluxCD
    needs: [list-clusters]
    runs-on: ubuntu-latest
    strategy:
      matrix:
        directory: ${{ fromJson(needs.list-clusters.outputs.directories ) }}
    steps:
      - name: Upgrade FluxCD 2 in cluster directory
        uses: dmitriysafronov/upgrade-flux2-pr@2.0.1
        with:
            directory: ${{ matrix.directory }}
```
