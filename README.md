# Git Workflow

## Introduction

We should follow a feature branch workflow with merges going into a single mainline branch trunk-->(master).
This is a simple workflow, but there are specifics on how we handle conflicts and pull requests.

## Creating a Feature Branch

When using this workflow, you start on the `master` branch, make sure it is updated, and then create a feature branch to start your work from.

* Checkout the `master` branch
    ```bash
    git checkout master
    ```

* Ensure that it is up-to-date with the remote `master` branch
    ```bash
    git pull origin master
    ```

* Create a new branch with the name reflecting what you are working on
    ```bash
    git checkout -b updating-readme-usage
    ```

* Start developing

## Branch Naming Conventions

For branch naming, we keep it simple, all you need is a phrase that makes sense for what you are working on and words separated by a dash (`-`).

Example:

    ```bash
    # Branch created for working on updating documentation on usage
    update-docs-usage
    ```

This naming convention makes it simple to know what branches are available and what they are meant to be for.

_Try to avoid using a slash ( `/` ) character in your branch names. It has caused issues with git libraries and the pipeline plugin in the pastlives._

## Commit Conventions

Generally, we follow commit conventions similar to link:http://chris.beams.io/posts/git-commit [these] with the exception of #6 on there.

## Dealing with Conflicts

Often times, you will have a branch that has been under development for longer than a day.
That can result in your branch being out-of-date with the `master` branch.

We update our branches with the rebase model rather than the merge model.
What this means is that if you have a branch with commits, it will place those commits on top of what was on master.
This gives a good look at the history of commits in order that they were merged into the `master` branch.

You can see the link:https://www.atlassian.com/git/tutorials/merging-vs-rebasing [merge vs. rebasing] article for more information on the difference between the two.

You can update your branch and resolve conflicts with the following steps:

* Ensure that no changes are staged to commit and you are on your branch.
    ```bash
    # Look for nothing in the "staged for commit" section
    git status
    ```
* Rebase with the `master` branch.
    ```bash
    git pull --rebase origin master
    ```
* If you have conflicts, resolve those and then add the file(s) changed.
    ```bash
    git add path/to/file/changed
    ```
* Continue the rebase process. If more conflicts occur, repeat the previous step.
    ```bash
    git rebase --continue
    ```
* Once all conflicts are resolved, you should see your latest commit message show up in the output. You should now be able to push up to your branch with the updated commits from `master`.
    ```bash
    # Force push because the commit history has changed
    git push -f origin your-branch-name
    ```

### Note

You should avoid using the force push (`-f`) option to the `master` branch.
You should ONLY use force push for feature branches you are working on.

* Yay! You now have an updated branch.

## Updating Pull Requests

When a pull request you have submitted gets comments, there are typically changes you need to make in order to conform to those comments.
Once you have finished making those changes, DO NOT make a new commit unless it is including a brand new set of information and is unrelated to the comments on the pull request.
Instead, you can amend your previous commit with the new changes. So, the history and commit message remains the same, but your slight fixes are in there.

The reason for doing this is to avoid commits that are not useful like "addressed pr comments".
Rather, it is revised version of the commit you want other developers to see after going through a peer review process.
It also keeps the commit history clean and easy to navigate.

* Make your changes and add the changes to be staged for commit
    ```bash
    git add path/to/file/changed
    ```
* Amend your commit message so that it includes the new file changes
    ```bash
    git commit --amend
    ```
  * That will bring up your default editor for git. Save and exit it with the same commit message unless you feel the need to modify it and add on to the body of the message.
* Push the branch up with the modified commit
    ```bash
    git push -f origin your-branch-name
    ```
* Refresh the pull request and it should have your updated commit and files
