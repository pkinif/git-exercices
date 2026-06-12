# Hands-on Git Exercises — STATFOR Session 1

> **General tip:** For all exercises, use the **RStudio Git pane buttons** rather than terminal commands. The visual interface builds the mental model first — terminal commands come with expertise.

---

## Before you start

Confirm these work:

- You can open RStudio
- `git --version` returns something in the terminal
- You can log into Azure DevOps in your browser
- Your Git user name + email are set:
  ```bash
  git config --global user.name "Your Name"
  git config --global user.email "your-email@eurocontrol.int"
  ```

If any of these fail, raise your hand before we start.

---

## Exercise 1 — Solo: your first end-to-end workflow (20 min)

**Goal:** Create a repo, clone it, modify it, push your first commit.

### Steps

**1. Create a repo on Azure DevOps**

- Go to your team's Azure DevOps project → **Repos** → **+ New repository**
- Name it: `statfor-git-training-<yourname>` (e.g. `statfor-git-training-zvon`)
- ✅ Tick "Add a README"
- Add a .gitignore; you can choose `R`
- Click **Create**
- Copy the HTTPS clone URL (top-right "Clone" button)

**2. Clone in RStudio**

- **File** → **New Project** → **Version Control** → **Git**
- Paste the URL, choose a local folder, click **Create Project**
- Git will prompt for a password, you find on Azure Devops (click on `generate git credentials` and copy/paste the password)
- RStudio opens the project. You should see three files: `README.md`, the `.gitignore` and your `*.Rproj` files in the Files pane.

**3. Add an R script**

- **File** → **New File** → **R Script**
- Save it as `hello_git.R`
- Paste this code:

```r
# hello_git.R — my first Git-tracked script

print("Hello Git from RStudio!")
print(paste("R version:", R.version.string))

x <- c(1, 2, 3, 4, 5)
print(paste("Mean of x:", mean(x)))
```

**4. Run it** to confirm it works (`Ctrl+Shift+Enter` or click **Source**).

**5. Add your .Rproj to the .gitignore**

- Open the .gitignore
- Add this *.Rproj at the end (as a new line)
- Save (CTRL + S)
- Refresh the git tab and your .Rproj file should be gone from the list

**6. Stage + Commit + Push via the RStudio Git pane**

- Open the **Git** tab (top-right pane)
- ✅ Tick the box next to `hello_git.R` and `.gitignore` — this stages it
- Click **Commit** and a popup should open
- Write a message: `my first commit` in the Commit Message Tab, a black tab with some writing will be displayed, something like:
- Click **Commit** in the dialog
  
```r
>>> /usr/bin/git commit -F /tmp/RtmpkekDx1/git-commit-message-12e96626a84.txt
[main b555a2f] my first commit
 2 files changed, 9 insertions(+)
 create mode 100644 hello_git.R
```

- Click **Push** (green up arrow)

**6. Verify on Azure DevOps**

- Refresh your repo page in the browser
- `hello_git.R` should be visible
- Click **Commits** in the left sidebar — your commit message should appear with your name and timestamp

### ✅ Checkpoint
Your script is visible on Azure DevOps with a clear commit message attributed to you.

---

## Exercise 2 — Pair: collaborating on each other's repos (20 min)

**Goal:** Experience the workflow from both sides — as contributor AND as repo owner.

### Setup

- Pair up (Pierrick will assign)
- **Share your repo URL** with your partner in the chat
- You now have two repos to think about: **your own** + **your partner's**

### Steps

> Throughout this exercise: **A** = you, **B** = your partner.

**1. Clone B's repo**

- **File** → **New Project** → **Version Control** → **Git**
- Paste B's URL
- ⚠️ Use a **different local folder** from your own repo — name it after the partner (e.g. `git-training-laurent`)
- You now have two RStudio projects you can switch between (top-right corner of RStudio)

**2. Modify B's `hello_git.R`**

- Make sure you're in B's project (check the project name top-right)
- Open `hello_git.R`
- Add these lines at the **end** of the file:

```r
# Added by <your-name>
y <- c(10, 20, 30)
print(paste("Sum of y:", sum(y)))
```

**3. Stage + commit + push** (to B's remote)

- Same RStudio workflow as Exercise 1
- Commit message: `add y vector`
- Push
- See the changes on Azure Devops in the B repo

**4. Switch back to your OWN project**

- Top-right corner of RStudio → switch to your repo
- At this point, you must wait for your partner to have `git push` on your own repository
- You're now in your own repo, which is **untouched locally** but has new commits on the remote (from B working on your repo).

**5. Pull B's changes**

- Click **Pull** (blue down arrow) in the Git pane
- Open `hello_git.R` — you should see the lines B added

**6. Run the updated script** to confirm it still works

### ✅ Checkpoint
Both `hello_git.R` files (in both repos) now contain code from both A and B.

> **Heads-up:** If A and B happen to edit the same line, Git will raise a **merge conflict** on the second push. We're not triggering this on purpose today — but if it happens, and you don't know how to handle it... that'll be your proof we need a Session 2 on Git 🙂

---

## Exercise 3 — Branches: working on `feature/...` (15 min)

**Goal:** Make changes on a separate branch without touching `main`.

> Back in **your own repo** (the one from Exercise 1).

### Steps

**1. Create a new branch via RStudio**

- In the Git pane, click the **New Branch** button (purple-ish icon, top of the pane)
- Name the branch: `feature/<yourname>-experiment` (e.g. `feature/zvon-experiment`)
- Keep "Remote: origin" and "Sync branch with remote" checked
- Click **Create**
- A terminal will pop up and you will see something like this:

```r
>>> /usr/bin/git checkout -B feature/pierrick-experiment
Switched to a new branch 'feature/pierrick-experiment'
>>> /usr/bin/git push -u origin feature/pierrick-experiment
To https://dev.azure.com/ECTL-AZURE/Aviation%20Intelligence%20Unit/_git/statfor-git-training-pierrick
 * [new branch]      feature/pierrick-experiment -> feature/pierrick-experiment
branch 'feature/pierrick-experiment' set up to track 'origin/feature/pierrick-experiment'.
```

- Close it and the branch dropdown in the Git pane should now show your new branch


**2. Add a change to `hello_git.R`**

- Append this at the end of the file:

```r
# Experimenting on a feature branch
square <- function(n) n * n
print(square(7))
```

**3. Stage + commit + push**

- Same Git pane workflow
- Commit message: `add square function on feature branch`
- Push
  
**4. Verify on Azure DevOps**

- Repo page → **Branches** (left sidebar)
- You should see two branches: `main` and `feature/<yourname>-experiment`
- Click on each one and compare `hello_git.R`:
  - `main` does NOT have the `square` function
  - your feature branch DOES

### ✅ Checkpoint
Two branches exist on Azure DevOps with different content. You can switch between them in the Branches view.

---

## Wrap-up (5 min — back in the main room)

- Questions on anything that didn't work
- Pierrick demos what a **Pull Request** looks like on Azure DevOps (we'll do one in Session 2)
- Brief look at Session 2 topics

