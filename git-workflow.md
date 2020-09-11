# Git-Workflow

## Resources
- Git Cheatsheet by GitHub: https://services.github.com/on-demand/downloads/github-git-cheat-sheet/
- Git Tutorial: http://gitimmersion.com/
## Process

Switch to the master branch: 
    
    git checkout master

Create a new branch and switch to it: 
    
    git checkout -b <branch-name>

Make some changes and then add the files and commit them: 
    
    git add .

    git commit -m "my commit message"

Push to the remote repository - if the branch does not exist on GitHub it is created: 
    
    git push origin <branch-name>

Go to the GitHub repository and click on "New Pull Request"
The Pull Request gets reviewed by the other team member and merged

Then we have to pull in the changes from the GitHub repo to our local branch:

We change to the master branch:
    
    git checkout master branch

We pull in the changes from the GitHub repo:
    
    git pull origin master

We switch to our own branch that we just pushed to GitHub: 
    
    git checkout <branch-name>

Now we merge the master branch into the branch we are currently in:
    
    git merge master