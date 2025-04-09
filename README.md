Leaning github action

<!-- to use an action -->
<!-- use the uses keyword -->
<!-- to run a command use the run keyword -->
<!-- runs-on: this is to specify the github action server to use when running the job -->
<!-- steps: this is to list out the flow of the jobs, like the steps it need to run to complete the job -->
<!-- needs: this is to wait for another job to run before running the selected job, just like depends in docker  -->
<!--  -->


Key	Description
name	Names the workflow for identification in the GitHub UI
on	Specifies the events that trigger the workflow (e.g., push, pull_request, schedule)
jobs	Defines one or more jobs to be run as part of the workflow
runs-on	Specifies the environment (e.g., ubuntu-latest, windows-latest) where the job runs
steps	A sequence of steps (actions or commands) to run inside a job
uses	Indicates an external GitHub Action or reusable workflow to run (e.g., actions/checkout@v3)
run	Runs a shell command in the runner
env	Defines environment variables available to steps within a job
with	Passes input parameters to actions
if	Conditional statement to control whether a job or step runs
strategy	Used for defining build matrix (e.g., multiple Node versions)
matrix	Sub-key of strategy, defines the combinations for matrix builds
needs	Specifies job dependencies (one job waits for another to finish)
timeout-minutes	Sets a time limit for a job to run
defaults	Set default values like shell for all steps
continue-on-error	Allow a job or step to fail without failing the whole workflow
secrets	Secure variables stored in GitHub, used like secrets.MY_SECRET_KEY
permissions	Control access to the GitHub API and other scopes for the job

If you want, I can help you build one of the workflows step-by-step from the tasks above. Want to try one together?
You said:
list more and explain them
ChatGPT said:

Absolutely! Let’s go deeper and expand the list of keys you might encounter in GitHub Actions .yml files — especially as you grow into more intermediate/advanced use cases.
🔁 More GitHub Actions YAML Keys (With Explanations)
Key	Description
jobs.<job_id>	A named job block. You can have multiple jobs under jobs, each with its own steps, runs-on, etc.
jobs.<job_id>.name	Overrides the default job ID in the Actions UI with a readable name.
jobs.<job_id>.runs-on	Specifies the runner (environment) for this specific job.
jobs.<job_id>.container	Runs the job inside a Docker container instead of a VM runner. Useful for consistent environments.
jobs.<job_id>.services	Define services like databases (e.g., PostgreSQL, Redis) for integration tests.
jobs.<job_id>.defaults.run.shell	Specify a custom shell (like bash, sh, pwsh) for all run steps in the job.
jobs.<job_id>.outputs	Used to pass data from one job to another when using needs.
jobs.<job_id>.continue-on-error	Lets the job continue even if it fails. Doesn’t fail the whole workflow.
🔁 Step-Level Keys
Key	Description
id	Gives a step an identifier so its outputs can be referenced later (e.g., steps.step_id.outputs.some_value).
uses	Runs an existing GitHub Action or composite action. Example: actions/setup-node@v3.
run	Runs a shell command. Example: run: npm install.
name	Human-readable name of the step (shows in GitHub UI).
shell	Overrides the default shell (bash on Linux/macOS, pwsh on Windows).
working-directory	Changes the directory context for the step.
env	Set environment variables only for that step.
timeout-minutes	Sets a time limit for just that step.
continue-on-error	Allows the workflow to proceed even if this step fails.
with	Provides input parameters to an action specified by uses.
🔁 Workflow Triggers (inside on)

You define how and when a workflow is triggered under the on: block:

on:
  push:
    branches: [main]
    paths: ["src/**", "!src/ignore-this-file.js"]
  pull_request:
    types: [opened, synchronize, closed]
  schedule:
    - cron: '0 0 * * *'  # Every day at midnight UTC
  workflow_dispatch:     # Allows manual trigger via GitHub UI
  workflow_call:         # Used for reusable workflows
  release:
    types: [published]

🔁 Matrix Builds

Matrix builds let you test across multiple combinations:

strategy:
  matrix:
    node-version: [16, 18, 20]
    os: [ubuntu-latest, windows-latest]

This will create 3 x 2 = 6 parallel jobs!
🔁 Reusable Workflows

You can define workflows that are reusable using:

on:
  workflow_call:
    inputs:
      environment:
        required: true
        type: string
    secrets:
      MY_SECRET:
        required: true

And call it from another workflow using:

uses: user/repo/.github/workflows/workflow.yml@main
with:
  environment: "production"
secrets:
  MY_SECRET: ${{ secrets.MY_SECRET }}

🔁 Expressions & Contexts

You can use ${{ ... }} expressions to access variables and conditionals:
Example	Description
${{ github.actor }}	The username of the person who triggered the workflow
${{ github.ref }}	Branch or tag ref
${{ github.event_name }}	The name of the event that triggered the workflow
${{ job.status }}	Status of the job (success, failure, cancelled)
${{ secrets.MY_SECRET }}	Access a secret defined in repo settings
${{ steps.step_id.outputs.some_output }}	Get the output of a previous step

This list should cover 90% of what you'll encounter with GitHub Actions. If you want, I can share a full sample workflow that includes most of these features so you can study and test it.

Do you want one real-world example that includes secrets, matrix, and job dependencies?
