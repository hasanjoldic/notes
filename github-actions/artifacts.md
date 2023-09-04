# Artifacts

Artifacts allow you to persist data after a job has completed, and share that data with another job in the same workflow. An artifact is a file or collection of files produced during a workflow run. For example, you can use artifacts to save your build and test output after a workflow run has ended. All **actions and workflows called within a run have write access to that run's artifacts.**

These are some of the common artifacts that you can upload:

- Log files and core dumps
- Test results, failures, and screenshots
- Binary or compressed files
- Stress test performance output and code coverage results

> By default, GitHub stores build logs and artifacts for 90 days, and this retention period can be customized.

## Comparing artifacts and dependency caching

Artifacts and caching are similar because they provide the ability to store files on GitHub, but each feature offers different use cases and cannot be used interchangeably.

- Use caching when you want to reuse files that don't change often between jobs or workflow runs, such as build dependencies from a package management system.
- Use artifacts when you want to save files produced by a job to view after a workflow run has ended, such as built binaries or build logs.

## Uploading build and test artifacts

The output of building and testing your code often produces files you can use to debug test failures and production code that you can deploy. You can configure a workflow to build and test the code pushed to your repository and report a success or failure status. You can upload the build and test output to use for deployments, debugging failed tests or crashes, and viewing test suite coverage.

```yml
name: Node CI

on: [push]

jobs:
  build_and_test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v3
      - name: npm install, build, and test
        run: |
          npm install
          npm run build --if-present
          npm test
      - name: Archive production artifacts
        uses: actions/upload-artifact@v3
        with:
          name: dist-without-markdown
          path: |
            dist
            !dist/**/*.md
      - name: Archive code coverage results
        uses: actions/upload-artifact@v3
        with:
          name: code-coverage-report
          path: output/test/code-coverage.html
```

## Downloading or deleting artifacts

```yml
- name: Download a single artifact
  uses: actions/download-artifact@v3
  with:
    name: my-artifact
```

You can also download all artifacts in a workflow run by not specifying a name. This can be useful if you are working with lots of artifacts.

```yml
- name: Download all workflow run artifacts
  uses: actions/download-artifact@v3
```

Jobs that are dependent on a previous job's artifacts must wait for the dependent job to complete successfully. We use the `needs` keyword to ensure that jobs run sequentially.

```yml
name: Share data between jobs

on: [push]

jobs:
  job_1:
    name: Add 3 and 7
    runs-on: ubuntu-latest
    steps:
      - shell: bash
        run: |
          expr 3 + 7 > math-homework.txt
      - name: Upload math result for job 1
        uses: actions/upload-artifact@v3
        with:
          name: homework
          path: math-homework.txt

  job_2:
    name: Multiply by 9
    needs: job_1
    runs-on: windows-latest
    steps:
      - name: Download math result for job 1
        uses: actions/download-artifact@v3
        with:
          name: homework
      - shell: bash
        run: |
          value=`cat math-homework.txt`
          expr $value \* 9 > math-homework.txt
      - name: Upload math result for job 2
        uses: actions/upload-artifact@v3
        with:
          name: homework
          path: math-homework.txt

  job_3:
    name: Display results
    needs: job_2
    runs-on: macOS-latest
    steps:
      - name: Download math result for job 2
        uses: actions/download-artifact@v3
        with:
          name: homework
      - name: Print the final result
        shell: bash
        run: |
          value=`cat math-homework.txt`
          echo The result is $value
```
