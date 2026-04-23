# GitHub / Git Reference

## Setup
git config --global credential.helper osxkeychain  # save credentials on Mac

## Basic workflow
git status                  # see what changed
git add filename            # stage a file
git add .                   # stage everything
git commit -m "message"     # commit with message
git push                    # push to GitHub
git log --oneline           # see commit history

## .gitignore — what to exclude
.idea/          # PyCharm settings
.venv/          # virtual environment
__pycache__/    # Python bytecode
*.pyc           # compiled Python
.DS_Store       # Mac metadata
*.env           # secrets, API keys — NEVER commit

## Always commit
requirements.txt            # pip freeze > requirements.txt
README.md                   # project description
your .py files

## Verify remote repo
git remote -v    # shows which GitHub repo you're connected to