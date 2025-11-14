# Rsdoctor Bundle Analysis Forked Action

This is a custom fork (for testing) of this GitHub Action: https://github.com/web-infra-dev/rsdoctor-action/tree/main/ which is a  GitHub Action for comprehensive bundle size analysis and reporting using [Rsdoctor](https://github.com/web-infra-dev/rsdoctor). It provides detailed bundle analysis, size comparisons, and interactive HTML reports for your web applications. See that README for more detailed information regarding config, features...etc.

This fork is only for testing, and caters to very specific use cases so as a result, it's not intended to be configurable.

## Updates to this fork thus far
- **Comment Configuration**
  - The base GitHub Action currently creates a comment on the PR sharing the bundle diffs with a hardcoded title (i.e. `Rsdoctor Bundle Diff Analysis`)
  - You can pass `comment_identifier` or `comment_title` as inputs to the GitHub Action to further configure this comment - which is particularly useful for monorepo setups.
  - https://github.com/giavinh79/rsdoctor-action-test/commit/3f3aff8ed129cdf4f104644ea2c5cd3aeae90f9e
- **Updating baseline artifact on regular commits to baseline branch**
  - The base GitHub Action only rebuilds and uploads a new baseline artifact when a PR is merged into the baseline branch. However, for certain use cases (i.e. backmerge events), you may have code committed directly to the branch. For these cases, we still want to upload a new artifact.
  - https://github.com/giavinh79/rsdoctor-action-test/commit/752aa34c74f92796f610bc355fb4188e89009d0e
  - Issue created in original repo here - `https://github.com/web-infra-dev/rsdoctor-action/issues/9`
- **CI gating**
  - Simple implementation for gating CI on _x KB increase_ in bundle size
  - Configure by passing these inputs to the GitHub Action `max_kb_increase` and `skip_size_check`
  - https://github.com/giavinh79/rsdoctor-action-test/commit/3df7fa228d954f6ae78e9a4ac5d1294443c7a240
- **Monorepo Support**
  - Ideally we would want to consolidate it all into one GitHub comment and configure the action once, but this at least allows us to run diffs for multiple apps in a monorepo architecture.
  - To use, simply add another step using this custom action and specify a different `file_path`
  - Relevant commits: https://github.com/giavinh79/rsdoctor-action-test/commit/3df7fa228d954f6ae78e9a4ac5d1294443c7a240 & https://github.com/giavinh79/rsdoctor-action-test/commit/13ddbb7608c07f2279a7b03ddc867727c470304b
- **Artifact Retention Updates**
  - For large codebases with many engineers contributing, the workflow will run on hundreds of commits. Currently, artifacts of the builds as well as a HTML files containing more detailed bundle diffs are uploaded to GitHub.
  - On GitHub Enterprise, you can have up to `50gb` stored per month. Additionally, artifact retention is something that can be configured on the repo level. However to be more conservative, I've updated the artifactory retention period of the detailed HTML analysis to be 2 days.
  - https://github.com/giavinh79/rsdoctor-action-test/commit/af9816226748fe055dfd2592712c23454d047555
  

## Development

```bash
# Install dependencies
pnpm install

# Build the action
pnpm run build

# Test with example project
cd examples/rsbuild-demo
pnpm install
pnpm run build
```
