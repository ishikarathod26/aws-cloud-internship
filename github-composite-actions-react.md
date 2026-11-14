GitHub Composite Actions for React Application

## 1. Objective

The objective of this task was to create a reusable GitHub Composite Action in a GitHub Organization repository and integrate it into a React application repository.

This approach helps multiple projects reuse common CI steps without duplicating workflow commands.

## 2. Repository Setup

**GitHub Organization:** `ishika-learning-lab`

**Shared Repository:** `shared-github-actions`

**React Application Repository:** `react-github-actions`

The shared repository contains the Composite Action, while the React repository consumes it.

### Shared Repository Structure

```text
shared-github-actions/
│
├── react-build/
│   └── action.yml
│
└── README.md
```

## 3. Creating the Composite Action

Created `react-build/action.yml` inside the shared repository.

### action.yml

```yaml
name: React Build Action
description: Setup Node.js, install dependencies, run ESLint and build React application

runs:
  using: composite

  steps:
    - name: Setup Node.js
      uses: actions/setup-node@v4
      with:
        node-version: 24

    - name: Install dependencies
      shell: bash
      run: npm ci

    - name: Run ESLint
      shell: bash
      run: npm run lint

    - name: Build React App
      shell: bash
      run: npm run build
```

### Explanation

- `using: composite` defines the action as a Composite Action.
- `actions/setup-node@v4` configures Node.js.
- `npm ci` installs dependencies using the lock file.
- `npm run lint` checks the React application's code.
- `npm run build` generates the production build.
- `shell: bash` specifies the shell for each `run` step.

## 4. Versioning and Release

Created and verified the first GitHub release in the shared repository.

- **Tag:** `v1`
- **Release Name:** React Build Action v1
- **Release Description:** Initial release of a reusable GitHub Composite Action for React applications.

The release provides a versioned reference that other repositories can use.

## 5. Integrating the Composite Action into React

Created the workflow file:

`.github/workflows/composite-ci.yml`

### composite-ci.yml

```yaml
name: React Composite Action CI

on:
  push:
    branches:
      - main

  pull_request:
    branches:
      - main

  workflow_dispatch:

permissions:
  contents: read

jobs:
  react-build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout React Repository
        uses: actions/checkout@v4

      - name: Run Shared React Composite Action
        uses: ishika-learning-lab/shared-github-actions/react-build@v1
```

### Explanation

- `push`: Runs the workflow when code is pushed to the main branch.
- `pull_request`: Runs the workflow for pull requests targeting main.
- `workflow_dispatch`: Allows manual execution.
- `actions/checkout@v4`: Checks out the React repository.
- `uses: ishika-learning-lab/shared-github-actions/react-build@v1`: Invokes the Composite Action from the organization repository using release tag `v1`.

## 6. Execution Flow

```text
React Application Repository
          |
          v
composite-ci.yml
          |
          v
Checkout React Repository
          |
          v
Call Shared Composite Action
          |
          v
shared-github-actions/react-build/action.yml
          |
          v
Setup Node.js 24
          |
          v
Install Dependencies (npm ci)
          |
          v
Run ESLint (npm run lint)
          |
          v
Build React App (npm run build)
          |
          v
Workflow Completed Successfully
```

## 7. Testing and Verification

Executed the `React Composite Action CI` workflow through GitHub Actions.

**Workflow Details:**

- Workflow: React Composite Action CI
- Run: #1
- Commit: `a291475`
- Branch: `main`
- Result: Successful
- Execution time: Approximately 15 seconds (job succeeded in approximately 10 seconds).

The workflow completed successfully, confirming that the React repository could invoke and execute the shared Composite Action.

### Warnings and Notices

During execution, GitHub displayed:

1. **Node.js 20 Deprecation Warning:** `actions/checkout@v4` and `actions/setup-node@v4` were reported as targeting Node.js 20, with execution being forced to Node.js 24. This is a GitHub Actions runtime compatibility warning, not a React application's Node.js version error.
2. **Ubuntu Runner Migration Notice:** GitHub announced that the `ubuntu-latest` runner label would migrate to Ubuntu 26 beginning October 19, 2026.

Both messages were informational/compatibility notices; the workflow still succeeded.

## 8. Key Concepts Learned

- GitHub Organizations and shared repositories.
- Composite Actions and their structure.
- Purpose of `action.yml`.
- Difference between `uses` and `run`.
- Node.js setup in GitHub Actions.
- Dependency installation using `npm ci`.
- ESLint checks and production builds.
- Git tags and GitHub Releases.
- Reusing organization-level Composite Actions in multiple repositories.
- Workflow execution and debugging through GitHub Actions.

## 9. Final Outcome

Successfully created and published a reusable GitHub Composite Action in the `ishika-learning-lab` organization and integrated it into the React application repository.

The React CI workflow successfully executed the shared action, demonstrating how common build operations can be centralized and reused across multiple projects.
