# GIT_COMMANDS.md — One-Shot Repo Publishing

```bash
cd ~/monitoring-stack
git init
git add .
git commit -m "Initial commit: Monitoring Stack project"

# Create a new empty public repo at https://github.com/new
git remote add origin https://github.com/ZdravecPanov/monitoring-stack.git
git branch -M main
git push -u origin main

# Add screenshots later
mkdir -p screenshots
# (save images)
git add screenshots/
git commit -m "Add Grafana screenshots"
git push
```
