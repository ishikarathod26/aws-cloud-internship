# TASK 5: GitHub Workflows for React App

## 1. Objective

The objective of this task was to implement a Continuous Integration (CI) workflow for a React application using GitHub Actions.

The workflow automatically checks the React application whenever code is pushed to the `main` branch or a Pull Request is created targeting the `main` branch.

## 2. Technologies Used

- React.js
- Vite
- Node.js
- npm
- Git
- GitHub
- GitHub Actions
- ESLint

## 3. Project Setup

Created a React application using Vite.

```bash
npm create vite@latest react-github-actions -- --template react
```

Installed project dependencies:

```bash
npm install
```

Started the development server:

```bash
npm run dev
```

## 4. Git and GitHub Setup

Initialized Git in the project:

```bash
git init
```

Added project files:

```bash
git add .
```

Created the initial commit:

```bash
git commit -m "Initial React project setup"
```

Renamed the default branch to main:

```bash
git branch -M main
```

Connected the GitHub repository:

```bash
git remote add origin https://github.com/ishikarathod26/react-github-actions.git
```

Pushed the project to GitHub:

```bash
git push -u origin main
```

## 5. GitHub Actions Workflow Configuration

Created the following directory structure:

```text
.github/
   workflows/
      ci.yml
```

Configured the workflow in `ci.yml`:

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

## 6. Workflow Explanation

### Triggers

- **Push:** Runs the workflow when code is pushed to the main branch.
- **Pull Request:** Runs the workflow when a Pull Request targets the main branch.
- **Workflow Dispatch:** Allows manual execution of the workflow.

### Job Configuration

- **runs-on:** Uses an Ubuntu GitHub-hosted runner.
- **Checkout:** Downloads the repository code into the runner.
- **Setup Node.js:** Configures Node.js version 24.
- **npm ci:** Installs dependencies using the package-lock.json file.
- **ESLint:** Checks the source code for configured linting issues.
- **Build:** Creates the production build of the React application.

## 7. Testing the CI Workflow

After configuring the workflow, modified the React application UI and updated its CSS styling.

Checked the application locally using:

```bash
npm run dev
```

Verified the changes in the browser.

Then staged and committed the updated files:

```bash
git add .
git commit -m "Improve React portfolio UI"
```

Pushed the changes to GitHub:

```bash
git push
```

The push to the main branch triggers the GitHub Actions CI workflow automatically.

## 8. GitHub Repository

Repository: https://github.com/ishikarathod26/react-github-actions

GitHub Actions: https://github.com/ishikarathod26/react-github-actions/actions

## 9. Expected Outcome

The CI workflow automates the following checks:

1. Repository checkout.
2. Node.js environment setup.
3. Dependency installation.
4. ESLint validation.
5. React production build.

This helps identify code quality issues and build errors before changes are merged or deployed.

## 10. Conclusion

Successfully implemented a GitHub Actions Continuous Integration workflow for a React application.

The workflow is configured to run automatically on pushes and Pull Requests targeting the main branch, along with manual execution support.

This task provided practical experience with Git, GitHub, React, CI pipelines, YAML configuration, and GitHub Actions.
