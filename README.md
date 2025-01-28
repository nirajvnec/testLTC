git config --global alias.ra "rebase --abort"

git config --global alias.rc "rebase --continue"



git config --global alias.uf '!git fetch origin && git rebase origin/development'

git config --global alias.ufs '!git stash push -m "Auto-stash before rebase" && git fetch origin && git rebase origin/development && git stash apply'

