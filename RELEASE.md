# Release process

To publish a new version to PyPI, complete the following steps:

- Bump the version in `pyproject.toml` and commit
- Tag the latest commit with the version in this format: `v1.2.3`. e.g.
  `git tag v1.2.3`.
- Push the tag to GitHub with `git push --tags`

This will trigger a GitHub action to automatically build the package and
publish it to PyPI. The repository is configured as a 'PyPI trusted
publisher', so it is authorised to automatically publish new versions.

