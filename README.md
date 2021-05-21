# GitHub Deployment Buildkite Plugin

Creates GitHub Deployment before/after a Buildkite step is executed.

## Example

Add the following to your `pipeline.yml`:

```yml
steps:
  - command: echo 'Deploy preview created'
    plugins:
      - ailohq/github-deployment#v1.0.7:
          username-env: GITHUB_USERNAME
          token-env: GITHUB_TOKEN
          ref: v1.0.0
          environment: qa
          environment_url: https://dev.ailo.io
          production_environment: false
          transient_environment: false
```

## Configuration

Many of the configuration options are passed to the GitHub deployment API requests. See the [deployment API docs](https://docs.github.com/en/rest/reference/repos#deployments) for additional details.

### `username-env` (Optional, string)

Name of environment variable for your GitHub username. Default: `GITHUB_USERNAME`.

### `token-env` (Optional, string)

Name of environment variable for your GitHub token which should contain `repo_deployment` permissions. Default: `GITHUB_TOKEN`.

### `ref` (Optional, string)

The ref to deploy. This can be a branch, tag, or SHA. Default: current git commit SHA hash.

### `environment` (Optional, string)

Name for the target deployment environment (e.g., production, staging, qa). Default: `production`.

### `environment_url` (Optional, string)

Sets the URL for accessing your environment. Default: `""`.

If you don't know your `environment_url` during pipeline creation, you can use Buildkite meta-data to set the environment url in your command.

```sh
buildkite-agent meta-data set "environment_url" "https://www.google.com"
```

### `production_environment` (Optional, boolean)

Specifies if the given environment is one that end-users directly interact with. Default: `true` when environment is `production` or `prod` and `false` otherwise.

### `transient_environment` (Optional, boolean)

Specifies if the given environment is specific to the deployment and will no longer exist at some point in the future. Default: `false`.

## Developing

To lint the `plugin.yml` file:

```sh
docker-compose run --rm lint
```

To run the tests:

```shell
docker-compose run --rm -e GITHUB_USERNAME=trunkbot -e GITHUB_TOKEN=abc123 tests
```

## Releasing

Bump the version in README example and publish it as a git tag.
