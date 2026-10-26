# Contributing

Moai has completed Stage 0 product definition. The current contribution surface is documentation, architecture review, and reproducible technical experiments in [codevedas/moai](https://github.com/codevedas/moai). The license, governance, and code contribution terms have not been finalized.

## Useful contributions now

- Identify missing requirements using concrete operator scenarios.
- Review migration, restore, scheduling, and failure-recovery assumptions.
- Propose narrow prototypes that resolve an open engineering decision.
- Improve terminology, consistency, and acceptance criteria.

Read the [vision](docs/vision.md), [first-release scope](docs/first-release.md), and [engineering decisions](docs/engineering.md) before proposing an implementation.

Follow the required [open-source and open-standards policy](docs/open-source-and-standards.md). Dependency proposals must identify the exact version and license, explain compatibility and purpose, and preserve a self-hostable path. Interface proposals should reuse applicable open specifications and include a way to verify compatibility.

## Change proposals

Explain the problem, affected user workflow, proposed behavior, alternatives, and how the result can be verified. Label new assumptions and unresolved questions. For behavior changes, update the relevant docs alongside the implementation once code exists.

Use isolated test infrastructure and synthetic data for experiments. Include reproduction instructions and observed limitations. Do not commit credentials, private support reports, or production data.

## Future code workflow

Build, formatting, linting, and test commands will be documented after the implementation stack is chosen. Contributions involving operational adapters will need capability contract tests and relevant failure scenarios. A contribution must not expand claims of support beyond the configurations it verifies.

A license and contribution policy must be established before soliciting public code contributions. A private vulnerability reporting channel must also be established before a public operational release; no contact address is invented here.
