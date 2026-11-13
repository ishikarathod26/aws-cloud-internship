# Task 6: GitHub Reusable Workflows for React App

## Objective
To create and implement a reusable GitHub Actions workflow for a React application to avoid duplicating CI configuration across multiple workflows and repositories.

## Project Structure

```text
react-github-actions/
│
├── .github/
│   └── workflows/
│       ├── ci.yml
│       ├── reusable-ci.yml
│       └── deploy.yml
│
├── src/
├── public/
├── package.json
└── vite.config.js
```

## Implementation Steps

### 1. Create a Reusable Workflow

Created `reusable-ci.yml` inside `.github/workflows/`.

Configured the `workflow_call` trigger to allow other workflows to call it.

The reusable workflow performs the following tasks:

- Checkout repository
- Setup Node.js 24
- Install dependencies using `npm ci`
- Run ESLint using `npm run lint`
- Build React application using `npm run build`

### 2. Configure the Reusable Workflow

File: `.github/workflows/reusable-ci.yml`

```yaml
name: Reusable React CI

on:
  workflow_call:

permissions:
  contents: read

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v6

      - name: Setup Node.js
        uses: actions/setup-node@v7
        with:
          node-version: 24
          cache: npm

      - name: Install dependencies
        run: npm ci

      - name: Run ESLint
        run: npm run lint

      - name: Build React application
        run: npm run build
```

### 3. Update the Caller Workflow

Updated the existing `ci.yml` to call the reusable workflow instead of repeating all CI steps.

File: `.github/workflows/ci.yml`

```yaml
name: React App CI

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
  call-reusable-workflow:
    uses: ./.github/workflows/reusable-ci.yml
```

### 4. GitHub Actions Execution Flow

1. A push or pull request to the main branch triggers the caller workflow.
2. The caller workflow invokes `reusable-ci.yml`.
3. The reusable workflow executes the build job.
4. Node.js is configured and dependencies are installed.
5. ESLint checks the React application.
6. The React application is built successfully.

## Git Commands

```bash
git add .github/workflows/ci.yml .github/workflows/reusable-ci.yml

git commit -m "Add reusable React CI workflow"

git push
```

## Verification

- Verified the workflow execution in the GitHub Actions tab.
- Confirmed that the caller workflow successfully invoked the reusable workflow.
- Verified successful execution of all five steps.
- Confirmed that the React application build completed successfully.

## Key Concepts Learned

- Reusable workflows in GitHub Actions
- `workflow_call` trigger
- Caller and reusable workflows
- Using the `uses` keyword at the job level
- Avoiding duplication in CI pipelines
- Automating React application build and lint checks

## Conclusion

Successfully implemented a reusable GitHub Actions workflow for a React application. The CI process is now separated into a caller workflow and a reusable workflow, making the automation easier to maintain and reuse across projects.
