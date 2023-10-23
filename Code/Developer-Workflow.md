# Developer Workflow

This guide will cover 3 facets of our workflow:

1. Writing code
2. Having a clean and thorough commit history
3. Reviewing and merging code into deployable branches via Pull Requests

## The coding process

### Always do your work in a feature branch

Our defualt branch is `dev`, and your work-in-progress should not be done on `dev`. Before making any changes, create a new issue to request a feature or report a bug and create a new branch for that issue.

1. Start with a clean `dev` branch.
```
$ git checkout dev
$ git pull origin dev
```
2. Checkout to `your-feature-branch`.
```
$ git checkout your-feature-branch
```
### Make incremental commits throughout your process

Making incremental commits is a good practice when working with GitHub. It allows us to track the history of our project and makes it easier to collaborate with others.

1. Add the changed file to the staging area:
```
$ git add changed_file.dart
```
2. Make periodic commits with proper massages following our [commit message conventions](https://github.com/JoyfulTechLauncher/joyfulfashionista_app_beta#commit-message-conventions):
```
$ git commit -m "fix: a bug"
```
3. Push your commits to the remote
```
$ git push origin your-feature-branch
```
### The review process

The review process, often facilitated through Pull Requests (PRs) in GitHub, is a crucial part of the software development workflow. They allow us to get feedback on our changes, to isolate work-in-progress from deployed environments.

Ultimately your commits from `your-feature-branch` will need to get merged into `dev` in order for them to be deployed to our application’s production environment.

1. **Invite Code Review:**
   After you've pushed your code and commits to your repository, you can invite other developers to review it. Reviewers will look at your code for correctness, style, and potential issues.

2. **Opening a Pull Request (PR):**
   To compare your feature branch (`your-feature-branch`) against the main branch (`dev`), you open a Pull Request. The PR allows for a side-by-side comparison of the changes between the two branches.

3. **Setting Base and Compare Branch:**
   In the PR, you should specify `dev` as the base branch (the destination), and `your-feature-branch` as the compare branch (the source). This indicates that you want to merge the changes from `your-feature-branch` into `dev`.

4. **Code Review:**
   Our team members review the code, provide feedback, and make comments on the PR. You may need to address any issues or make necessary changes based on the feedback.

5. **Accept & Merge:**
   Once the code has been tested and reviewed and any conflicts are resolved, we will accept and merge the PR. This action incorporates the changes from `your-feature-branch` into `dev`. We will also delete the source branch (`your-feature-branch`) after merging.

By following these steps, we maintain a structured development workflow, ensure code quality through reviews, and seamlessly integrate new features or fixes into our codebase.

