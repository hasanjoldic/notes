# Variables

Variables provide a way to store and reuse **non-sensitive** configuration information.

## Defining environment variables for a single workflow

You can define variables that are scoped for:

- The entire workflow, by using `env` at the top level of the workflow file
- The contents of a job within a workflow, by using `jobs.<job_id>.env`
- A specific step within a job, by using `jobs.<job_id>.steps[*].env`

```yml
name: Greeting on variable day

on: workflow_dispatch

env:
  DAY_OF_WEEK: Monday

jobs:
  greeting_job:
    runs-on: ubuntu-latest
    env:
      Greeting: Hello
    steps:
      - name: "Say Hello Mona it's Monday"
        run: echo "$Greeting $First_Name. Today is $DAY_OF_WEEK!"
        env:
          First_Name: Mona
```

Because runner environment variable interpolation is done after a workflow job is sent to a runner machine, you must use the appropriate syntax for the shell that's used on the runner. In this example, the workflow specifies ubuntu-latest. By default, Linux runners use the bash shell, so you must use the syntax `$NAME`. If the workflow specified a Windows runner, you would use the syntax for PowerShell, `$env:NAME`.

## Naming conventions for environment variables

When you set an environment variable, you cannot use any of the default environment variable names.

Any new variables you set that point to a location on the filesystem should have a `_PATH` suffix. The `GITHUB_ENV` and `GITHUB_WORKSPACE` default variables are exceptions to this convention.

> Note: You can list the entire set of environment variables that are available to a workflow step by using `run: env` in a step and then examining the output for the step.

## Defining configuration variables for multiple workflows

You can create configuration variables for use across multiple workflows, and can define them at either the **organization**, **repository**, or **environment** level.

> If a variable with the same name exists at multiple levels, the variable at the lowest level takes precedence.

## Using the env context to access environment variable values

Runner environment variables are always interpolated on the runner machine. However, parts of a workflow are processed by GitHub Actions and are not sent to the runner. You cannot use environment variables in these parts of a workflow file. Instead, you can use contexts. For example, an `if` conditional, which determines whether a job or step is sent to the runner, is always processed by GitHub Actions. You can use a context in an if conditional statement to access the value of an variable.

```yml
env:
  DAY_OF_WEEK: Monday

jobs:
  greeting_job:
    runs-on: ubuntu-latest
    env:
      Greeting: Hello
    steps:
      - name: "Say Hello Mona it's Monday"
        if: ${{ env.DAY_OF_WEEK == 'Monday' }}
        run: echo "$Greeting $First_Name. Today is $DAY_OF_WEEK!"
        env:
          First_Name: Mona
```

You will commonly use either the `env` or `github` context to access variable values in parts of the workflow that are processed before jobs are sent to runners.

|  Context | Use case                                                                           | Example                    |
| -------: | :--------------------------------------------------------------------------------- | :------------------------- |
|    `env` | Reference custom variables defined in the workflow.                                | `${{ env.MY_VARIABLE }}`   |
| `github` | Reference information about the workflow run and the event that triggered the run. | `${{ github.repository }}` |

## Using the vars context to access configuration variable values

```yml
on:
  workflow_dispatch:
env:
  # Setting an environment variable with the value of a configuration variable
  env_var: ${{ vars.ENV_CONTEXT_VAR }}

jobs:
  display-variables:
    name: ${{ vars.JOB_NAME }}
    # You can use configuration variables with the `vars` context for dynamic jobs
    if: ${{ vars.USE_VARIABLES == 'true' }}
    runs-on: ${{ vars.RUNNER }}
    environment: ${{ vars.ENVIRONMENT_STAGE }}
    steps:
      - name: Use variables
        run: |
          echo "repository variable : $REPOSITORY_VAR"
          echo "organization variable : $ORGANIZATION_VAR"
          echo "overridden variable : $OVERRIDE_VAR"
          echo "variable from shell environment : $env_var"
        env:
          REPOSITORY_VAR: ${{ vars.REPOSITORY_VAR }}
          ORGANIZATION_VAR: ${{ vars.ORGANIZATION_VAR }}
          OVERRIDE_VAR: ${{ vars.OVERRIDE_VAR }}

      - name: ${{ vars.HELLO_WORLD_STEP }}
        if: ${{ vars.HELLO_WORLD_ENABLED == 'true' }}
        uses: actions/hello-world-javascript-action@main
        with:
          who-to-greet: ${{ vars.GREET_NAME }}
```

## Default environment variables

| Variable                   | Description                                                                                                                               |
| -------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| CI                         | Always set to true.                                                                                                                       |
| GITHUB_ACTION              | The name of the action currently running, or the id of a step.                                                                            |
|                            | GitHub removes special characters, and uses the name `__run` when the current step runs a script without an id.                           |
|                            | If you use the same script or action more than once in the same job, the name will include a suffix that consists of the sequence number. |
|                            | For example, the first script you run will have the name `__run`, and the second script will be named `__run_2`.                          |
|                            | Similarly, the second invocation of `actions/checkout` will be `actionscheckout2`.                                                        |
| GITHUB_ACTION_PATH         | The path where an action is located. This property is only supported in composite actions.                                                |
|                            | You can use this path to access files located in the same repository as the action.                                                       |
| GITHUB_ACTION_REPOSITORY   | For a step executing an action, this is the owner and repository name of the action.                                                      |
| GITHUB_ACTIONS             | Always set to true when GitHub Actions is running the workflow.                                                                           |
|                            | You can use this variable to differentiate when tests are being run locally or by GitHub Actions.                                         |
| GITHUB_ACTOR               | The name of the person or app that initiated the workflow.                                                                                |
| GITHUB_ACTOR_ID            | The account ID of the person or app that triggered the initial workflow run.                                                              |
| GITHUB_API_URL             | Returns the API URL.                                                                                                                      |
| GITHUB_BASE_REF            | The name of the base ref or target branch of the pull request in a workflow run.                                                          |
|                            | This is only set when the event that triggers a workflow run is either pull_request or pull_request_target.                               |
| GITHUB_ENV                 | The path on the runner to the file that sets variables from workflow commands.                                                            |
| GITHUB_EVENT_NAME          | The name of the event that triggered the workflow.                                                                                        |
| GITHUB_EVENT_PATH          | The path to the file on the runner that contains the full event webhook payload.                                                          |
| GITHUB_GRAPHQL_URL         | Returns the GraphQL API URL.                                                                                                              |
| GITHUB_HEAD_REF            | The head ref or source branch of the pull request in a workflow run.                                                                      |
| GITHUB_JOB                 | The job_id of the current job.                                                                                                            |
| GITHUB_OUTPUT              | The path on the runner to the file that sets the current step's outputs from workflow commands.                                           |
| GITHUB_PATH                | The path on the runner to the file that sets system PATH variables from workflow commands.                                                |
| GITHUB_REF                 | The fully-formed ref of the branch or tag that triggered the workflow run.                                                                |
| GITHUB_REF_NAME            | The short ref name of the branch or tag that triggered the workflow run.                                                                  |
| GITHUB_REF_PROTECTED       | true if branch protections are configured for the ref that triggered the workflow run.                                                    |
| GITHUB_REF_TYPE            | The type of ref that triggered the workflow run.                                                                                          |
| GITHUB_REPOSITORY          | The owner and repository name.                                                                                                            |
| GITHUB_REPOSITORY_ID       | The ID of the repository.                                                                                                                 |
| GITHUB_REPOSITORY_OWNER    | The repository owner's name.                                                                                                              |
| GITHUB_REPOSITORY_OWNER_ID | The repository owner's account ID.                                                                                                        |
| GITHUB_RETENTION_DAYS      | The number of days that workflow run logs and artifacts are kept.                                                                         |
| GITHUB_RUN_ATTEMPT         | A unique number for each attempt of a particular workflow run in a repository.                                                            |
| GITHUB_RUN_ID              | A unique number for each workflow run within a repository.                                                                                |
| GITHUB_RUN_NUMBER          | A unique number for each run of a particular workflow in a repository.                                                                    |
| GITHUB_SERVER_URL          | The URL of the GitHub server.                                                                                                             |
| GITHUB_SHA                 | The commit SHA that triggered the workflow.                                                                                               |
| GITHUB_STEP_SUMMARY        | The path on the runner to the file that contains job summaries from workflow commands.                                                    |
| GITHUB_WORKFLOW            | The name of the workflow.                                                                                                                 |
| GITHUB_WORKFLOW_REF        | The ref path to the workflow.                                                                                                             |
| GITHUB_WORKFLOW_SHA        | The commit SHA for the workflow file.                                                                                                     |
| GITHUB_WORKSPACE           | The default working directory on the runner for steps, and the default location of your repository when using the checkout action.        |
| RUNNER_ARCH                | The architecture of the runner executing the job.                                                                                         |
| RUNNER_DEBUG               | This is set only if debug logging is enabled, and always has the value of 1.                                                              |
| RUNNER_NAME                | The name of the runner executing the job.                                                                                                 |
| RUNNER_OS                  | The operating system of the runner executing the job.                                                                                     |
| RUNNER_TEMP                | The path to a temporary directory on the runner.                                                                                          |
| RUNNER_TOOL_CACHE          | The path to the directory containing preinstalled tools for GitHub-hosted runners.                                                        |

## Detecting the operating system

```yml
jobs:
  if-Windows-else:
    runs-on: macos-latest
    steps:
      - name: condition 1
        if: runner.os == 'Windows'
        run: echo "The operating system on the runner is $env:RUNNER_OS."
      - name: condition 2
        if: runner.os != 'Windows'
        run: echo "The operating system on the runner is not Windows, it's $RUNNER_OS."
```

## Passing values between steps and jobs in a workflow

```yml
steps:
  - name: Set the value
    id: step_one
    run: |
      echo "action_state=yellow" >> "$GITHUB_ENV"
  - name: Use the value
    id: step_two
    run: |
      printf '%s\n' "$action_state" # This will output 'yellow'
```

```yml
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
        run: echo "test=world" >> "$GITHUB_OUTPUT"
  job2:
    runs-on: ubuntu-latest
    needs: job1
    steps:
      - env:
          OUTPUT1: ${{needs.job1.outputs.output1}}
          OUTPUT2: ${{needs.job1.outputs.output2}}
        run: echo "$OUTPUT1 $OUTPUT2"
```
