# Git Cheat Sheet

1) How to remove files from git commit?

   A: `git reset HEAD^ -- path/to/file && git commit --amend --no-edit`

2) How to undo most recent commits in git?

   A: `git reset --soft HEAD~1`
   
3) How to revert a specific commit after push to git repo?

   A: `git revert <commit-sha>` or `git revert <oldest_commit_hash>..<latest_commit_hash>`
   
4) How to clone a specific branch from git?

   A: `git clone --single-branch -b <branch_name> <repository url>` or `git clone --single-branch -b --depth=1 <branch_name> <repository url>`
   
5) How to create a branch and delete a branch?

   A: Create a branch:
   
          - git checkout -b <barnch_name>
          
      Delete a branch(locally)
      
          - git branch -d [branch_name]
          
      Delete a branch(remotely)
      
          - git push origin :[branch_name]
