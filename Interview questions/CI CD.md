### Continuous Integration (CI):
Automatically builds and tests code(using written unit tests) every time you push changes to the repository — helps catch bugs early.

**what does build mean here:**
1. Cloning the code from your repository (GitHub, GitLab, etc.)
2. Installing dependencies (e.g., npm install)
3. Running the build command (e.g., npm run build)
4. Running unit tests (e.g., npm test)
5. Checking linting (e.g., npm run lint)

**What does npm run build does?**
1. Bundles all your JS files using Webpack
2. Minifies and uglifies the code (to reduce file size)
3. Optimizes assets (like images, CSS)
4. Outputs everything to a /build folder

### Continuous Deployment (CD):
Goes a step further — automatically deploys to production after passing tests, without manual approval.

1. In meta- we use diff - where CI/CD is integrated
2. In General - Jenkins, newly came GitHub actions
