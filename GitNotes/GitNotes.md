## Git Notes
### 1. Install Git
Too lazy to teach you
### 2. Initial git
Enter a directory, then input: `git init`
### 3. Add and Commit
`git add <filename>`  
`git commit -m "your comment"`
### 4. Local repository operation
Display which file has been modified, use `git status` 

Display the changes, use `git diff`

Displays commit logs from most recent to farthest, use`git log`

Rollback to the previous version, use `git reset --hard HEAD^`

Rollback to the specified version, use `git reset --hard 
<version>` <u>There is no need to write the entire version number, just the first few digits.</u>

Review the command history to determine which version to go back to in the future, use `git reflog`

Delete file, use `rm <filename>`, then, use `git rm <filename>`, finally, remember use `git add` and `git commit`

### 5.Remote repository
First, create SSH Key. In the user's home directory, check whether the **.ssh** directory exists. If so, check whether the **id_rsa** and **id_rsa.pub** files exist in the directory, if they are already exist, skip to the next step. If they are not exist, open Shell (Git Bash on Windows) and create an SSH Key: `ssh-keygen -t rsa -C "youremail@example.com"`

If everything goes well, you can find the **.ssh** directory in the user home directory which contains **id_rsa** and **id_rsa.pub**