# 🛠️ Contribution Guidelines

_For personal repositories maintained by [Saumik Sarkar](https://github.com/SaumikSarkar)_

This file outlines the internal workflow and conventions I follow to maintain clean, consistent, and production-ready code across my portfolio projects. It reflects the same standards I apply in team environments.

## 🧬 Branching Strategy

These repositories follow a **Trunk-Based Development** model for fast iteration and simplicity.

- **`main`** — Always stable and production-ready (protected)
- **Short-lived branches** are created for features, bug fixes, documentation, etc., and merged back into `main` via pull requests.

### Branch Naming Convention

Branches are named using the following pattern:

```
type/SSPORT-<issue-number>-<short-description>
```

**Examples:**

- feat/SSPORT-42-add-button-component
- fix/SSPORT-105-handle-redundant-queries

**Types:**

- `feat` – Feature implementation
- `fix` – Bug fix
- `docs` – Documentation updates
- `chore` – Maintenance tasks
- `refactor` – Code cleanup or restructuring
- `test` – Testing-related changes
- `ci` – CI/CD or workflow updates

## 📓 Commit Conventions

Refer to [COMMIT_GUIDELINES.md](./COMMIT_GUIDELINES.md) for full commit structure and tooling details.

## 📌 Note

These guidelines are intentionally followed in my portfolio repositories to mirror industry-standard workflows. They serve as a demonstration of my approach to structured, maintainable, and scalable code practices — even in solo projects.
