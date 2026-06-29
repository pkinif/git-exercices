# Hands-on Git Exercises — STATFOR Session 2

> **General tip:** As in Session 1, use the **RStudio Git pane buttons** for all local operations. For Pull Requests and reviews, you'll work directly in **Azure DevOps**.

---

## Before you start

Confirm these still work from Session 1:

- You can open RStudio
- `git --version` returns something in the terminal
- You can log into Azure DevOps in your browser
- Your git identity is set:

```
git config --list | grep user
```

If you don't see your `user.name` and `user.email`, set them:

```
git config --global user.name "Your Name"
git config --global user.email "your-email@eurocontrol.int"
```

If anything fails, raise your hand before we start.

---

## Session 2 roadmap

Today we'll do four exercises, building on each other:

1. **Solo warm-up** — get the basic workflow back in your hands
2. **Solo branching** — work on a feature branch
3. **Pair collaboration** — Pull Requests with peer review
4. **Pair collaboration v2** — something interesting will happen 🙂

Pierrick will form **3 pairs**. Each pair will share a repo for Exercises 3 and 4.

---

## Exercise 1 — Solo: warm-up workflow (15 min)

**Goal:** Get the full clone → edit → commit → push cycle back in your hands.

> If you already have your `statfor-git-training-<yourname>` repo from Session 1 and it still works, you can skip the repo creation and jump straight to step 3.

### Steps

**1. Create (or reopen) your Azure DevOps repo**

- New repo: name it `statfor-git-training-<yourname>` (e.g. `statfor-git-training-zvon`)
- ✅ Tick "Add a README"
- Add a `.gitignore`: choose **R**
- Click **Create**
- Copy the HTTPS clone URL

**2. Clone in RStudio**

- **File** → **New Project** → **Version Control** → **Git**
- Paste the URL, choose a local folder, click **Create Project**
- If prompted: generate Git credentials on Azure DevOps and paste them

**3. Add or update an R script**

- Create `hello_git.R` (or reopen it if it exists)
- Make sure it contains something like:

```r
# hello_git.R
print("Hello Git from RStudio!")
x <- c(1, 2, 3, 4, 5)
print(paste("Mean of x:", mean(x)))
```

**4. Add your `.Rproj` to `.gitignore`**

- Open `.gitignore`
- Add `*.Rproj` on a new line
- Save (CTRL + S)

**5. Stage + Commit + Push via the Git pane**

- Stage `hello_git.R` and `.gitignore`
- Commit with message: `session 2 warm-up`
- Push

**6. Verify on Azure DevOps**

- Refresh the repo page → your commit should appear with your name and timestamp

### ✅ Checkpoint

Your repo is up to date on Azure DevOps with a fresh commit from today.

---

## Exercise 2 — Solo: working on a feature branch (10 min)

**Goal:** Confirm you can work on a separate branch without touching `main`.

> Same repo as Exercise 1.

### Steps

**1. Create a new branch**

- In the Git pane, click the **New Branch** button (purple-ish icon)
- Name it: `feature/<yourname>-warmup` (e.g. `feature/zvon-warmup`)
- Keep "Remote: origin" and "Sync branch with remote" checked
- Click **Create**

**2. Add a change to `hello_git.R`**

- Append at the end of the file:

```r
# Working on a feature branch
square <- function(n) n * n
print(square(7))
```

**3. Stage + commit + push**

- Commit message: `add square function`
- Push

**4. Verify on Azure DevOps**

- Repo page → **Branches** in the left sidebar
- You should see at least two branches: `main` and `feature/<yourname>-warmup`
- Click on each — your feature branch has the `square` function, `main` does not

### ✅ Checkpoint

Two branches exist on Azure DevOps with different content.

---

## Exercise 3 — Pair: collaborating with Pull Requests (25 min)

**Goal:** Experience the full PR-and-review workflow that real teams use.

### Setup

- Pierrick will pair you up
- One of you (Person **A**) creates the shared repo. The other is Person **B**.

### Steps

**1. Person A — create the shared repo**

- Azure DevOps → **+ New repository**
- Name: `statfor-pair-<A>-<B>` (e.g. `statfor-pair-pierrick-laurent`)
- ✅ Tick "Add a README"
- Add a `.gitignore`: choose **R**
- Click **Create**
- Copy the HTTPS clone URL and share it with B in the chat

**2. Both A and B — clone the shared repo in RStudio**

- **File** → **New Project** → **Version Control** → **Git**
- Paste the shared URL
- Choose a local folder named after the pair (e.g. `pair-pierrick-laurent`)

**3. Both — create your own feature branch**

- In the Git pane, **New Branch**
- A names it: `feature/<A>-analysis`
- B names it: `feature/<B>-analysis`

**4. Both — add your own R script**

- A creates `analysis_<A>.R` with this content:

```r
# analysis by <A>
library(dplyr)

mtcars %>%
  group_by(cyl) %>%
  summarise(avg_mpg = mean(mpg))
```

- B creates `analysis_<B>.R` with this content:

```r
# analysis by <B>
library(dplyr)

mtcars %>%
  group_by(gear) %>%
  summarise(avg_hp = mean(hp))
```

**5. Both — stage + commit + push**

- Commit message: `add my analysis script`
- Push

**6. Create a Pull Request on Azure DevOps**

- Go to the repo → **Pull requests** (left sidebar) → **New pull request**
- Source: your feature branch · Target: `main`
- Title: `Add analysis_<yourname>.R`
- Add a short description
- Add your partner as a **reviewer**
- Click **Create**

**7. Review your partner's PR**

- Open their PR
- Click the **Files** tab to see what they changed (the diff)
- Leave at least one comment (anywhere — could just be "looks good")
- Click **Approve**

**8. Merge once approved**

- Click **Complete** on your own PR (after your partner has approved it)
- Keep the default merge settings, click **Complete merge**

**9. Both — pull `main`**

- Switch back to `main` in the Git pane (branch dropdown)
- Click **Pull**
- You should now see both `analysis_<A>.R` and `analysis_<B>.R` in your project

### ✅ Checkpoint

Both analysis scripts are now on `main`. Each PR has been reviewed and approved by the other person before merging.

> **If you finish early:** browse the **Commits** and **Branches** views on Azure DevOps. Compare the commit history with your local view in RStudio's Git **History** button.

---

## Exercise 4 — Pair: the real-world workflow (35 min)

**Goal:** Now that you've each merged your own work, you'll both work on **the same file**. Let's see what happens.

> Same pair, same repo as Exercise 3.

### Part A — Create a shared file (10 min)

**1. Person A — create the shared file on a feature branch**

- Make sure you're on `main` and you've pulled the latest changes
- Create a new branch: `feature/shared-init`
- Create a new file called `shared_analysis.R` and **copy-paste** this exact content:

```r
# shared_analysis.R — pair analysis
library(dplyr)

# Load the mtcars dataset
data("mtcars")

# Summarize the dataset
summary_table <- mtcars %>%
  group_by(cyl) %>%
  summarise(value = mean(mpg))

print(summary_table)
```

- Stage, commit (`add shared_analysis.R`), push
- Create a PR to `main`, add B as reviewer

**2. Person B — review and merge**

- Review A's PR, approve it
- Merge to `main`

**3. Both — pull `main`**

- Switch to `main` in RStudio, click **Pull**
- You should both have `shared_analysis.R` now

### Part B — Both modify the same file (25 min)

> Important: **Do NOT communicate about the code changes** for this part. Just follow the instructions for your role.

**4. Both — create a new feature branch from `main`**

- Make sure you're on `main` and have pulled
- A creates: `feature/<A>-shared`
- B creates: `feature/<B>-shared`

**5. Modify `shared_analysis.R` — different content for A and B**

Person A — replace the `summary_table <- ...` block with:

```r
summary_table <- mtcars %>%
  group_by(cyl) %>%
  summarise(avg_mpg = mean(mpg), avg_hp = mean(hp))
```

Person B — replace the `summary_table <- ...` block with:

```r
summary_table <- mtcars %>%
  group_by(cyl) %>%
  summarise(median_hp = median(hp))
```

**6. Both — stage + commit + push your branch**

- Commit message for A: `summary with mpg and hp`
- Commit message for B: `summary with median hp`
- Push

**7. Person A — create PR and merge first**

- Create PR from your branch to `main`
- B reviews and approves
- A merges to `main`

**8. Person B — try to create your PR**

- Create PR from `feature/<B>-shared` to `main`
- 👀 **Look at what Azure DevOps tells you about this PR.**

### What happened?

Don't try to fix it yet. Raise your hand when you see it.

This is what we'll spend Session 3 learning to resolve. 🙂

---

## Wrap-up (10 min)

- What did we cover today
- Quick demo: what does the conflict look like inside the file
- Session 3 preview: resolving merge conflicts, plus a few advanced tricks
- Q&A
