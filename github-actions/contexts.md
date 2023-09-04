# Contexts

Contexts are a way to access information about workflow runs, variables, runner environments, jobs, and steps. Each context is an object that contains properties, which can be strings or other objects.

| Context name | Description                                                                           |
| ------------ | ------------------------------------------------------------------------------------- |
| github       | Information about the workflow run.                                                   |
| env          | Contains variables set in a workflow, job, or step.                                   |
| vars         | Contains variables set at the repository, organization, or environment levels.        |
| job          | Information about the currently running job.                                          |
| jobs         | For reusable workflows only, contains outputs of jobs from the reusable workflow.     |
| steps        | Information about the steps that have been run in the current job.                    |
| runner       | Information about the runner that is running the current job.                         |
| secrets      | Contains the names and values of secrets that are available to a workflow run.        |
| strategy     | Information about the matrix execution strategy for the current job.                  |
| matrix       | Contains the matrix properties defined in the workflow that apply to the current job. |
| needs        | Contains the outputs of all jobs that are defined as a dependency of the current job. |
| inputs       | Contains the inputs of a reusable or manually triggered workflow.                     |
