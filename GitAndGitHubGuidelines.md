# Git and GitHub guidelines

### Contents

- [Git commit messages](#git-commit-messages)
- [Git workflow](#git-workflow)
- [Repository names](#repository-names)
- [Other Git conventions](#other-git-conventions)
- [Git config](#git-config)
- [GitHub workflow](#github-workflow)
- [GitHub releases](#github-releases)


## Git commit messages

Writing good commit messages is very important. Therefore, we follow the [7 rules for
great commit messages](https://cbea.ms/git-commit/#seven-rules). I encourage you to check
out the rest of the [related blog post](https://cbea.ms/git-commit/) too. It goes into
much more detail on how to write good commit messages.

You can also adhere to the [Conventional Commits
specification](https://www.conventionalcommits.org/en/v1.0.0/) but with the following
additional constraints:

- Capitalize the type and the description: `Fix: Prevent division by 0`
- Always use `!` in the subject line *and* `BREAKING CHANGE:` in the body for breaking
  changes

The initial commit of a repository is free from these restrictions. Instead, roll a d12
(12-sided die) to determine its commit message from [this
list](https://gist.github.com/fguisso/1403912bd1b8ff53f4a7919f0bb9399a). If you don't have
a d12 at hand, just google "roll 1d12".


## Git workflow

We use [trunk-based
development](https://www.atlassian.com/continuous-delivery/continuous-integration/trunk-based-development)
to support CI/CD (continuous integration/continuous delivery). We also aim for a
semi-linear history, because I think it's the cleanest and clearest. This means:

- Small, frequent changes are merged to the trunk (= main branch)
- The main branch always contains functional software that passes all tests
- Work on new features, bug fixes, etc., is done on new branches
- When work is completed and all tests pass, the branch is rebased onto main and then
  merged with a merge commit. This gives the previously mentioned semi-linear history. I
  encourage you to clean up the branch history before merging, but squashing all commits
  into one is not required. Also, don't forget to delete the branch after merging, both
  locally and on the remote repository.


## Repository names

Repository names should be prefixed with the project name (if applicable) using correct
English spelling and capitalization followed by an underscore. The style for the rest of
the name is determined by the ecosystem in which the repository belongs. In Python
packages must be in lowercase or snake_case, so you should use that case for the
repository name. In C++ we use PascalCase for files and folders and therefore also for
repository names. Vcpkg uses kebap-case for its ports and Git registries. If you are
unsure what to use, correct English spelling and capitalization with underscores instead
of spaces is the default. Component and standard names like TE0706-04 or CV7-E can always
be spelled and capitalized correctly to enhance readability. Here are some examples of
correct repository names:
- `Bullseye_FireControlSystem`: a C++ repo for project Bullseye
- `EagleEye_playground`: a Python repo for project EagleEye
- `LEO2VLEO_FPGA_CCSDS_123.0-B-1`: a VHDL repo for project LEO2VLEO implementing the
  CCSDS 123.0-B-1 standard


## Other Git conventions

- **Branches** must use kebap-case and be named after the work being done. Since branches
  are short-lived (they are deleted after the merge), use short names like `ci`,
  `presets`, or `readme`.
- **Version tags** are added with `git tag -a x.y.z -m "Version x.y.z"`, where `x.y.z` is
  the [semantic version](https://semver.org/) number (see [this section](#git-config) for
  a convenient Git alias). This means we use annotated tags without a "v" prefix. The
  reason for that is that `git describe` now gives you a nice human-readable version of
  the current state of the repository (see [this blog
  post](https://dev.to/davidb31/for-version-as-git-tag-use-annotated-tag-not-v-prefix-2i4c)
  for more details). If the version must appear in a file, create special commits for
  updating the version with the message "Update version to x.y.z" committed directly to
  the main branch.
- **Other tags** must be lightweight to avoid showing up when calling `git describe`.


## Git config

Consider adding the following to your global Git config (`git config --global --edit`).

~~~ini
[alias]
  lg1 = log --graph --abbrev-commit --decorate --format=format:'%C(bold red)%h%C(reset) - %C(bold green)(%ar)%C(reset) %C(bold white)%s%C(reset) %C(bold cyan)- %an%C(reset)%C(bold yellow)%d%C(reset)'
  lg2 = log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold cyan)%aD%C(reset) %C(bold green)(%ar)%C(reset)%C(bold yellow)%d%C(reset)%n''          %C(white)%s%C(reset) %C(dim white)- %an%C(reset)'
  lg = "!git lg1"
  re = "!git rebase -i HEAD~$1 #"
  force = push --force-with-lease
  version-tag = "!f() { git tag -a $1 -m \"Version $1\"; }; f"
[rebase]
  autostash = true
~~~

The aliases `lg1`, `lg2`, and `lg` show a nicely formatted and colored git log. If you
often rebase then you'll like the `re` alias and `autostash = true`. The latter causes Git
to automatically push everything on the stash before rebasing and pop it afterward. The
`force` alias is convenient for force pushing, and the `version-tag` alias makes it easy
to adhere to our version tag convention mentioned in [this
section](#other-git-conventions).


## GitHub workflow

The goal is to track all work in a repository with
[issues](https://docs.github.com/en/issues/tracking-your-work-with-issues/about-issues).
These issues are then fixed by raising and merging [pull requests
(PRs)](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests).
Ideally, each issue has a separate PR, but fixing multiple smaller, related issues in one
PR is acceptable.

Conventions for issues and PRs:

- Because of our [Git workflow](#git-workflow) we do not allow squash or rebase merging.
  We only merge PRs with merge commits.
- Merge commit messages are the PR titles.
- PR titles must therefore be written like subject lines of [Git commit
  messages](#git-commit-messages).
- If a PR fixes a single issue and the issue title is written like a commit message
  subject line, use the same title for the PR.
- PR descriptions must list the issues they fix with `Fixes #<issue number>`. List each
  issue on a separate line. This links the issues to the PR. When the PR is merged the
  issues are closed automatically.
- PR descriptions should not duplicate information from the linked issues. If the issue
  description is sufficient, the PR description can just contain the `Fixes #<issue
  number>` lines.
- Issue titles must also be written like commit message subject lines, unless the issue is
  about a problem. In that case, the title should briefly describe the problem, e.g.,
  "Firing solver sometimes crashes after startup".

> [!TIP]
>
> To enforce the first two points, go to Settings > General > Pull Requests. Check "Allow
> merge commits" and under "Default commit message" choose "Pull request title". Uncheck
> "Allow squash merging" and "Allow rebase merging".

TODO: Once we have customized the issue types for our organization, require choosing one
for each new issue.


## GitHub releases

In addition to version tags, we use [GitHub
releases](https://docs.github.com/en/repositories/releasing-projects-on-github/about-releases).
They are created from tags but show up more prominently on the repository's landing page.
GitHub can generate the release title and description automatically. It uses the tag name
as the title and generates a change log from all merged PRs since the previous version.
Our Git and GitHub workflows ensure that all changes are covered with brief, single-line
descriptions. The only thing you need to change is the title since it must read "Version
x.y.z".
