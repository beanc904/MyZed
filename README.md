# Auto sync script

# unix

Locate the script `zedsync` in `~/.local/bin`, and remember to add the path to `$PATH`.

```bash
#!/bin/bash

source $COLOR_SCHEME_INCLUDE

ZED_HOME="$HOME/.config/zed"

cd $ZED_HOME

if [[ $1 == "push" ]]; then
  git add .
  git commit -m "Auto Sync (arch): $(date +"%Y-%m-%d %T")"
  git push origin main
  success "Zed settings pushed to remote repository"
elif [[ $1 == "pull" ]]; then
  git pull origin main
  success "Zed settings pulled from remote repository"
elif [[ $1 == "stash" ]]; then
  git stash
  success "Zed settings stashed locally"
else
  error "Usage: zedsync [push|pull|stash]"
  exit 1
fi
```

# win

Add the code below to pwsh config file `~/Documents/PowerShell/Microsoft.PowerShell_profile.ps1`.

```powershell
function zedsync
{
    param(
        [Parameter(Mandatory=$true)]
        [ValidateSet("push", "pull", "stash")]
        [string]$Action
    )

    $zedConfig = Join-Path $HOME "AppData\Roaming\Zed"
    Push-Location $zedConfig

    try
    {
        if ($action -eq "push")
        {
            git add .
            git commit -m "Auto Sync (win): $(Get-Date -Format 'yyyy-MM-dd HH:mm:ss')"
            git push origin main
        } elseif ($Action -eq "pull")
        {
            git pull origin main
        } elseif ($Action -eq "stash")
        {
            git stash
        }
    } finally
    {
        Pop-Location
    }
}

function zedconfig
{
    $zedConfig = Join-Path $HOME "AppData\Roaming\Zed"
    Push-Location $zedConfig

    try
    {
        zed .
    } finally
    {
        Pop-Location
    }
}
```
