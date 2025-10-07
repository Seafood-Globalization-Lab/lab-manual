# UW Seafood Globalization Lab’s Manual 📖 🦀 

This is the repository where the **UW Seafood Globalization Lab’s Manual** is edited, built, and deployed from.  
The website is automatically published from the `gh-pages` branch and served via **GitHub Pages**.

## How to Edit the Manual

### Workflow Overview (Simplified GitFlow)

We follow a lightweight **GitFlow-style workflow** to keep the history clean and contributions organized:

- 🌳 **`main` branch** – long-lived, production branch  
   - Represents the current public version of the Lab Manual.  
   - Every change merged into `main` triggers a site rebuild and redeploy.

- ✨ **Feature branches** – short-lived, task-specific branches  
   - Created from remote `main` branch  
   - Named after the GitHub Issue or task and incorporate contributors’ initials (e.g., `feature-am-issue-3-contributing-file` or `feature-jp-code-of-conduct`).  
   - Used to make and test changes locally before merging into `main`.  
   - Rebased frequently on `main` to maintain a linear and up-to-date history.

- 🆕 **Integrate Changes**  
   - The feature branch is rebased and merged into `main`.  
   - The feature branch is then closed/deleted.

### Example Workflow (git in your terminal)

  1) Clone the repository
  ```bash
  git clone git@github.com:Seafood-Globalization-Lab/lab-manual.git
  cd lab-manual
  ```

  2) Create a short-lived feature branch from main
  ```bash
  git checkout main
  git pull origin main
  git checkout -b feature-update-methods-section
  ```

  3) Make edits locally and preview entire website
  ```bash
  quarto preview
  ```

> [!Note]
> If you have created a new page for the website, make sure it is added/referenced in the `lab-manual/_quarto.yml` file. This is the configuration file that provides the skeleton for the website. Add your new page under `contents:`. Indenting is very strict in `.yml` files, follow the existing formatting.

  4) Stage and commit your changes
  ```bash
  git add .
  git commit -m "Add new section on seafood sourcing methods"
  ```

  5) Rebase to keep your branch up to date with main
  ```bash
  git fetch origin
  git rebase origin/main
  ```

> [!Important] 
> There are two methods for integrating your changes into `main`:
> - (6a) A Pull Request (PR) is *recommended* for collaboration and review. If you have not run your changes by Jessica then a PR is recommended. Request a reviewer for your changes and notify that person of the open PR.
> - (6b - 8) A PR is *not required* for deployment. Any direct push to `main` will automatically trigger the GitHub Action and redeploy the site.

  6a) Open a Pull Request on GitHub
   - Click the "Pull Request" tab on the lab-maunal repo page.
   - Set the "base" to 'main' and "compare" to 'your-feature-branch`
   - Click "Create Pull Request"
   - Add new title and complete the PR template inside of the description box.
   - Assign a reviewer in the right hand column.
   - Add labels that apply to your changes.
   - Click "Create Pull Request"
   - Notify the reviewer you suggested (probably Jessica or current data scientist) of the existence of the PR and you are asking for their review of your changes.

**OR**

  6b) Merge your feature branch into main with a clear message
  ```bash
  git checkout main
  git pull origin main
  git merge --no-ff feature-update-methods-section -m "Merge branch 'feature-update-methods-section' into main"
  ```

> [!Tip]
> - The `--no-ff` flag ensures the merge is recorded as a distinct commit, preserving the context of your feature branch.
> - The `-m` flag paried with `"your message"` adds a message to the merge commit helping keep track of feature integration.

  7) Push updated main to trigger the GitHub Action
  ```bash
  git push origin main
  ```

  8) Delete the feature branch locally & on the remote
  ```bash
  git branch -d feature-update-methods-section
  git push origin --delete feature-update-methods-section
  ```

> [!Important]
> Once `main` is updated on GitHub, a GitHub Action is triggered to publish the website. See below (How to Deploy the Website) for more details.

## How to Deploy the Website

The website is published from the **`gh-pages` branch** using a **GitHub Actions workflow** that automates the Quarto build and deployment process.

### Automatic Deployment (via GitHub Actions)

When changes are pushed or merged into `main`, the GitHub Action defined in `.github/workflows/publish.yml` automatically builds and deploys the site **on a GitHub-hosted runner** (a temporary virtual 
machine created for each workflow run). This process mirrors what would happen if you ran `quarto publish gh-pages` locally, but everything occurs remotely within GitHub’s infrastructure.

Here’s what happens step by step:

   1. **Start a GitHub-hosted runner** – GitHub spins up a clean virtual machine (Ubuntu environment) to execute the workflow.  
   2. **Check out** the repository on the `main` branch using the `actions/checkout@v4` step.  
   3. **Install Quarto** with the `quarto-dev/quarto-actions/setup@v2` action.  
   4. **Render the site** within the runner by running:
      ```bash
      quarto render
      ```
      This builds the HTML output in the runner’s environment.
   5. **Publish the rendered output** to the `gh-pages` branch using:
      ```bash
      quarto publish gh-pages
      ```
      This command executes **on the runner**, authenticating via GitHub’s provided token, and pushes the built site to your repository.
   6. **Deploy to GitHub Pages**, which serves the contents of the `gh-pages` branch at:  
      👉 **https://seafood-globalization-lab.github.io/lab-manual/**

To verify that the site was successfully deployed:
1. Go to the **Actions** tab in the repository.  
2. Open the latest **“Publish to GitHub Pages”** workflow run.  
3. Confirm that all steps completed successfully (✅ green checkmark).

If successful, updates will appear on the public site within a few minutes.

### Manual Deployment (optional but not recommended)

If you need to deploy the site manually (e.g., for testing or local builds):

> [!Warning] 
> The branch you are on when running `quarto publish gh-pages` determines what content is published.  
> To avoid overwriting the live site with in-progress edits, always ensure you are on the `main` branch before deploying manually:
> ```bash
> git checkout main
> git pull origin main
> quarto publish gh-pages
> ```

```bash
quarto publish gh-pages
```

This command performs the same steps as the GitHub Action, but **runs locally** on your machine.  
It will:

- Render the manual using the `_quarto.yml` configuration **from whichever branch you are currently on**.  
- Push the built HTML files to the remote `gh-pages` branch.  
- Immediately update the live GitHub Pages site at  
  👉 **https://seafood-globalization-lab.github.io/lab-manual/**


Manual deployment can be useful for quick testing or emergency rebuilds, but the preferred and safest method is to let the **GitHub Action** handle publishing automatically when changes are merged or pushed to `main`.


### Reference

- 📘 [Quarto Docs: Publishing to GitHub Pages](https://quarto.org/docs/publishing/github-pages.html)  
