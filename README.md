# UW Seafood Globalization Lab’s Manual 📖 🦀 

This is the repository where the **UW Seafood Globalization Lab’s Manual** is edited, built, and deployed from.  
The website is automatically published from the `gh-pages` branch and served via **GitHub Pages**.

---

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

  6) Merge your feature branch into main with a clear message
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

  8) (Optional) Delete the feature branch
  ```bash
  git branch -d feature-update-methods-section
  git push origin --delete feature-update-methods-section
  ```

Once `main` is updated on GitHub, the site will automatically rebuild and redeploy via GitHub Actions.

## How to Deploy the Website

The website is published from the **`gh-pages` branch** using a **GitHub Actions workflow** that automates the Quarto build and deployment process.

### Automatic Deployment (via GitHub Actions)

When changes are pushed or merged into `main`, the GitHub Action defined in  
`.github/workflows/publish.yml` automatically runs the following steps:

1. **Check out** the repository on the `main` branch.  
2. **Install Quarto** using the `quarto-dev/quarto-actions/setup@v2` action.  
3. **Render the site** using `quarto render`.  
4. **Publish the rendered output** to the `gh-pages` branch using:  
   ```bash
   quarto publish gh-pages
   ```
5. **Deploy to GitHub Pages**, which hosts the live site at:  
   👉 **https://seafood-globalization-lab.github.io/lab-manual/**

> 💡 **Note:** A Pull Request is *recommended* for collaboration and review,  
> but **not required** for deployment.  
> Any direct push to `main` will automatically trigger the GitHub Action and redeploy the site.

To verify that the site was successfully deployed:
1. Go to the **Actions** tab in the repository.  
2. Open the latest **“Publish to GitHub Pages”** workflow run.  
3. Confirm that all steps completed successfully (✅ green checkmark).

If successful, updates will appear on the public site within a few minutes.

---

### Manual Deployment (optional)

If you need to deploy the site manually (e.g., for testing or local builds):

```bash
quarto publish gh-pages
```

This command will:
- Render the manual locally using `_quarto.yml`  
- Push the built HTML files to the `gh-pages` branch  
- Immediately update the live GitHub Pages site

---

### Reference

- 📘 [Quarto Docs: Publishing to GitHub Pages](https://quarto.org/docs/publishing/github-pages.html)  
- 🧭 [UW Seafood Globalization Lab Manual Repository](https://github.com/Seafood-Globalization-Lab/lab-manual)

---
