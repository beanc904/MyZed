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
