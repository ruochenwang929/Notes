## Git Notes
### 1. Install Git
Too lazy to teach you
### 2. Initial git
Enter a directory, then input: `git init`
### 3. Add and Commit
`git add <filename>`  
`git commit -m "your comment"`
### 4. Repository status
Display which file has been modified, use `git status` 

Display the changes, use `git diff`

Displays commit logs from most recent to farthest, use`git log`

Rollback to the previous version, use `git reset --hard HEAD^`

Rollback to the specified version, use `git reset --hard 
<version>`<u>There is no need to write the entire version number, just the first few digits.</u>

Review the command history to determine which version to go back to in the future, use `git reflog`