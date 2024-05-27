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
>> git clone <url> <project_name> # will clone the project from the remote repository to the local machine
>> git remote get-url origin # command will output the URL associated with the specified remote repository
```

## 🤔💬 Q & A's

<details>
<summary><h3> What is the meaning of <code>HEAD->main</code> ? </h3></summary>

The `HEAD` is the reference to the currently checked-out branch or commit. `main` refers to the name of the branch. When you see `HEAD-main` it means that the `HEAD` pointer is currently pointing to the `main` branch. This typically occurs when you are on the `main` branch and have made changes or are viewing the history of commits on that branch. It is a way that Git indicates which branch you are currently working with or viewing. In Git, `main` is the default branch (instead of the `master`, which was more commonly used in the past).
</details>

<details>
<summary><h3> What is the difference between <code>rebase</code> and <code>merge</code> ? </h3></summary>

#### Merge

1. **Merge `feature` into `main`:**
   - With merge, you bring the changes from `feature` branch into `main` branch.
   - When you merge `feature` into `main`, Git creates a new commit on `main` that combines the changes from both branches.
   - This creates a merge commit that has two parent commits: one from `main` and one from `feature`.
   - This method preserves the commit history of both branches but adds a merge commit.

```sh
git checkout main
git merge feature
```

#### Rebase

1. **Rebase `feature` onto `main`:**
   - With rebase, you move the entire `feature` branch to begin from the tip of `main` branch.
   - Git rewinds the changes made in `feature`, switches to `main`, applies the changes from `main`, and then applies the changes from `feature` on top of it.
   - This results in a linear commit history without any merge commits.
   - It essentially replays the changes made in `feature` as if they were made directly on top of the latest `main` commit.

```sh
git checkout feature
git rebase main
```

#### Example

Let's say `main` has commits A, B, and C, and `feature` has commits X, Y, and Z. After merging and rebasing, the commit history might look like this:

##### Merge

```
              ┌─── Merge Commit (M) ─┐
             /                        \
main:  A ── B ── C ── M ─────────────────
                       \            /
feature:               X ── Y ── Z
```

##### Rebase

```
main:  A ── B ── C ─────────────────────────
                                 \
feature:                         X' ── Y' ── Z'
```

In the merge scenario, there's a merge commit (`M`) that integrates changes from both branches. In the rebase scenario, the commits from `feature` are applied directly on top of `main` without any additional merge commits.

#### Summary

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

#### Use Case

- Suppose you've created a new branch locally and made some commits on it.
- When you try to push these changes using `git push`, Git will prompt you to specify the remote repository and branch to push to because it doesn't know where to push the changes.
- By using `git push --set-upstream origin main`, you're specifying that the local branch should track the `main` branch on the `origin` remote repository.
- After this setup, future `git push` commands on this branch will automatically push changes to the `main` branch on the `origin` repository without needing to specify the remote and branch every time.

In summary, `git push --set-upstream origin main` is used to establish a tracking relationship between a local branch and a branch on a remote repository, simplifying future push and pull operations.

</details>

# II. 🏗️ GitHub Actions - Basic Building Blocks

### Workflows, Jobs and Steps

- **Workflows** : They are attached to the GitHub repository. They contain one or more jobs. There are **events** which trigger the workflow
- **Jobs** : They define a **Runner** (an execution environment). They can be pre-defined or custom. There are steps which are executed in the runner environment. The jobs can run sequentially/parallely. They can also be driven by some condition.
- **Steps**: They execute a shell script or an action. They can be custom or third

The GitHub workflows are defined as files within the repository and will be part of the repo itself and commits within it.

### Events (Workflow Triggers)

There are tons of repository-related events, some include: `push`, `pull_request`, `create`, `fork`, `issues`, `issue_comment`, etc.
Also, we can control what kind of push should trigger the event. So, for example we can trigger the event when we perform a `push` to a particular branch.

### Actions

An Action is a separate feature from a workflow in the context of GitHub action. It is a (custom) application that performs a (typically complex) frequently repeated task. An alternative but a simple step is the `run` command in GitHub ction which could be a simple shell command that is defined by us. We can use our own custom action or community driven action.

The keyword that we use for running an **action** in the job of a workflow is that we use the `uses` keyword.

Also note when configuring the runner in a GitHub workflow, the application logic is run on the runner. The runner sets up the environment according to the job's requirement (specified in the `runs-on` field). The runner checks out the code (using actions like `actions/checkout`), sets up any related environment variables, and install necessary dependencies. The runner then executes each step defined in the job. Steps can run shell commands directly or use pre-built actions from the GitHub marketplace.

### Jobs

A job is a set of steps that execute on the same runner. Jobs allow you to define stages of your workflow, such as build, test, deploy. Every job has its own runner - its own VM that is totally isolated from other machines and jobs. Jobs within the same workflow can run independently and concurrently unless you specify dependencies between them.

The unique identifiers for the job in GitHub Actions workflow are not built-in keywords; they are arbitrary names that you define to reference each job within the workflow. You choose the identifiers for the jobs. They should be unique within the workflow file. There are no predefined job identifiers like `build`, `test` and `deploy` in GitHub Actions.

### Naming jobs (best practice)

- **Descriptive names:** Use descriptive names for job identifiers to make your workflow easier to understand. E.g. `build_stage` instead of `build`
- **Consistency:** Maintain consistent naming conventions throughout your workflows to improve readability and maintainability.
- **Avoid Special characters:** Stick to alphanumeric characters and underscores (_) for job identifiers to avoid potential issues.

### Expressions

They are used to compute values dynamically within the workflow YAML. They can be used in `if` conditionals, environment variables, and other fields that support dynamic values.

#### Basic Syntax

Expressions are enclosed within `${{  }}`. For example:

```yml
name: CI

on: [push]

jobs:
  example-job:
    runs-on: ubuntu-latest

    steps:
      - name: Print message
        run: echo "Hello, ${{ github.actor }}"

```

In this example, we have `%{{ github.actor }}` which evaluates to the username of the person who triggered the workflow.

**Common operators:**

- Logical Operators: &&, ||, !
- Comparison Operators: ==, !=, <, <=, >, >=
- Arithmetic Operators: +, -, *, /, %
- String Operators: startsWith, endsWith, contains

##### Example with `if` conditional

```yml
jobs:
  example-job:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Run tests
        if: ${{ github.event_name == 'push' && github.ref == 'refs/heads/main' }}
        run: npm test
```

This exaample runs the tests only if the workflow is triggered by a push event to the main branch.

### Contexts

Context provide access to the information about workflow runs, runner environment, jobs and steps. They are available in expressions and can be used to make decisions or pass data between jobs.

**Common Contexts**

1. `github` Context: Contains information aobut the workflow run & the event that triggered it. e.g. `github.actor`
2. `env` Context: Contains the env variable that have been set in the workflow. e.g. `env.<VARIABLE_NAME>`
3. `job` Context: Contains the info about the currently running job. e.g. `job.status`
4. `steps` Context: Contains the result of the previous steps in the current job. e.g. `steps.<step_id>.outputs`
5. `runner` Context: Contains the info about the runner executing the job. e.g. `runner.os`, `runner.arch`

##### Example with Contexts

```yml
name: CI

on: [push]

jobs:
  example-job:
    runs-on: ubuntu-latest

    steps:
      - name: Set up Node.js
        uses: actions/setup-node@v2
        with:
          node-version: '14'

      - name: Check out the code
        uses: actions/checkout@v2

      - name: Run tests
        run: npm test

      - name: Print GitHub Context
        run: echo "This workflow was triggered by ${{ github.actor }} on branch ${{ github.ref }} using commit ${{ github.sha }}"

      - name: Conditional Step
        if: ${{ runner.os == 'Linux' }}
        run: echo "This step runs only on Linux runners"
```

##### Combining Expressions and Contexts

```yml
name: CI

on: [push]

jobs:
  example-job:
    runs-on: ubuntu-latest

    steps:
      - name: Check out the code
        uses: actions/checkout@v2

      - name: Set environment variable
        run: echo "COMMIT_SHA=${{ github.sha }}" >> $GITHUB_ENV

      - name: Use environment variable
        run: echo "The commit SHA is ${{ env.COMMIT_SHA }}"
```

In this example, the `COMMIT_SHA` environment variable is set dynamically using the `github.sha` context and then used in a subsequent step.

##### Summary of Expressions & Contexts

- **Expressions**: Used to compute values dynamically within the workflow. Enclosed within `${{ }}` and support various operators.
- **Contexts**: Provide access to detailed information about the workflow, runner, jobs, and steps. Used within expressions to make workflows dynamic and responsive to runtime data.

## 🤔💬 Q & A's

<details><summary><h3>How can we set the jobs in a workflow to run in sequence / parallel?</h3></summary>

  In GitHub Actions, jobs run in parallel by default. However, you can control the execution order of jobs and run them sequentially by specifying job dependencies using the `needs` keyword. By setting up the dependencies, you ensure that a job will only run after the job it depends on is completed successfully.

### Example Workflow with Sequential Jobs

Here’s an example of how to set up a workflow with three jobs (`build`, `test`, and `deploy`) that run sequentially:

```yaml
name: CI Pipeline

on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Set up Node.js
      uses: actions/setup-node@v2
      with:
        node-version: '14'

    - name: Install dependencies
      run: npm install

    - name: Build project
      run: npm run build

  test:
    runs-on: ubuntu-latest
    needs: build

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Set up Node.js
      uses: actions/setup-node@v2
      with:
        node-version: '14'

    - name: Install dependencies
      run: npm install

    - name: Run tests
      run: npm test

  deploy:
    runs-on: ubuntu-latest
    needs: test

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Deploy to server
      run: ./deploy.sh
```

### Explanation

1. **Job Definitions:**
   - **`build` job:** This job checks out the code, sets up Node.js, installs dependencies, and builds the project.
   - **`test` job:** This job checks out the code, sets up Node.js, installs dependencies, and runs tests. It will only run after the `build` job completes successfully.
   - **`deploy` job:** This job checks out the code and deploys the project. It will only run after the `test` job completes successfully.

2. **Dependencies:**
   - The `needs: build` keyword in the `test` job specifies that the `test` job should run after the `build` job.
   - The `needs: test` keyword in the `deploy` job specifies that the `deploy` job should run after the `test` job.

### Using `needs` for Sequential Execution

- **Defining Dependencies:** Use the `needs` keyword to define which job(s) a job depends on. This ensures that jobs run in the specified order.
- **Sequential Execution:** By chaining dependencies, you can create a sequence of jobs that run one after another.

### Example with More Complex Dependencies

If you have a more complex workflow where multiple jobs need to run sequentially but some jobs can run in parallel within a sequence, you can set up a more complex dependency structure.

```yaml
name: Complex CI Pipeline

on: [push, pull_request]

jobs:
  setup:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Set up environment
      run: ./setup-env.sh

  build:
    runs-on: ubuntu-latest
    needs: setup

    steps:
    - name: Build project
      run: ./build.sh

  lint:
    runs-on: ubuntu-latest
    needs: setup

    steps:
    - name: Run linter
      run: npm run lint

  test:
    runs-on: ubuntu-latest
    needs: [build, lint]

    steps:
    - name: Run tests
      run: npm test

  deploy:
    runs-on: ubuntu-latest
    needs: test

    steps:
    - name: Deploy
      run: ./deploy.sh
```

### Explanation of Complex Example

1. **`setup` job:** This job prepares the environment.
2. **`build` and `lint` jobs:** These jobs both depend on the `setup` job and will run in parallel after `setup` completes.
3. **`test` job:** This job depends on both `build` and `lint` jobs, ensuring that tests are run only after both build and linting are done. Hence, multiple jobs can be placed in a list for the `needs` element.
4. **`deploy` job:** This job depends on the `test` job, ensuring that deployment happens only after tests pass.

### Summary

- **Parallel by Default:** Jobs run in parallel unless dependencies are specified.
- **Sequential Execution:** Use the `needs` keyword to specify dependencies and control the execution order.
- **Complex Workflows:** Combine multiple dependencies to create complex workflows with parallel and sequential job execution.

By defining dependencies using the `needs` keyword, you can control the order in which jobs run in your GitHub Actions workflow, ensuring they execute sequentially as required.

</details>

# III. 🔍 Workflows & Events - Deep Dive 🌊

There are many kinds of events which are repository-related and custom event-related.

![available_events](Resources/available_events.png)

### Event Filtering

Consider this YAML file:

```yaml
name: Events Demo 1
on:
  push:
    paths:
      - 'Code/03 Events/01 Starting Project/**'
  workflow_dispatch:
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Output event data
        run: echo "${{ toJSON(github.event) }}"
      - name: Get code
        uses: actions/checkout@v3
      - name: Install dependencies
        run: npm ci
      - name: Test code
        run: npm run test
      - name: Build code
        run: npm run build
      - name: Deploy project
        run: echo "Deploying..."
```

Here we have the `push` & `workflow_dispatch` event triggers activated on the custom path `Code/03 Events/01 Starting Project/**` but not always we want to run the `Deploy Project` step, we would only want to do that during the push to the main branch. In cases like that we can add an event filter. We can add an `if` conditional to the `Deploy Project` step like so:

- `github.ref == 'ref/heads/main'` : Checks if the reference (branch) being pushed to is the main branch
- `github.event_name == 'push'`: Ensures that the event triggering the workflow is a push event.

A workflow event in GitHub Actions can have multiple activity types associated with it. For example, the `pull_request` event can include activities such as `opened`, `assigned`, `closed`, `reopened`, etc. You can configure the workflow to be triggered or run only when a specific activity type occurs within the event, allowing you to precisely control when your workflow should execute based on the specific action that occurred.

### Skipping Workflow Runs

We can skip the workflow runs triggered by the `push` and `pull_request` events by including a command in the commit message.

Workflows that would otherwise be triggered using `on: push` or `on: pull_request` won't be triggered if you add any of the following strings to the commit message in a push, or the HEAD commit of a pull request:

- `[skip ci]`
- `[ci skip]`
- `[no ci]`
- `[skip actions]`
- `[actions skip]`

To leave the commit message exactly as you entered it, use the `--clean=verbatim` option on your commit.

The skip-checks trailer in a commit message is a convention used to instruct continuous integration (CI) systems, like GitHub Actions, to skip certain checks or workflows for that particular commit. This can be useful in situations where you want to bypass CI checks for a commit that doesn't need them, such as a minor documentation change or a temporary experiment.

#### Example 1 : Simple Commit message with `skip-checks: true` 

```txt
Fix typo in README

This commit corrects a small typo in the documentation.

skip-checks: true
```

#### Example 2 : Commit message with multiple trailers

```txt
Update dependencies and fix security vulnerabilities

This commit updates several dependencies to their latest versions
and addresses security vulnerabilities reported by Dependabot.

Signed-off-by: John Doe <john.doe@example.com>

skip-checks: true
```

## 🤔💬 Q & A's

<details><summary><h3>What are activity types and event filters in a GitHub Workflow?</h3></summary>

The "activity types" and "event filters" are concepts used to control the workflow and how should it be triggered or run based on specific aactions or conditions within an event.

### Activity Types

They refer to a specific action or event that can occur with a broader event type. For example:

- For the `pull_request` event, activity types include `opened`, `closed`, `synchronize`, `reopened` etc.
- For the `issues` event, activity types include `opened`, `edited`, `closed`, `assigned`, `labelled` etc.

Each activity type represents a specific action or change within the context of the event. You can configure your workflow to respond to specific activity types within an event by specifying them in the workflow YAML file.

### Event Filters

Event filters are conditions or criteria that you can use to control when the workflow should be triggered or run based on specific properties of the event. Common event filters include:

- Branch name: Trigger the workflow only when the changes are pushed to a specific branch (e.g. `main`, `develop`).
- Paths: Trigger the workflow only when the changes are made to files matching a specific path or pattern.
- Pull request actions: Trigger the workflow only when certain actions are performed on a pull request. (e.g. `opened`, `closed`, `synchronize`).

Event filters allow you to fine-tune when your workflow should execute based on the context of the event. You can use event filters to ensure that your workflow runs only when certain conditions are met, such as specific branches are affected or specific files are modified.

### Example

```yaml
name: Example Workflow

on:
  push:
    branches:
      - main
      - 'dev-*' # all branches which start with the word dev-
    paths:
      - 'src/**'
    paths-ignore:
      - '.github/workflows/*'
  pull_request:
    types: [opened, synchronize]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2
      - name: Build project
        run: npm run build
```

In this workflow,

- The workflow will run on push events only when changes are pushed to the `main` branch & `dev-*` like branches and affect files in the `src` directory.
- The workflow will also run on `pull_request` events when pull requests are opened or synchronized.

</details>

<details><summary><h3>What is the default behaviour for an action that might be validily triggered by a forked repository?</h3></summary>

By default, Pull requests based on forks **DO NOT** trigger a workflow.

We can have our workflow configured such that it takes into account the restrictoins when dealing with forked repositories:

```yaml
name: Example Workflow

on:
  pull_request:
    types: [opened, synchronize]

jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      contents: read  # Restrict permissions to read-only
    steps:
      - name: Checkout code
        uses: actions/checkout@v2
        with:
          fetch-depth: 0  # Ensure full history is fetched for accurate diffs

      - name: Set up Node.js
        uses: actions/setup-node@v2
        with:
          node-version: '14'

      - name: Install dependencies
        run: npm ci

      - name: Run tests
        run: npm test

      - name: Conditional Deployment
        if: github.event.pull_request.head.repo.full_name == github.repository
        run: echo "Deploying..."
        # Add your deployment script here, but it will only run for pull requests from the same repository
```

- **Conditional Steps** - The `Conditional Deployment` step includes a conditional check to ensure that the deployments only occur for pull requests originating from the same repository (`github.event.pull_request.head.repo.full_name == github.repository`). This prevents the deployment job to be triggered from the forked repos.
- **Fetching full history** - The `fetch-depth : 0` option in the `actions/checkout` step ensures that the full history is fetched, which is useful for calculating the accurate diffs and understanding the full context of changes.
- **Read-only permissions** - The `permissions` key is set to read-only for contents to limit the scope of what the `GITHUB_TOKEN` can do, providing an additional layer of security.

</details>

# IV. 📦 Job Artifacts & Outputs

**Job Artifacts** - These are the outputs/assets generated by the job. e.g. app binary, website files etc. We can take these artifacts and download/use them via GitHub UI / REST API. We can also download & use them in other jobs via actions.

Artifacts help to share the data between jobs in a workflow and store data once the workflow has completed its execution. By default, GitHub stores the build logs and artifacts for 90 days, and this retention period can be customized. We can also upload some job artifacts (log files, core dumps, test results, failures, screenshots, stress test performance output and code coverage results etc).

GitHub provides two actions that you can use to upload/download build artifacts. (`upload-artifact` and `download-artifact`)

### Job Outputs

We can use the `jobs.<job_id>.outputs` to create a `map` of outputs for a job. Job outputs are available to all downstream jobs that depend on this job. Below is an example:

```yaml
jobs:
   job1:
      runs-on: ubuntu-latest
      # Map a step output to a job output
      outputs:
         output1: ${{ steps.step1.outputs.test }}
         output2: ${{ steps.step2.outputs.test }}
      steps:
         - id: step1
           run: echo "test=hello" >> "$GITHUB_OUTPUT"
         - id: step2
           run: echo "test=hello" >> "$GITHUB_OUTPUT"
   job2:
      runs-on: ubuntu-latest
      needs: job1
      steps:
         - env:
              OUTPUT1: ${{ needs.job1.outputs.output1 }}
              OUTPUT2: ${{ needs.job2.outputs.output2 }}
```

The simple values can be passed or when needs to be passed from one step to the next, we can utilize job outputs functionality. Example: Name of a file generated in the previous build step.

The outputs are shared via the `echo '{output-name}={output-value}' >> $GITHUB_OUTPUT` call in the workflow.

### Dependency Caching

This is to create and use caches for dependencies and other commonly reused files. Workflow runs often reuse the same outputs or downloaded dependencies from one run to another. For example, package and dependency management tools such as Maven, Gradle, npm and Yarn keep a local cache of downloaded dependencies.

To cache you can use the GitHub's `cache` action. The action creates and restores a cache identified by an unique key. Alternatively, if you are caching the package managers listed (`setup-node`, `setup-python`, `setup-java`, `setup-ruby`, `setup-go`, `setup-dotnet`) requires a miniml configuration and will cerate and restore the dependency caches for you.

- **Cache purpose**: The cache action aims to store and reuse downloaded dependencies across workflow runs. It focuses on the content of the path you specify, not where those dependencies are used within your project.

- **Global npm cache**: The path `~/.npm` refers to the user's home directory on the runner and specifically the `.npm` folder. This folder is the global cache location for npm on the runner machine. Regardless of the working directory in later steps, npm will automatically access and install dependencies from this cached location if available.

- **Hashing ensures accuracy**: The cache key you use, `deps-node-modules-${{ hashFiles('**/package-lock.json') }}`, considers the content of the `package-lock.json` file. If this file changes, indicating a change in dependencies, the cache key changes as well, forcing a fresh download.

# V. 🔒 Environment Variables & Secrets

This is to avoid the hardcoding part of the confidential and not-to-be shared values.

We can set the environment variables at the workflow/job/step level. To seet a custom environment variable for a single workflow, you can define it using the `env` key in the workflow file. The scope of a custom variable set by this method is limited to the element in which it is defined. You can define variables that are scoped for:

- The entire workflow, by using the `env` at the top level of the workflow file.
- The contents of a job within a workflow, by using the `job.<job_id>.env`
- A specific steps within a job, using the `job.<job_id>.steps[*].env`

Consider this example yaml:
```yaml
name: Greeting on variable day

on:
   workflow_dispatch

env:
   DAY_OF_WEEK: Monday

jobs:
   greeting_job:
      runs-on: ubuntu-latest
      env:
         Greeting: Hello
      steps:
         - name: "Say Hello Mona it's Monday"
           run: echo "$Greeting $First_Name. Today is $DAY_OF_WEEK"
           env:
             First_Name: Mona 
```

### Default Environment Variables

The defaualt environment variables that GitHub sets are available to every step in the workflow. They are not accessible through the `env` context. However, most of the default variables have a corresponding, and similiarly named, context property.You can't overwrite the value of the default environment variables named `GITHUB_*` and `RUNNER_*`.

### Secrets

There are some values which must not be directly placed as environment variables in the workflow file, and these values are known as secrets. We can use the `secrets` context object to access the repository secrets that are set in the repository under the `Settings` tab. For secrets that are stored at the organization level, you can use access policies to control which repositories can use the organization secrets. Org-level secrets let you share secrets between multiple repositories, which reduces the need for creating duplicate secrets. Updating an organization secret in one location also ensures that the change takes effect in all repositories workflows that use that secret.

For secrets stored at the environment level, you can enable the required reviewers to control access to the secrets. A workflow job cannot access environment secrets until approval is granted by required approvers.

When we try to print the secrets, GitHub takes care of hiding it and not will not reveal the actual value to the user who is trying to fetch it.

### GitHub Repository Environments

The idea is that we configure the environments on a repository and the workflow environment can store/attach the secrets onto such specific target environments. Different configurations can be provided to the job (e.g. different passwords) at the environment level.

### Environment Protection Rules & Deployment Branches

We can have environment level protection rules which can be used to configure the manual approvals and timeouts. The possible options are:

- **Required approvers**: Specify the people or teams that may approve the workflow runs when they access the environment.
- **Wait timer**: Set an amount of time to wait before allowing deployments to proceed.

We can also set the **deployment branch patterns** that must be adhered to when the environment specific workflow is to be triggered.

VI. Controlling Workflow and Job Execution ⚙️

The default behaviour of the workflow steps is sequential/parallel and **dependent** which means we might need to override the default execution flow of the job steps in a workflow. The dependent jobs will not be executed (will be cancelled) when a step in a parent job/ parent job itself fails.

### Conditional Jobs and Steps

`If` field can be added to both the jobs and steps. For steps, we have `continue-on-error` field which can help in the execution flow.

### Special Conditional Functions

- `failure()` - This function evaluates to `true`, regardless of the outcome (success, failure, or cancellation) of the referenced job. It is useful when you wan to perform the step irrespective of the previous job's status.
- `success()` - This function evaluates to `true` only if the job it references completed successfully. You can use it to trigger dependent jobs or steps that rely on the successful execution of another job.
- `always()` - This function **always** evaluates to `true`, regardless of the outcome (success, failure, or cancellation) of the referenced job. It is useful when you want to perform a step irrespective of the previous job's status, like cleaning up resources.
- `cancelled()` - This function evaulates to `true` only if the job it references was cancelled. This can be helpful for handling workflows that were manually stopped before execution.

Consider this yaml:

```yaml
- name: Test code
  id: run-test # this id can be used in subsequent steps to control the execution flow
  run: npm run test
- name: Upload test report
  if: failure() && steps.run-test.outcome == 'failure' # comparing the steps context outcome value with 'failure' state to run this step on failure
  uses: actions/upload-artifact@v4.3.3
  with:
    name: test-report
    path: test.json
```

Here we use the `steps.run-test.outcome` context object's value we can control the flow of the run. The special function `failure()` here will take care of running the step `Upload test report` even if there are failing steps before the current step.

The `Upload Test Report` step uses the pre-built action `actions/upload-artifact@v4.3.3`. Actions are reusalbe building blocks for common tasks in workflow. The step only runs under two conditions (represented by the `if` keyword):

- The previous step (`run-test`) must have failed (`failure()`)
- The outcome fo the previous step (`steps.run-test.outcome`) needs to be explicitly 'failure'. This double-checks the previous step's status.
- If both the conditions are met, this step uploads the file `test.json` (assumed to be the test report) to an artifact storage in GitHub. This allows you to access the report later.

### `If` field at the Job level

We can add the `if` field at the job level too. Consider the following yaml file:

```yaml
name: Website Deployment
on:
   push:
      branches:
         - main
jobs:
   lint:
   ...
   test:
   ...
   build:
   ...
   deploy:
   ...
   report:
      needs: [lint, deploy]
      if: failure()
      runs-on: ubuntu-latest
      steps:
         - name: Output information
           run: |
            echo "Something went wrong"
            echo "${{ toJSON(github) }}"
```

Here, the `if` field will be satisfied when any of the job in the workflow fails. And, also it needs all the jobs starting from `lint` to `deploy` to finish before performing the check on the `failure()` status. In essence, the `report` job acts as a post-mortem step in this workflow. It gathers the information (potentially GitHub context) only when there is a deployment failure after successful linting and deployment, providing a central location to identify potential cause of this issue.

Another example of the `if` field can be done in the combination of the `caching` and `install dependencies` steps. Consider the following workflow:

```yaml
name: Website Deployment
on:
   push:
      branches:
         - main
jobs:
   lint:
      runs-on: ubuntu-latest
      steps:
         - name: Get code
           uses: actions/checkout@v4.0.3
         - name: Cache dependencies
           id: cache
           uses: actions/cache@v3.2.0
           with:
              path: node_modules
              key: deps-node-modules-${{ hashFiles('**/package-lock.json') }}
         - name: Install dependencies
           if: steps.cache.outputs.cache-hit != 'true' # this will be true when the previous steps' caching is successful
           run: npm ci
         - name: Lint code
           run: npm run lint
```

The `if` conditional statement in this workflow config controls whether the `npm ci` command (to install dependencies) runs or is skipped.

- `if: steps.cache.outputs.cache-hit != 'true'` : This condition checks the output of the previous caching step named `cache` using the `id` attribute. The specific output it checks is the `cache-hit`.
- If the cache is successfully retreived (`cache-hit` output is `true`), then the `if` condition evaluates to `false`. This means the next step `npm ci` to install the dependencies is skipped. Reusing the cache saves time by avoiding redundant installations.
- However, if the cache retrival fails (cache miss), the `cache-hit` output will be `false`. In this case, the `if` condition become `true`, and the subsequent step (`npm ci`) to install dependencies runs as intended.

The `if` statement implements the caching logic to optimize the workflow execution. It only installs dependencies if the cache miss or there was no usaable cache version available.

### Understanding & Using Matrix Strategies

A matrix strategy lets you use the variables in a single job definition to automatically create multiple job runs that are based on the combination of the variables. For example, you can use a matrix strategy to test your code in multiple versions of a language or on multiple OS's.

#### Using a matrix strategy

Use the `jobs.<job_id>.strategy.matrix` to define a matrix of different job configurations Within the matrix, define one or more variables followed by an array of values. For example, the following matrix has a variable called `version` with the value `[10, 12, 14]` and variable called `os` with the value `[ubuntu-latest, windows-latest]`:

```yaml
jobs:
  example_matrix:
    strategy:
      matrix:
        version: [10, 12, 14]
        os: [ubuntu-latest, windows-latest]
```

A job will run for each of the possible combination of the variables. In this example, the workflow will run six jobs, one for each combination of the `os` and `version` variables.

By default, GitHub will maximize the number of jobs run in parallel depending on runner availability. The order of the variables in the matrix determines the order in which the jobs are created. The first variable you define will be the first job that is created in the workflow run. In the above YAML, the jobs are created in the following order:

- `{version: 10, os: ubuntu-latest}`
- `{version: 10, os: windows-latest}`
- `{version: 12, os: ubuntu-latest}`
- `{version: 12, os: windows-latest}`
- `{version: 14, os: ubuntu-latest}`
- `{version: 14, os: windows-latest}`

A matrix will generate a maximum of 256 jobs per workflow run. This limit applies to both GitHub-hosted and self-hosted runners.

#### Example: Using a single dimension matrix

```yaml
jobs:
   single_d_matrix:
      strategy:
         matrix:
            version: [10, 12, 14]
   steps:
      - uses: actions/setup-node@v4
        with:
           node-version: ${{ matrix.version }}
```

Here we define the matrix variable `version` under the `strategy.matrix` nesting which can take up the values `[10, 12, 14]`. The workflow will run three jobs, one for each value of the variable. Each job will access the `version` value through the `matrix.version` context and pass that value as `node-version` to the `actions/setup-node@v4` action.

#### Example: Using a multi-dimension matrix

```yaml
jobs:
   multi_d_matrix:
      strategy:
         matrix:
            os: [ubuntu-22.04, ubuntu-20.04]
            versionn: [10, 12, 14]
      runs-on: ${{ matrix.os }}
      steps:
         - uses: actions/setup-node@v4
           with:
              node-version: ${{ matrix.version }}
```

In this workflow there are two variables `os` and `version`, there would be siz jobs, one for each combination of the `os` and `version` variable. Each job would set the `runs-on` value to the current `os` value and will pass the current `version` value to the `actions/setup-node` action.

#### Expanding or adding particular matrix configurations using `include` and `exclude`

Using `jobs.<job_id>.strategy.matrix.include` to expand the existing configuraions or to add new configurations. The value `include` is a list of objects. For each object in the `include` list, the key:value pairs overwrite any of the original matrix values.

For example, this matrix:
```yaml
strategy:
   matrix:
      fruit: [apple, pear]
      animal: [cat, dog]
      include:
         -  color: green
         -  color: pink
            animal: cat
         -  fruit: apple
            shape: circle
         -  fruit: banana
         -  fruit: banana
            animal: cat
```

This will result in six jobs with the following matrix combinations:

* `{fruit: apple, animal: cat, color: pink, shape: circle}`
* `{fruit: apple, animal: dog, color: green, shape: circle}`
* `{fruit: pear, animal: cat, color: pink}`
* `{fruit: pear, animal: dog, color: green}`
* `{fruit: banana}`
* `{fruit: banana, animal: cat}`

following this logic:

* `{color: green}` is added to all of the originl matrix cominations because it can be added without overwriting any part of the original combinations.
* `{color: pink, animal: cat}` adds `color:pink` only to the original combinations that include `animal:cat`. This overwrites the `color: green` that was added by the previous `include` entry.
* `{fruit: apple, shape: circle}` adds `shape:circle` only to the original matrix combinations that include `fruit:apple`.
* `{fruit: banana}` cannot be added to any original matrix combination without overwriting a value, so it is added as an additional matrix combination. 

The `matrix` strategy in GitHub actions allows you to create multiple jobs with different configurations. The `include` attribute is used to add additional configurations to the matrix.

1. The `matrix` attribute defines the base configurations. In you example, there are two base configurations: `fruit` and `animal`. This will create a job for each configuration of `fruit` and `animal`, resulting in four jobs:
   
   * `{fruit: apple, animal: cat}`
   * `{fruit: apple, animal: dog}`
   * `{fruit: pear, animal: cat}`
   * `{fruit: pear, animal: dog}`

3. The `include` attribute is used to add additional configurations to the matrix. Each item in the `include` list is a dictionary that defines a new configuration.

   * If the dictionary contains keys that match the base configurations (like `fruit` or `animal`), it will add the new configuration to the corresponding base job. For example `{fruit: apple, shape: circle}` will add the `shape:circle` to the jobs where `fruit: apple`
   * If the dictionary does not contain any keys that match the base configurations, it will create a new job with the given configuration. For example `{color: green}` will create a new job `{color: green}`

4. If the `include` attribute contains duplicate configurations, they will be ignored. For example, `{fruit: banana}` appears twice in our `include` list, but it will create one job.

#### Excluding Matrix Configurations

To remove a specific configuration defined in the matrix, use `jobs.<job_id>.strategy.matrix.exclude`. An excluded configuration only has to be a partial match for it to be excluded. For example, the following workflow will run nine jobs: one job of the 12 configurations, minus the one excluded job that matches `{os: macos-latest, version: 12, environment: production}`, and the two excluded jobs that match `{os: windows-latest, version: 16}`.

```yaml
strategy:
   matrix:
      os: [macos-latest, windows-latest]
      version: [12, 14, 16]
      environment: [staging, production]
      exclude:
         - os: macos-latest
           version: 12
           environment: production
         - os: windows-latest
           version: 16
runs-on: ${{ matrix.os }}
```

## 🤔💬 Q & A's

<details><summary><h3>Can we ignore the errors and failures in GitHub actions?</h3></summary>

When we add the `continue-on-error: true` attribute in the job/step then we are mentioning that job/step must continue no matter if the job under current execution passes/fails. This breaks the default behaviour of stopping the job if the current step fails.

Consider this YAML:

```yaml
name: "Continue on Error"
on:
   push: # This acation is called when the code is pushed to GitHub
   workflow_dispatch: # this can be manually triggered
jobs:
   retry-job:
      runs-on: "ubuntu-latest"
      name: My job
      steps:
         - name: Checkout repository
           uses: actions/checkout@v4

         - name: Some action that can fail
           # You need to specify an id to be able to tell what the status of this action was
           id: myStepId1
           continue-on-error: true # this needs to be true to proceed to the next step of failure
           uses: actions/someaction

         - name: Some action that can fail
           id: myStepId2
           # Only run this step if step 1 fails. It knows that step one failed based on the `id` specified in the first step
           uses: steps.myStepId1.outcome == 'failure'
           # this needs to be true to proceed to the next step of failure
           continue-on-error: true
           uses: actions/someaction
```

</details>

<details><summary><h3>Is it possible to allow the jobs in a workflow matrix even if one of the combination fails?</h3></summary>

Yes, this can be done by adding the `continue-on-error: true` at the job level which contains the matrix strategy.

A workflow that uses another workflow is referred to as a **"caller"** workflow. The reusable workflow is a **"called"** workflow. One caller workflow can use multiple called workflows. Each called workflow is referenced in a single line. The result is that the caller workflow file may contain just a few lines of YAML, but may perform a large tasks when it's run.

</details>

</details>

<details><summary><h3>Can we reuse the workflows in another workflow?</h3></summary>

Reusing workflows avoids duplication. This makes workflow easier to maintain and allows you to create new workflows more quickly by building on the work of others, just as you do with actions. Workflow reuse also promotes best practice.

Consider the following **called** workflow. This workflow will have an input parameter that it receives from the caller workflow. We will name this workflow `called-workflow.yml`:

```yaml
name: Called Workflow

on:
  workflow_call:
    inputs:
      myInput:
        # this means that the input needs to be present when the workflow is called
        required: true
        # data type of the input parameter 
        type: string
    secrets:
      mySecret:
         required: true

jobs:
  calledJob:
    runs-on: ubuntu-latest
    steps:
      - name: Print input
        run: echo "The input is ${{ inputs.myInput }}"
```

In this workflow, we define an input parameter `myInput` that is required. In the job `calledJob`, we print the value of this input.

Now, we can create the **caller** workflow. This workflow will call the `called-workflow.yml` and pass a value to the `myInput` parameter. We will name this workflow `caller-workflow.yml`:

```yaml
name: Caller Workflow
on: [push]

jobs:
   callerWorkflow:
      runs-on: ubuntu-latest
      steps:
         - name: Call the workflow
           uses: ./.github/workflows/called-workflow.yml@main
           with:
              myInput: "Hello, World!"
           secrets:
              mySecret: ${{ secrets.some-secret }}
```

In this workflow, we use the `uses` keyword to call the `called-workflow.yml` workflow. We pass the value to the `myInput` parameter using the `with` keyword.

</details>

<details><summary><h3>Can we reuse the outputs from the called workflow in the caller workflow?</h3></summary>

We can reuse the outputs from the called workflow in
the caller workflow. This is achieved by defining the outputs in the called workflow and then accessing those outputs in the caller workflow.

Consider the `called-workflow.yml`:

```yaml
name: Called Workflow

on:
   workflow_call:
      inputs:
         myInput:
            required: true
            type: string
      outputs:
         myOutput:
            description: "Output from the called workflow"
            value: ${{ jobs.calledJob.outputs.myOutput }}

jobs:
   calledJob:
   runs-on: ubuntu-latest
   steps:
      - id: step1
        run: echo "The input is ${{ inputs.myInput }}"
        shell: bash
      - id: step2
        run: echo "::set-output name=myOutput::Hello from the called workflow"
        shell: bash
```

In this workflow, we define an output parameter `myOutput` that is set in the `step2` of the `calledJob`. The `::set-output name=myOutput::Hello from the called workflow` line sets the output of the step, which is then used as the value of the workflow output.

Then, in the `caller-workflow.yml`:

```yaml
name: Caller Workflow

on: [push]

jobs:
   callerJob:
      runs-on: ubuntu-latest
      steps:
         - id: call-workflow
           uses: ./.github/workflows/called-workflow.yml@main
           with:
              myInput: "Hello, World!"
          - name: Use output
            run: echo "The output from the called workflow is ${{ steps.call-workflow.outputs.myOutput }}"
```
</details>
