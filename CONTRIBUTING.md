# Contributing

## Ownership and Copyright

As this project is [copyrighted](COPYRIGHT.md), contributions are only accepted from contributors who are named in an active employment agreement, contractor agreement, or other work-for-hire agreement with Sarana Innovation Labs BV ("Company", "We", "Us").

Company is the sole owner of all contributions to this project. Contributors are not entitled to any compensation for contributions to this project beyond what is outlined in their employment agreement, contractor agreement, or other work-for-hire agreement with Company.

## Code of Conduct

We expect contributors to make participation in this project a harassment-free experience for everyone, regardless of age, body size, visible or invisible disability, ethnicity, sex characteristics, gender identity and expression, level of experience, education, socio-economic status, nationality, personal appearance, race, caste, color, religion, or sexual identity and orientation.

We expect contributors to act professionally and respectfully in all interactions with other contributors, users, and maintainers of this project.

Examples of behavior that contributes to creating a positive environment include:

- Using welcoming and inclusive language
- Being respectful of differing viewpoints and experiences
- Gracefully accepting constructive criticism
- Focusing on what is best for the project
- Showing empathy towards other contributors

Examples of unacceptable behavior include:

- The use of sexualized language or imagery and unwelcome sexual attention or advances
- Trolling, insulting/derogatory comments, and personal or political attacks
- Public or private harassment
- Publishing others' private information, such as a physical or electronic address, without explicit permission
- Other conduct which could reasonably be considered inappropriate in a professional setting

This Code of Conduct applies within all project spaces, and it also applies when an individual is representing the project or its community in public spaces. Examples of representing a project or community include using an official project e-mail address, posting via an official social media account, or acting as an appointed representative at an online or offline event.

In its sole discretion, Company pledges to enforce this Code of Conduct and will assign consequences for unacceptable behavior, including termination of any employment agreement, contractor agreement, or other work-for-hire agreement with Company.

## Submitting Contributions

We've adopted the [Conventional Commits](https://www.conventionalcommits.org/) specification for commit messages. This specification defines a set of rules for creating an explicit commit history; which makes it easier to write automated tools on top of. But more importantly, it provides an easy set of rules for creating human readable change logs.

### Setup Your Environment

To contribute to this project, you must install [`commitlint`](https://github.com/conventional-changelog/commitlint)

```bash
npm install --save-dev @commitlint/config-conventional @commitlint/cli
```

Configure commitlint to use conventional config:

```bash
echo "export default { extends: ['@commitlint/config-conventional'] };" > commitlint.config.js
```

To use commitlint you need to setup commit-msg hook:

```bash
npm install --save-dev husky
npx husky init
```

Add commit message linting to commit-msg hook

```bash
echo "npx --no -- commitlint --edit \$1" > .husky/commit-msg
```

It’s all done. Commitlint is now validating all of your commit messages and helping you comply with the standards of this project.

### Branching

Our branching model is based on Gitflow and streamlined to fit the needs of this project. Under this  model, contributors create a feature branch and delay merging it to the `main` branch until the feature is complete. This model is designed to help us manage larger projects with multiple contributors over a longer period of time.

We assign very specific roles to different branches. In addition to `feature` branches, we use individual branches for preparing, maintaining, and recording releases.

#### Develop and Main Branches

![Gitflow](docs/images/gitflow-how-it-works.svg)

Instead of a single `main` branch, this project uses two branches to record the history of the project. The `main` branch stores the official release history, and the `develop` branch serves as an integration branch for features. It's also convenient to tag all commits in the `main` branch with a version number.

The `develop` branch will contain the complete history of the project whereas `main` will contain an abridged (squashed) version. Contributors are not allowed to commit directly to the `main` or `develop` branches. Instead, use a `feature` branch and submit a pull request to the `develop` branch when the feature is complete.

#### Branch Prefixes

We use prefixes to differentiate the types of branches in this project. The following prefixes are used:

| Prefix | Description |
|--------|-------------|
| `feature/` | Feature branches |
| `release/` | Release branches |
| `hotfix/` | Hotfix branches |
| `support/` | Support branches |
| `docs/` | Documentation branches |

#### Feature Branches

![Gitflow](docs/images/gitflow-feature-branches.svg)

Each new feature should reside in its own `feature` branch. Instead of branching from `main`, feature branches use `develop` as their parent branch. When a feature is complete, it gets merged back into `develop`. Features should never interact directly with `main`.

Feature branches typically exist in developer repos only, not in origin. This is because all features are merged into `develop` through pull requests. Once a feature has been merged, the feature branch is deleted. To create a feature branch, run:

```bash
git checkout develop
git checkout -b feature/[scope]/[description]
```
Do not include the `[]` brackets in the branch name. Replace `[scope]` with the [scope](#scope) of the feature and `[description]` with a short description of the feature. For example:

```bash
git checkout develop
git checkout -b feature/azure/resource-groups-template
```
> [!NOTE]
> The branch name should be short and descriptive, using all lower-case letters. Use dashes (`-`) to separate words.

Feature branches should be created from the latest commit of the `develop` branch. It is up to the contributor to ensure that their local `develop` branch is up to date before creating a feature branch. To update your local `develop` branch, run:

```bash
git fetch -p origin
git rebase origin/develop develop
```
#### Release Branches

![Gitflow](docs/images/gitflow-release-branches.svg)

Making `release` branches is another straightforward branching operation. Like `feature` branches, `release` branches are based on the `devekop` branch.

Once `develop` has acquired enough features for a release, a `release` branch is created from `develop` to start the next release cycle. No new features can be added to `release`. Only bug fixes, documentation, and other tasks related to release can go on `release`.

To create a `release` branch, run:

```bash
git checkout develop
git checkout -b release/[version]
```

Do not include the `[]` brackets in the branch name. Replace `[version]` with the version number of the release.

Once it's ready to ship, the `release` branch is merged into `main` (via pull request) and tagged with the version number. It is also merged back into `develop`, which may have progressed since `release` was initiated.

Once `release` is merged into `main` and `develop`, the `release` branch is deleted until the next release cycle.

#### Hotfix Branches

![Gitflow](docs/images/gitflow-hotfix-branches.svg)

A `hotfix` branch is used to quickly patch production releases. They are based on `main` instead of `develop`. As soon as tbe fix is complete, it should be merged into both `main` and `develop` (or the current `release` branch), and `main` should be tagged with an updated version number.

Having a dedicated line of development for bug fixes lets the team address issues without interrupting the rest of the workflow or waiting for the next release cycle.

To create a `hotifx` branch, run:

```bash
git checkout main
git checkout -b hotifx/[scope]/[description]
```

Do not include the `[]` brackets in the branch name. Replace `[scope]` with the [scope](#scope) of the hotfix and `[description]` with a short description of the hotfix.

#### Support Branches

Support branches are used to maintain multiple versions of the same software. For example, if you need to maintain version 1.0 and 2.0 of your software in production, you can create a `support/1.x` branch from `main` to maintain version 1.0 and a `support/2.x` branch from `main` to maintain version 2.0. You can then create `hotfix` branches from the appropriate `support` branch to patch each version of the software.

#### Documentation Branches

Documentation branches are used to update the documentation of the project. They are based on `develop` and should be merged back into `develop` when complete.

### Commit Message Format

> [!NOTE]
> The commit message format is enforced by the Git hook `commit-msg` and will be rejected if it does not comply with the standards of this project. If you are using Visual Studio Code, you can use the [Conventional Commits](https://marketplace.visualstudio.com/items?itemName=vivaxy.vscode-conventional-commits) extension to help you comply with the standards of this project.

You can view the [Conventional Commits Rules](https://commitlint.js.org/#/reference-rules) for more information. Every commit message should be structured as follows:

```
<type>(<scope?>): <description!>
<BLANK LINE>
<body?>
<BLANK LINE>
<footer?>
```

#### Type

The `type` is mandatory and indicates the type of change you are committing. The `type` must be one of the following:

| Type | Title | Description |
|------|------|-------------|
| `feat` | Feature | A new feature |
| `fix` | Bug Fixes | A bug fix |
| `docs` | Documentation | Documentation only changes |
| `style` | Styles | Changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc) |
| `refactor` | Code Refactoring | A code change that neither fixes a bug nor adds a feature |
| `perf` | Performance Improvements | A code change that improves performance |
| `test` | Tests | Adding missing tests or correcting existing tests |
| `build` | Builds | Changes that affect the build system or external dependencies (example scopes: gulp, broccoli, npm) |
| `ci` | Continuous Integrations | Changes to our CI configuration files and scripts (example scopes: Travis, Circle, BrowserStack, SauceLabs) |
| `chore` | Chores | Other changes that don't modify src or test files |
| `revert` | Reverts | Reverts a previous commit |

#### Scope

The `scope` is optional and can be anything specifying the place of the commit change. For example `docs`, `README`, `CONTRIBUTING`, `LICENSE`, `package.json`, `server`, `client`, etc. You can use `*` when the change affects more than a single scope.

#### Description

The `description` is mandatory and must be a short description of the change. Use the imperative, present tense: "change" not "changed" nor "changes". Don't capitalize the first letter. No dot (`.`) at the end.

#### Body

The `body` is optional and is used to explain the what and why of a commit, not the how. The body should include the motivation for the change and contrast this with previous behavior.

#### Footer

The `footer` is optional and is used to reference issue tracker IDs. Breaking changes should start with the word `BREAKING CHANGE:` with space or two newlines.

> [!NOTE]
> Make sure you’ve formatted the Jira issue key correctly, using capital letters. For example, `JRA-123`, not `jra-123`.

### Sign Your Work

To indiciate that you agree in whole with the statements made in this document and all policies governing this project, you must "sign off" each commit by adding a line with your full name (given name, surname) and e-mail address to every commit message. You must use your real name and a valid e-mail address. The format of the line is:

```bash
Signed-off-by: Jane Doe <jane.doe@example.com>
```

Signing off your commit represents your sigbature and certifies that you are either the author of the contribution or have the right to submit it under the terms of the applicable agreement with Company.

If you set your `user.name` and `user.email` as part of your Git configuration, you can sign your commit automatically with:

```bash
git commit -sm "commit message"
```

To sign off your last commit from the command line, run:

```bash
git commit --amend --signoff
```

### Submit Your Work

You are responsible for keeping your local repository current with the remote repository. Doing so will help you avoid discovering merge conflicts when you submit a pull request. Pull requests with merge conflicts will be rejected by the project team.

#### Rebase From Origin

> [!IMPORTANT]
> This project uses `rebase` to update local branches from origin. Never use `pull` to update your local repository.

To update a branch in your local repository from the remote, run:

```bash
git fetch -p origin
git rebase origin/[branch] [branch]
```

For example, to rebase the `develop` branch before creating a new `feature` branch, run:

```bash
git fetch -p origin
git rebase origin/develop develop
```

The major benefit of `rebase` is a much cleaner project history. It eliminates unnecessary merge commits required by `merge`. It also maintains a perfectly linear project history. You can follow the tip of `feature` all the way to the beginning of the project without any forks.

Another important role of `rebase` is to incorporate the latest changes on a parent branch like `develop` into your `feature` branch.

To rebase changes from `develop` into a `feature` branch, run:

```bash
git fetch -p origin
git rebase origin/develop feature/[branch name]
```

Git `rebase` will update your `feature` branch by rebuilding the commit history in sync with the remote `develop` branch while keeping the commits of your `feature` branch at the tip.

#### Submit Pull Requests

This project requires all contributors to submit work via pull request. The `main` and `develop` branches are maintained by the project leads by approving and merging pull requests.

The pull request title should match the name of the branch you are submitting, following the naming conventions outlined in this document. GitHub will combine your commit messages into the descriptiom of the pull request automatically in most cases. Otherwise, you must provide an outline of the changes included in your branch as well as any references to Jira issues that are affectes by your branch.

If your pull request is approved, a project lead will squash and merge your work into the parent branch. Squashing compacts all the commits of your `feature` branch into a single nee commit on the parent (`develop`) branch. Work-in-progress commits are helpful when working on a `feature` branch, but they aren't necessarily important to retain in the Git history.

If you're not familiar with creating pull requests, please follow the guide on [GitHub](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request?tool=webui).