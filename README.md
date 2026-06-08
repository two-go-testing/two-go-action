# two-go-action

A composite GitHub Action that sets up Node.js, installs your project's
dependencies, and runs its [two-go](https://github.com/two-go-testing/two-go)
API tests.

[two-go](https://github.com/two-go-testing/two-go) is a zero-dependency fluent
service and API testing library for Node. This action wraps the common
"set up Node, install, test" flow so you can run your two-go suite in CI with a
single step.

## What it does

1. Sets up the requested Node.js version using `actions/setup-node`.
2. Installs your dependencies (default: `npm ci`).
3. Runs your test command (default: `npm test`), which is expected to execute
   your two-go tests.

It works with any project whose test command runs two-go tests, whether that is
`node --test`, a custom npm script, or any other runner.

## Usage

Create a workflow such as `.github/workflows/tests.yml`:

```yaml
name: API tests

on:
  push:
  pull_request:

jobs:
  two-go:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: two-go-testing/two-go-action@v1
        with:
          node-version: "20"
          working-directory: .
          install-command: npm ci
          test-command: npm test
```

All inputs are optional. The minimal form is:

```yaml
      - uses: actions/checkout@v4
      - uses: two-go-testing/two-go-action@v1
```

## Inputs

| Input               | Description                                              | Required | Default    |
| ------------------- | -------------------------------------------------------- | -------- | ---------- |
| `node-version`      | Node.js version to set up before running the tests.      | No       | `20`       |
| `working-directory` | Directory that contains the project to install and test. | No       | `.`        |
| `install-command`   | Command used to install project dependencies.            | No       | `npm ci`   |
| `test-command`      | Command used to run the two-go tests.                    | No       | `npm test` |

## Notes

- This action works with any project whose test command runs two-go tests. If
  your suite is invoked differently (for example `node --test test/`), set
  `test-command` accordingly.
- If your project lives in a subdirectory of the repository, point
  `working-directory` at it.

## License

[MIT](./LICENSE)
