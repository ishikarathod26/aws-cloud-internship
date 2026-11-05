# TASK 5:GitHub Actions – React App CI/CD Pipeline

## 1. Objective

The objective of this task was to create a React application using Vite and implement Continuous Integration (CI) and Continuous Deployment (CD) using GitHub Actions.

The pipeline automatically:
- Installs project dependencies.
- Checks code quality using ESLint.
- Builds the React application.
- Deploys the production build to GitHub Pages.

## 2. Technologies Used

- React.js
- JavaScript
- Vite
- Node.js 24
- npm
- Git and GitHub
- GitHub Actions
- GitHub Pages
- YAML

## 3. React Project Setup

Created the React application using Vite:

```bash
npm create vite@latest react-github-actions -- --template react
```

Installed dependencies:

```bash
npm install
```

Started the development server:

```bash
npm run dev
```

## 4. Git and GitHub Setup

Initialized the Git repository and pushed the project to GitHub.

```bash
git init
git add .
git commit -m "Initial React project setup"
git branch -M main
git remote add origin <repository-url>
git push -u origin main
```

## 5. Continuous Integration (CI)

Created the workflow file:

`.github/workflows/ci.yml`

The CI workflow is triggered on:
- Push to the main branch.
- Pull requests targeting the main branch.
- Manual execution using workflow_dispatch.

### CI Workflow Responsibilities

1. Checkout repository code.
2. Set up Node.js.
3. Install dependencies using `npm ci`.
4. Run ESLint using `npm run lint`.
5. Build the React application using `npm run build`.

### Important Commands

- `npm ci`: Installs dependencies from the lock file.
- `npm run lint`: Checks code for configured linting issues.
- `npm run build`: Generates the production build inside the `dist` directory.

## 6. Continuous Deployment (CD)

Implemented automatic deployment using GitHub Pages.

Created a separate workflow:

`.github/workflows/deploy.yml`

The deployment workflow is triggered whenever code is pushed to the main branch. It also supports manual execution.

### Deployment Process

1. Checkout repository code.
2. Set up Node.js.
3. Install dependencies using `npm ci`.
4. Generate the production build using `npm run build`.
5. Upload the `dist` directory as a GitHub Pages artifact.
6. Deploy the artifact using GitHub Pages.

### Important GitHub Actions

- `actions/checkout`: Checks out the repository.
- `actions/setup-node`: Configures Node.js.
- `actions/upload-pages-artifact`: Uploads the production build for deployment.
- `actions/deploy-pages`: Publishes the uploaded artifact to GitHub Pages.

## 7. Vite Configuration for GitHub Pages

Updated `vite.config.js` with the repository base path:

```javascript
import react from '@vitejs/plugin-react'
import { defineConfig } from 'vite'

export default defineConfig({
  plugins: [react()],
  base: '/react-github-actions/',
})
```

The `base` property ensures that JavaScript, CSS and other assets are loaded correctly when the application is hosted under a repository-specific URL.

## 8. GitHub Pages Configuration

Configured GitHub Pages in repository settings:

**Settings → Pages → Build and deployment → Source → GitHub Actions**

This allows GitHub Actions to publish the application.

## 9. Testing and Verification

Pushed the updated configuration and deployment workflow to the main branch.

Verified:
- CI workflow execution.
- Production build completion.
- CD workflow execution.
- GitHub Pages deployment.
- Live React application accessibility.

## 10. Project Links

**GitHub Repository:**

https://github.com/ishikarathod26/react-github-actions

**GitHub Actions:**

https://github.com/ishikarathod26/react-github-actions/actions

**Live Application:**

https://ishikarathod26.github.io/react-github-actions/

## 11. Expected Outcome

The project demonstrates an automated CI/CD pipeline using GitHub Actions.

Whenever new code is pushed to the main branch:
- CI performs linting and build validation.
- CD independently builds and deploys the application to GitHub Pages.

This reduces manual deployment effort and makes the application accessible online.

## 12. Conclusion

Successfully implemented Continuous Integration and Continuous Deployment for a React application using GitHub Actions and GitHub Pages.

This task provided practical experience with Git, GitHub, YAML workflow configuration, automated build validation and deployment.
