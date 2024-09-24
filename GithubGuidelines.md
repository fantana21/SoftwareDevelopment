# GITHUB
## HOW TO - GUIDELINES

## GITHUB WORKFLOW

### Branches
For every new feature, bugfix, or enhancement, a new branch must be created. The name of the branch should be short and describe what the new branch is going to change in the codebase. The name of the branch must be written in camelcase. For example, “fix-db-connection” or “add-tcp-connection”.

#### Create branch
1.  `git switch -c [branch-name]`

After a branch has been merged into the main branch, it should be removed from the local repository as well as the remote one.

#### Remove branch
1. `git branch -D [branch-name]` # remove locally
2. `git push origin --delete [branch-name]` # remove on remote

### Commits
A commit should address a specific problem in the codebase. Do not commit multiple unrelated changes in one commit. Describe the commit in less than 72 characters (ideally under 50). The commit message should describe what has changed and be written in clear English with correct spelling. Start the message with a capital letter. Use action words like "Fix", "Add", "Rename", etc. Prefer "Fix" over "Fixed".

For more details, visit: [How to Write a Git Commit Message](https://cbea.ms/git-commit/).

#### Create commit
1. `git commit -m "Custom Message"`
2. `git push`

Alternatively, use built-in Git support in Visual Studio Code or GitHub Desktop.

---

## Issues
An issue represents a task, bug, or feature request that needs to be addressed in a codebase. Issues help track work and facilitate collaboration. Each issue should have a descriptive title and detailed explanation. Assign labels, milestones, and assignees to provide more context.

### Creating an Issue
1. Navigate to the "Issues" tab in your repository.
2. Click on the "New Issue" button.
3. Provide a clear and concise title.
4. Add a detailed description explaining the issue, steps to reproduce (if it's a bug), or objectives (if it's a feature or task).
5. Assign the issue to relevant team members or link it to a milestone.

### Best Practices
- Use clear, action-oriented titles (e.g., "Fix login button bug").
- Add detailed descriptions with relevant screenshots or logs.
- Use labels like "bug", "enhancement", or "documentation" to classify the issue.
- Assign only relevant team members to avoid responsibility overlap.

---

## Pull Requests
A pull request (PR) is a request to merge code changes from one branch into another, typically from a feature or bugfix branch into the main branch. PRs are essential for code review and collaboration, allowing team members to review, comment, and discuss changes before merging them into the main codebase.

### Creating a Pull Request
1. Push your changes to a remote branch.
2. Navigate to the "Pull Requests" tab in your repository.
3. Click on "New Pull Request".
4. Select the base branch (e.g., `main`) and the compare branch (the branch with your changes).
5. Add a clear title and detailed description explaining the changes.
6. Assign reviewers to the pull request.
7. Submit the pull request.

### Best Practices
- Ensure your branch is up-to-date with the latest version of the main branch before creating a pull request.
- Write a clear, concise title and description explaining the changes.
- Add references to issues (e.g., "Fixes #123") to automatically close them upon merging.
- Keep pull requests small and focused on a single feature or fix to make reviews easier.
- Reviewers should provide constructive feedback and ensure the code meets project standards.

---

## Releases
A release is a version of the codebase that is packaged and made available to users. Releases typically contain a specific set of features, bug fixes, or improvements and are tagged with version numbers following semantic versioning (e.g., `v1.0.0`). Releases help distribute software in a structured and traceable manner.

### Creating a Tag for a Release (C++ CMake Projects)
In C++ CMake projects, the tag is created by updating the version in the `CMakeLists.txt` file to the latest version.
1. Change the version in `CMakeLists.txt`:
   `project(MountControlUnit VERSION X.X.X LANGUAGES C CXX)`

In the commit for the release, only this line is changed. The commit message must follow this format:
`Release version X.X.X`

After the commit, the tag for the release can be created:
1. `git tag X.X.X`
2. `git push --tags`

This tag will be used for the following release.

### Creating a Release
1. Navigate to the "Releases" tab in your repository.
2. Click on "Draft a new release".
3. Select an existing tag or create a new one (e.g., `v1.1.0`).
4. Add a title and description for the release. Include a changelog to outline new features, bug fixes, and other updates.
5. Click on "Publish Release" to make it available.

### Best Practices
- Follow [Semantic Versioning](https://semver.org/) for tagging releases (e.g., `v1.0.0`, `v1.1.0`).
- Include a detailed changelog listing all major updates, features, and fixes.
- Ensure the release undergoes thorough testing before publishing.
- If releasing to production, ensure rollback procedures are in place in case of issues.

---

## Example Workflow
TODO: Add code
1. **Create Issue**
2. **Create Branch**
3. **Create Commit**
4. **Create Pull Request**
5. **Merge Pull Request**
6. **Delete Branch**
7. **Create Tag**
8. **Create Release**

---

## GITHUB ACTIONS
TODO: Add details for GitHub Actions.
