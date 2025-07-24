# 📘 Commit Message Guidelines

> This document outlines the commit message conventions and tooling used across the personal repositories maintained by [Saumik Sarkar](https://github.com/SaumikSarkar). While these are personal projects, maintaining a consistent and professional Git history helps ensure code clarity, effective debugging, and strong presentation for potential collaborators or reviewers.

## 📖 **Introduction**

To ensure code quality, consistency, and better collaboration across teams, a set of contribution guidelines is enforced. One of the most critical elements is maintaining a clear, structured **Git commit history**. This helps with:

- 🔍 Easier debugging and blame tracing
- 📦 Better changelogs and release automation
- 🔗 Direct correlation between code and Jira tickets

The project follows the [Conventional Commits](https://www.conventionalcommits.org/) specification — with some *custom rules* — enforced via:

- 🧱 **Local pre-commit hooks** using Husky
- 🚀 **GitHub Actions** on all Pull Requests to validate commit messages

By following this guide, it streamlines reviews, automates releases, and keeps the project maintainable.

## ✅ **Commit Message Format**

All commit messages must follow the structure below:

```
<type>(<scope>): <subject>

<body>

Fixes: TICKET-123, TICKET-456
```

Each commit should describe **what was changed** and **why** — not how. Commit messages are treated as a public history and should be helpful to all collaborators.

### 🧪 **Valid Example**

The following examples demonstrate **properly formatted commit messages** that meet all of my **commitlint** rules and contribution standards.

🔹 *Single Ticket Example*

```
feat(route-guard): add authorization checks for the routes

Implement authorization for the users when navigating to a route based on the roles.

Fixes: SSPORT-1234
```

✅ Why this works:

* `feat(route-guard)` uses the correct type (`feat` for a new feature) and a clear, scoped area of the code (`route-guard`).
* The **subject** is written in **imperative mood**: "add" instead of "added" or "adding".
* The **body** provides essential context, explaining the **why** behind the change — in this case, implementing role-based access control using ABAC.
* The **footer** includes the correctly formatted ticket reference: `Fixes: SSPORT-1234`. This ensures traceability between the code change and the Jira ticket it resolves.

🔹 *Multiple Tickets Example*

```
fix(analytics): add aria properties to the chart containers

Improve accessibility for the chart containers in analytics by adding the relevant aria properties.

Fixes: SSPORT-1234, SSPORT-5678, TICKET-ANALYTICS-987
```

✅ Why this works:

* `fix(analytics)` uses `fix` to indicate a bug or quality-of-life improvement (in this case, accessibility).
* The **scope** (`analytics`) tells reviewers and future readers where the change occurred, making history easier to navigate.
* The **subject** uses **imperative tone**: "add" (not "added" or "adding").
* The **body** explains the technical change and its purpose, specifically enhancing accessibility via ARIA attributes.
* The **footer** correctly references multiple Jira tickets using the `Fixes:` prefix, with tickets separated by commas. This shows that the single commit addresses multiple work items: `Fixes: SSPORT-1234, SSPORT-5678, TICKET-ANALYTICS-987`.

### 📌 **Allowed Types**

The `<type>` field must be one of the following:

| Type      | Meaning                                                 |
| --------- | ------------------------------------------------------- |
| `feat`    | Introducing a new feature                               |
| `fix`     | A bug fix                                               |
| `docs`    | Changes only to documentation                           |
| `style`   | Changes that do not affect code logic                   |
| `refactor`| Code change that neither fixes a bug nor adds a feature |
| `test`    | Adding or fixing tests                                  |
| `chore`   | Changes to the build process, tooling, etc.             |
| `ci`      | Changes to CI configuration/workflows/scripts           |
| `revert`  | Reverts a previous commit                               |

> 📖 Refer to [Conventional Commits Type Definitions](https://www.conventionalcommits.org/en/v1.0.0/#summary) for more details.

### ✏️ **Subject Line Rules**

The commit `subject` must:

- ✅ Be written in the **imperative mood** (e.g., “add”, not “added” or “adds”)
- ✅ Start with a valid `type` and optional `(scope)`
- ✅ Be concise (ideally ≤ 72 characters)
- ❌ Not end with a period `.`

✅ Good:

```
fix(axios-request): handle null values in response
```

❌ Bad:

```
handle null values in response
handled null values in response
fix(axios-request): handled null values in response
fix(axios-request): null values handling in response
fix(axios-request): Handle null values in response
```

### 📚 **Commit Body (Optional but Recommended)**

The commit body provides **context** and **motivation** for the change. Use it to explain:

* Why the change was needed
* What problems it solves
* Any relevant side effects or caveats

>Wrap lines at 100 characters for better readability in terminals and tools like GitHub.

>ℹ️ If your change is very small (like a typo), the body may be omitted.

### ✅ **Footer**

Every commit **must end with a footer line** referencing the relevant Jira ticket(s) and Sentry Short ID(s) (for fixing any Sentry reported issues), in the format:

- ✅ Starts with `Fixes:`
- ✅ Contain valid ticket(s) (e.g., SSPORT-1234”)
- ✅ Multiple ticket(s) should be separated by `commas`
- ❌ Must not be empty

✅ Good:

```
Fixes: SSPORT-1234
Fixes: SSPORT-1234, SSPORT-987, TICKET-ANALYTICS-567
```

❌ Bad:

```
fixes: SSPORT-1234         // wrong case
Fixes SSPORT-1234          // missing colon
Fixes: SSPORT-1234 SSPORT-5678 // tickets not separated by comma
```

The footer format must follow this regex:

```
/^Fixes: ([A-Z]+(?:-[A-Z]+)*-\d+)(, [A-Z]+(?:-[A-Z]+)*-\d+)*$/
```

This is enforced via a custom rule in `commitlint.config.js`.

## 🛠 **Local Enforcement (via Husky)**

To prevent invalid commits before they even reach the branch, **Husky pre-commit hooks** are used to validate commit messages locally.

🔧 `.husky/commit-msg`

```sh
#!/usr/bin/env sh
npx --no-install commitlint --edit "$1"
```

📚 Learn more about [Husky Git Hooks](https://typicode.github.io/husky/#/)

## 🚀 **CI Enforcement (via GitHub Actions)**

In addition to local enforcement, all commits pushed to a pull request are validated using GitHub Actions and `wagoid/commitlint-github-action`.

📄 `.github/workflows/commitlint.yml`

```yaml
name: Validate commit lint
on:
  pull_request:
    types: [opened, reopened, synchronize]
    branches:
      - main
env:
  NODE_VERSION: '22'
jobs:
  commitlint:
    runs-on: kubernetes
    steps:
      - name: Checkout code
        uses: actions/checkout@v4
        with:
          fetch-depth: 0
      - name: Set node version to ${{ env.NODE_VERSION }}
        uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
      - name: Install dependencies
        run: npm install --save-dev @commitlint/{config-conventional,cli}
        env:
          NODE_AUTH_TOKEN: ${{ secrets.SIMPPLR_NPM_TOKEN }}
          NPM_TOKEN: ${{ secrets.SIMPPLR_NPM_TOKEN }}
      - name: Check commit lint
        uses: wagoid/commitlint-github-action@v6
        env:
          NODE_PATH: ${{ github.workspace }}/node_modules
```

If the commit messages in the PR don’t pass, the check fails, and the PR can’t be merged.

## 🧪 **Commitlint Rule Summary**

📄 `commitlint.config.js`

```js
module.exports = {
  extends: ['@commitlint/config-conventional'],
  plugins: [
    {
      rules: {
        'footer-pattern': (parsed) => {
          const body = parsed.footer || parsed.body || '';
          const footer = body.split('\n').pop().trim();
          const footerRegex =
            /^Fixes:\s*([A-Z]+(?:-[A-Z0-9]+)*)(,\s*[A-Z]+(?:-[A-Z0-9]+)*)*\s*$/;
          if (!footerRegex.test(footer)) {
            return [
              false,
              'Footer must start with "Fixes: " followed by one or more ticket IDs (e.g., Fixes: DE-123, DE-456).',
            ];
          }
          return [true];
        },
      },
    },
  ],
  rules: {
    'footer-pattern': [2, 'always'],
  },
};
```

## 🔖 **Pull Request Template**

📄 `.github/pull_request_template.md`

```md
<!---
First of all, thank you for your contribution! 😄
For requesting to pull a new feature or bugfix, please send it from a feature/bugfix branch based on the `main` branch.
Before submitting your pull request, please make sure the checklist below is confirmed.
Thank you!
-->

# 📝 Description
<!-- Add a brief description -->

# 🔗 JIRA LINK
<!-- Please add your Jira ticket ID -->

# 📦 Artifacts
<!-- Please attach the artifacts for the changes -->

# ☑️ Self-Check before Merge
- [ ] My code and commits follow the contribution guidelines
- [ ] Added artifacts in the PR for new or updated components
- [ ] Added test for all new components
```

> 📘 GitHub will automatically use this template when opening a new PR.

📚 **Further Reading**

* [Conventional Commits](https://www.conventionalcommits.org)
* [Commitlint](https://commitlint.js.org)
* [Husky](https://typicode.github.io/husky)
* [About PR Templates](https://docs.github.com/en/github/building-a-strong-community/about-issue-and-pull-request-templates)
