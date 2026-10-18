# Contributing to TokenTimer

Thank you for your interest in contributing to TokenTimer!

## Getting Started

1. Fork the repository
2. Clone your fork locally
3. Install dependencies: `pnpm install`
4. Set up your environment: copy `.env.example` files and configure

See [DEVELOPMENT.md](DEVELOPMENT.md) for detailed local setup instructions.
See [QUICKSTART.md](QUICKSTART.md) for deployment options.

## Development Workflow

1. Create a feature branch from `main`
2. Make your changes
3. Run the quality checks:
   ```bash
   pnpm run lint
   pnpm run build
   pnpm run test:contracts
   ```
   If the dashboard is affected, also run the relevant Prettier checks.
   For a complete pre-merge validation, you can run:
   ```bash
   pnpm run test:ci
   ```
4. Commit using [Conventional Commits](https://www.conventionalcommits.org/) format:
   ```
   feat(core-api): add new endpoint for X
   fix(core-ui): resolve rendering issue in Y
   ```
5. Open a Pull Request against `main`

### Maintainer pre-merge validation

Contributor pull-request Actions are intentionally disabled to avoid executing untrusted contributor code in GitHub Actions.

Local validation is the normal pre-merge path. Maintainers should run the relevant local checks before merging. For a complete validation, `pnpm run test:ci` can be used.

A maintainer may use `workflow_dispatch` for a validation run only when the selected code/ref has already been reviewed and is considered trusted. It must not be used to execute unreviewed contributor code.

After a change is merged, the push CI run on `main` remains the authoritative post-merge validation and must be green before a release.

### Reviewing GitHub Actions and Dependabot PRs

Dependabot is configured solely for the `github-actions` package ecosystem to surface upstream Action updates while preserving the repository's supply-chain security posture. Updates for npm dependencies and Docker base images remain disabled.

Maintainers reviewing Dependabot or manual Action update PRs must adhere to the following requirements before merging:

1. **Mandatory full commit SHA pinning**:
   - Every GitHub Action reference in `.github/workflows/*.yml` must remain pinned to an immutable, full 40-character commit SHA (e.g., `uses: actions/checkout@de0fac2e4500dabe0009e67214ff5f5447ce83dd # v6.0.2`).
   - PRs must never convert a commit SHA into a mutable tag (e.g., `@v4`) or branch name. The only approved exception is `github/codeql-action`, which tracks the major tag to receive CodeQL CLI engine updates from GitHub.
   - Pinned actions must retain an inline comment indicating the associated semantic release version (e.g., `# v6.0.2`). Dependabot preserves the SHA format and updates the version tag comment when opening update PRs.

2. **Supply-chain integrity and provenance verification**:
   - Verify that the commit SHA in the PR matches the authentic release tag published upstream by the official action repository.
   - Inspect the upstream commit diff and release changelog between the existing SHA and the proposed SHA for unexpected scripts, network calls, security regressions, or altered behavior.

3. **Workflow permissions and least privilege**:
   - Check whether the updated action requests or requires altered `permissions` blocks. Workflow token permissions must remain strictly minimal and bounded.

4. **Validation before merge**:
   - Auto-merge is strictly disabled for all dependency updates.
   - Maintainers must run relevant local validation checks or trigger a manual `workflow_dispatch` run on a trusted branch before merging to confirm workflows execute without errors.


## Code Style

- Follow existing patterns in the codebase
- Use `camelCase` for variables and functions
- Use `PascalCase` for components and classes
- Use `kebab-case` for file and directory names
- Use `UPPER_CASE` for constants and environment variables

## Testing

- Run contract tests: `pnpm run test:contracts`
- Run integration tests: `pnpm run test:core` (requires Docker)
- Run frontend tests: `pnpm --filter @tokentimer/dashboard test`

## Reporting Issues

- Use GitHub Issues for bug reports and feature requests
- Include steps to reproduce for bugs
- Check existing issues before opening a new one

## Security Issues

Please do **not** open public issues for security vulnerabilities. See [SECURITY.md](SECURITY.md) for responsible disclosure instructions.

## License

By contributing, you agree that your contributions will be licensed under the same [AGPLv3 license](LICENSE) that covers the project. To keep the option of commercial dual-licensing (AGPLv3 + a separate commercial license) open, we may ask contributors to sign a contributor license agreement (CLA) granting Tokentimer Sàrl the rights needed to offer that commercial license.
