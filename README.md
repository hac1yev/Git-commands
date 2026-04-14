🔁 git rebase (rewrite history on top of another branch)
🧠 Scenario:

main:    A --- B --- C
feature:          D --- E

✅ Command:
git checkout feature
git rebase main
📊 Result:
main:    A --- B --- C
feature:              D' --- E'
💡 Use when:
You want clean linear history
Before opening PR
Updating feature branch

⚠️ Avoid on shared branches

Usage: 
When you created a "feature" branch, but "main" moved forward and you want your "feature" to be based on latest "main". You have to use commands "git add" and "git commit" in "feature" branch and switch to "main" branch to pull all remote changes. Right back to "feature" branch and use "git rebase main" command. Fix conflicts if they appear and use "git add .", "git commit" and 
"git rebase --continue" commands.


🧠 If you want to combine (squash) the last 5 commits into 1, use interactive rebase:

git rebase -i HEAD~5

🔹 You’ll see something like:
pick a1 commit 1
pick b2 commit 2
pick c3 commit 3
pick d4 commit 4
pick e5 commit 5

🔹 Change it to:
pick a1 commit 1
squash b2 commit 2
squash c3 commit 3
squash d4 commit 4
squash e5 commit 5

Then: Esc + :wq + Enter

After this: git push --force



🔀 Git Merge vs Rebase

🔵 git merge
👉 What it does:

Combines two branches by creating a new merge commit.

🧠 History shape:

It preserves the exact history of both branches.

📌 Example:
git checkout main
git merge feature
🧾 Result:

A---B---C (main)
     \     
      D---E (feature)

After merge:

A---B---C------M (main)
     \        /
      D---E---



🟣 2. git rebase
👉 What it does:

Moves your branch on top of another branch by rewriting commit history.

🧠 History shape:

Creates a linear history (clean timeline).

📌 Example:
git checkout feature
git rebase main
🧾 Result:

Before:
A---B---C (main)
     \
      D---E (feature)

After:
A---B---C---D'---E' (feature)


👤 You are working on feature/login-page
👥 Your teammate also does:
git checkout feature/login-page
git pull

👉 Now BOTH of you have the same branch locally.

💥 What happens if you rebase?

If you do:

git rebase main

Then you rewrite history:

Your commits become new IDs (D', E')
But your teammate still has old commits (D, E)

So now:

Your version:     D'---E'
Teammate version: D---E

👉 Same branch name, but different history

🚨 Why this is a problem

When teammate tries:

git pull

Git gets confused:

“These commits don’t match anymore”

This leads to:

conflicts
duplicated commits
messy history
force sync issues



🔹 git fetch — what it does

git fetch downloads the latest changes from the remote repository (like origin) but does NOT merge them into your current branch.


After this:

Your local repo gets updated info about remote branches
Your current branch stays exactly the same
Changes are stored in something like: origin/main, origin/branch1

| Command     | What it does                            |
| ----------- | --------------------------------------- |
| `git fetch` | Download changes only                   |
| `git pull`  | "git fetch" + "git merge" automatically |



🔵 git log --oneline --branches --not --remotes

If this command shows you your commits that means you have not push these commits to your remote branch.
If nothings appears that means you have pushed commits to your remote branch.


If nothing appers and you want to undo your commits you have to use: 
🔹"git revert"

If this command shows you your commits and you want to undo those commits you can use:
🔹"git reset"


↩️ git reset (move HEAD / undo commits locally)
🧠 Scenario:

You committed something wrong:

A --- B --- C (bad commit)
🔹 Soft reset (keep changes staged)
git reset --soft HEAD~1

✔ commit removed
✔ changes still staged

🔹 Mixed reset (default)
git reset HEAD~1

✔ commit removed
✔ changes stay in working directory

If you want to undo content that you wrote in code you must right after use:
git checkout .

🔹 Hard reset (danger)
git reset --hard HEAD~1

❌ removes commit + changes completely

💡 Use when:
Fix local commits before push
Clean history


🔄 git revert (safe undo for shared branches)
🧠 Scenario:

You already pushed a bad commit:

A --- B --- C (bad commit)
✅ Command:
git revert C
📊 Result:
A --- B --- C --- C'

(C' is inverse of C)

💡 Use when:
Commit is already pushed
Team uses same branch
Production fixes

✔ safest undo method