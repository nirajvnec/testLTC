# Git Helper Functions

function ifSuccess { if (-Not $?) { exit 1 } }

function gaa { git add --all }
function gca { git commit --amend }
function gcm { git commit }
function gcmm { git commit -m $args }
function grm { git reset --mixed HEAD~1 }
function gffm { git fetch origin development:development }
function gpo { git push $args }
function gfp { git push --force-with-lease $args }
function gffm { git fetch origin development:development }
function gffp { git pull --ff-only }

function grbm { 
    gffm; ifSuccess;
    gss; ifSuccess;
    git rebase development;
    git stash pop 
}

function grbi { 
    gss; ifSuccess;
    git rebase -i --keep-empty $args; ifSuccess;
    git stash pop 
}

function grba { git rebase --abort }
function grbc { git rebase --continue }
function grh { git reset --hard HEAD~1 }
function grs { git reset --soft HEAD~1 }

function gss { git stash save (Get-Date).ToLocalTime() }
