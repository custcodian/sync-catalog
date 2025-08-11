# Custcodian Minder Rule Catalog Sync

This action syncs one or more policies from the [Custcodian rule catalog](https://github.com/custcodian/minder-rules-and-profiles), allowing you to keep a copy of a [Minder](https://mindersec.dev) policy up to date with the source catalog, while preserving local patches and updates (for example, to tweak a policy or enable remediation).

This action works well alongside the [Minder action](https://github.com/custcodian/minder-action); use `sync-catalog` on a schedule (e.g. weekly) to keep your policies up to date (for example, in your `.github` repo), and then use `minder-action` to apply the checked-in rules all the repos in your project.

## Setup

Use this action to create PRs in the repository which sync specific directories of policy files from the source catalog to your environment.

You will also need to [enable GitHub Actions to create and approve pull requests](https://github.com/orgs/community/discussions/110415) in the repository or organization settings.

## Usage

```yaml
name: Sync Minder Policies
on:
  workflow_dispatch:
  schedule:
  - cron: "0 8 * * 1" # Run every Monday at 8AM UTC

permissions:
  # contents: read is needed to checkout the current repo
  contents: write
  pull-requests: write

jobs:

  catalog-sync:
    steps:
    # This will send a PR on the selected schedule to update the `minder` directory
    # with the top-5 policies from custcodian/minder-rules-and-profiles.
    # (this is the default configuration)
    - name: Sync catalog
      uses: custcodian/sync-catalog@v1
      with:
        policy_dirs:
          - top-5
        target_dir: minder
```

## Inputs

### `catalog_repo`

_(optional)_ The repository which contains the rule catalog. Dy default, this is set to `custcodian/minder-rules-and-profiles`.

### `catalog_ref`

_(optional)_ The ref in the catalog repository to update to. The previous reference used is tracked by the README.md file in the `target_dir` in the action's repository. Dy default, this is set to `main`.

### `policy_dirs`

_(optional)_ A list of policy directories to sync. Dy default, this is set to `[top-5]`.

### `target_dir`

_(optional)_ The destination directory to sync to. Dy default, this is set to `minder`.

### `pr_labels`

_(optional)_ Labels to apply to the resulting PR. Dy default, this is set to `[automated pr, chore]`.
