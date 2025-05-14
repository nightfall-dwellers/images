# Dev Container Templates

This project provides a collection of pre-configured templates for [Dev Containers](https://containers.dev/), which allow you to create consistent and reproducible development environments.

## Getting Started

To use a template, you can reference it in your `devcontainer.json` file. For example, to use the `base` template, you would add the following to your configuration:

```json
{
  "image": "ghcr.io/nightfall-dwellers/images/base:latest"
}
```

## Local Testing

To ensure that your changes are working correctly before creating a pull request, you can run the smoke tests locally. This process mimics the continuous integration (CI) environment.

1.  **Navigate to the project's root directory.**

2.  **Run the build script:** This will set up the test environment and build the dev container. Replace `base` with the name of the template you want to build and test.

    ```bash
    ./.github/actions/smoke-test/build.sh base
    ```

3.  **Run the test script:** This will execute the tests inside the container.

    ```bash
    ./.github/actions/smoke-test/test.sh base
    ```

4.  **(Optional) Remove docker images after testing:** You can remove images and builder cache to reclaim disk space. _Skip this step to improve iteration times._

    ```bash
    docker image prune -a -f && docker builder prune -a -f
    ```

## Continuous Integration

This project uses GitHub Actions to automatically test templates when a pull request is created. The workflow is defined in the `.github/workflows/test-pr.yaml` file.

The workflow uses a composite action, defined in `.github/actions/smoke-test/action.yaml`, to perform the following steps:

1.  **Detect Changes:** The workflow first determines which templates have been modified in the pull request.
2.  **Build and Test:** For each modified template, the action will:
    - Build a dev container using the template's configuration.
    - Run the smoke tests located in the template's `test` directory.

## Project Structure

```
.
├── .github/                # GitHub Actions workflows and actions
├── src/                    # Source code for the dev container templates
│   └── base/
│       ├── devcontainer-template.json
│       └── ...
└── test/                   # Tests for the templates
    ├── base/
    │   └── test.sh
    └── test-utils/
        └── test-utils.sh
```

## License

This project is licensed under the terms of the MIT License. See the [LICENSE](https://www.google.com/search?q=images-feature-base/LICENSE) file for more details.
