🔁 1. git rebase (rewrite history on top of another branch)
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