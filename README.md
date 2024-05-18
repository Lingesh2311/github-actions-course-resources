# I. 🐱‍💻📚 Git and GitHub Basics

## 🛠️💡Helper commands

### Basic 🛠️

```shell
>> git init # will initialize the project with git and its magic!
>> git add <file(s)> # will stage the changes for next commit
>> git commit -m <commit_message> # will commit the changes
>> git status # will show the status of the changes in git that are logged
>> git log # gives the chronologically ordered list of commits, and logs all commits older than the current commit we have checked out
>> git checkout <id> # This will be temporarily moving to a different commit
>> git revert <id> # This will revert the changes of the particular commit will be removed (not delete) - there is a new commit made to undo the commit changes
>> git reset --hard <id> # This will delete the data of the previous commits and will rewrite the history !!
```

### Branching 🌿

```shell 
>> git branch <branch_name> # creates a new branch
>> git checkout -b <branch_name> # will create a new branch and checkout the same
>> git checkout <branch_name> # will switch to an existing branch specified. If there are uncommitted changes in the current change, Git will allow to switch only if uncommitted changes do not conflict with the branch you are switching to
>> git merge  <branch_name> # merges the changes from the branch back to main branch
>> git branch # lists the branches and the *<green_branch_name> will indicate the current branch you are working on
>> git branch -D <branch_name> # will delete the branch and the commits that are part of it
```

### Working with GitHub 🔧

```shell
>> git remote add <local_name> <remote_repo_url> # will add a remote repository like GitHub, usually local_name = origin
>> git remote set-url origin git@github.com:custom_username/repository.git # will change the URL or origin remote to the new SSH URL with custom username
>> git push  # will add the local commits to the remote repository
>> git push --set-upstream origin main # will set up tracking and push local commits to the remote repository branch named "main"
>> git pull # will pull the commits from the remote repository which are not present locally
```

## 🤔💬 Q & A's

<details>
  
<summary><h3> What is the meaning of <code>HEAD->main</code> ? </h3></summary>

The `HEAD` is the reference to the currently checked-out branch or commit. `main` refers to the name of the branch. When you see `HEAD-main` it means that the `HEAD` pointer is currently pointing to the `main` branch. This typically occurs when you are on the `main` branch and have made changes or are viewing the history of commits on that branch. It is a way that Git indicates which branch you are currently working with or viewing. In Git, `main` is the default branch (instead of the `master`, which was more commonly used in the past).

</details>

<details>

<summary><h3> What is the difference between <code>rebase</code> and <code>merge</code> ? </h3></summary>

#### Merge:
1. **Merge `feature` into `main`:**
   - With merge, you bring the changes from `feature` branch into `main` branch.
   - When you merge `feature` into `main`, Git creates a new commit on `main` that combines the changes from both branches.
   - This creates a merge commit that has two parent commits: one from `main` and one from `feature`.
   - This method preserves the commit history of both branches but adds a merge commit.

```sh
git checkout main
git merge feature
```

#### Rebase:
1. **Rebase `feature` onto `main`:**
   - With rebase, you move the entire `feature` branch to begin from the tip of `main` branch.
   - Git rewinds the changes made in `feature`, switches to `main`, applies the changes from `main`, and then applies the changes from `feature` on top of it.
   - This results in a linear commit history without any merge commits.
   - It essentially replays the changes made in `feature` as if they were made directly on top of the latest `main` commit.

```sh
git checkout feature
git rebase main
```

#### Example:

Let's say `main` has commits A, B, and C, and `feature` has commits X, Y, and Z. After merging and rebasing, the commit history might look like this:

##### Merge:

```
              ┌─── Merge Commit (M) ─┐
             /                        \
main:  A ── B ── C ── M ─────────────────
                       \            /
feature:               X ── Y ── Z
```

##### Rebase:

```
main:  A ── B ── C ─────────────────────────
                                 \
feature:                         X' ── Y' ── Z'
```

In the merge scenario, there's a merge commit (`M`) that integrates changes from both branches. In the rebase scenario, the commits from `feature` are applied directly on top of `main` without any additional merge commits.

#### Summary:

- **Merge:** Integrates changes from one branch into another, creating a merge commit.
- **Rebase:** Moves the entire branch to begin from the tip of another branch, creating a linear commit history.

**Both approaches have their use cases. Merge is generally used for preserving context in a collaborative environment, while rebase is used for maintaining a clean and linear commit history.**

</details>

<details>

<summary><h3>What is the use of <code>git push --set-upstream origin main</code>?</h3></summary>

The command `git push --set-upstream origin main` is used to set up a tracking relationship between the local branch and the remote repository branch. Here's what it does:

1. **Push Changes to Remote Repository (`git push`):**
   - The `git push` command is used to upload local branch commits to a remote repository.

2. **Set Upstream Branch (`--set-upstream` or `-u`):**
   - The `--set-upstream` or `-u` option establishes a connection between the local branch and the corresponding branch on the remote repository.
   - This option sets the upstream branch for the current local branch, meaning that future `git push` and `git pull` commands will use this relationship by default.

3. **Specify Remote Repository (`origin`):**
   - `origin` is the name of the remote repository. It's a common default name for the remote repository that was cloned from or added as a remote.

4. **Specify Branch Name (`main`):**
   - `main` is the name of the branch on the remote repository where the changes will be pushed.

#### Use Case:
- Suppose you've created a new branch locally and made some commits on it.
- When you try to push these changes using `git push`, Git will prompt you to specify the remote repository and branch to push to because it doesn't know where to push the changes.
- By using `git push --set-upstream origin main`, you're specifying that the local branch should track the `main` branch on the `origin` remote repository.
- After this setup, future `git push` commands on this branch will automatically push changes to the `main` branch on the `origin` repository without needing to specify the remote and branch every time.

In summary, `git push --set-upstream origin main` is used to establish a tracking relationship between a local branch and a branch on a remote repository, simplifying future push and pull operations.

</details>
