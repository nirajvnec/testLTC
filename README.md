function ifSuccess { if(-Not ($?)) { exit 1 }}
function gaa { git add --all }
function gca { git commit --amend }
function gcm { git commit }
function gcmm { git commit -m $args }
function grm { git reset --mixed HEAD~1 }
function gffm { git fetch origin master:master }
function gpo { git push $args }
function gfp { git push --force-with-lease $args }
function gffm { git fetch origin master:master }
function gffp { git pull --ff-only }
function grbm { gffm; ifSuccess; gss; ifSuccess; git rebase master; git stash pop }
function grbi { gss; ifSuccess; git rebase -i --keep-empty $args; ifSuccess; git stash pop }
function grba { git rebase --abort }
function grbc { git rebase --continue }
function grh { git reset --hard HEAD~1 }
function grs { git reset --soft HEAD~1 }
function gss { git stash save (Get-Date).ToLocalTime() }
function touch { if((Test-Path -Path ($args[0])) -eq $false) { set-content -Path ($args[0]) -Value ($null) } }
function mklink { cmd /c mklink $args }
function gwip { gcmm "WIP: ${args}" }
function astroair { set-location c:/stash/astroair }
function canonical { set-location c:/stash/canonical }
function gp {}
$env:NODE_OPTIONS="--max_old_space_size=8172"
function rb{	
Write-Host("STARTING BUILD OF ALL.sln.....")
cd C:\stash\canonical
wmic service where "pathname like '%%Canonical%%'" call stopservice
nuget restore
$env:DISABLE_MRASPECT=true
C:\\"Program Files (x86)"\\"Microsoft Visual Studio"\2019\Professional\MSBuild\Current\Bin\msbuild All.sln /m /v:m /clp:PerformanceSummary;Summary /p:RunCodeAnalysis=false /p:RunAnalyzers=false /p:StyleCopEnabled=false /p:Configuration=Debug /p:Platform=x64
Write-Host("Build of ALL.sln complete.")
}
function rrrm{ gffm; rb; }
function rrrb{ grbm; rb; }
function da{ 
Write-Output "downloading ..."
Write-Output "$output not found, downloading ..."
$output = "C:\Users\kuniraj\Downloads"
$url = "https://bamboo.air-watch.com/artifact/COM-CN/shared/build-latest/AirWatch-DB-Build-Artifacts/"
Write-Output "Downloading $url"
$start_time = Get-Date
$webclient = new-object System.Net.WebClient
$webclient.Credentials = Get-Credential
$webclient.DownloadFile($url, $output)
Write-Output "Time taken: $((Get-Date).Subtract($start_time).Seconds) second(s)"
}

Write-Host "=== TEST ==="


