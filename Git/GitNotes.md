# Install Git
[www.google.com](https://www.google.com)

# Initial git
Enter a directory, then type: `git init`  
Now you have a empty git repo, you can use `ls -ah` to check if there is a `.git` file

# Add and Commit
`git add <filename>`  
`git commit -m "your comment"`

# Local repository operation
Display which file has been modified: `git status`  
Display the changes: `git diff`  
Display commit logs : `git log`
Display branch commit logs: `git log --graph`
Display command history: `git reflog`  
Rollback to the previous version: `git reset --hard HEAD^`  
Rollback to the specified version: `git reset --hard <version>`   
:point_up:*(There is no need to type the entire version number, just the first few characters)*  
Discard changes in working directory: `git checkout -- <file>`  
Delete file:
1. `git rm <filename>`
2. `git add` and `git commit`

# Create remote repository
**First step**, create SSH Key. In the user's home directory, check if the ***.ssh*** directory exists. If so, check if the ***id_rsa*** and ***id_rsa.pub*** files exist in the directory, if they are already exist, skip to the next step. If they are not exist, open Shell (Git Bash on Windows) and create an SSH Key:

`ssh-keygen -t rsa -C "youremail@example.com"`

If everything goes well, you can find the ***.ssh*** directory in the user home directory which contains ***id_rsa*** and ***id_rsa.pub***.

:blue_heart: ATTENTION :blue_heart: :  
***id_rsa*** is a private key, it can't be disclosed. ***id_rsa.pub*** is a public key, you can safely tell others.

**Second step**, login to GitHub, open "Settings" -> "SSH and GPG keys" page, then generate a new SSH key.

GitHub allows you to add multiple keys. If you have several computers and you submit code at company or at home, add the Key of each computer to GitHub and you will be able to push to GitHub from each computer.

# Remote synchronization
## Add remote repository

Now that you've created a Git repository locally, you want to create a Git repository on GitHub and synchronize the two repositories remotely, so that the repository on GitHub can both be used as a backup and allow others to collaborate with it.

**First step**, login to GitHub and create a new repository.

**Second step**, run the following command in your local git repository:

`git remote add origin git@github.com:<GitHub account name>/<GitHub repository name>.git`

For example, my GitHub account name is ***ruochenwang929***, and I create a new GitHub repository named ***Test***. Then I should type the following command in my local repository:

`git remote add origin git@github.com:ruochenwang929/Test.git`

**Third step**, push all content from the local repository to the remote repository:

`git push -u origin master`

Since the remote repository is empty, the first time we push the master branch, we add the `-u` parameter.

Git will not only push the contents of the local master branch to the new remote master branch, but also associate the local master branch with the remote master branch, which can simplify the command in the future push or pull.

GitHub can now see that the contents of the remote repository are exactly the same as those of the local repository. From now on, local push can be made by command:

`git push -u origin master`

If you want to delete remote repository:
1. use `git remote -v` to see the information about remote repository
2. then delete by name, for example I want to delete ***origin***: `git remote rm origin`

:heart: ATTENTION :heart: :
The 'delete' here just unbinds the local and remote binding relationship, not physically deleting the remote repo.

## Clone remote repository
 Assuming you're developing from scratch, the best way to do this is to create a remote repository and clone it to the local repository.

**First step**, login to GitHub and create a new repository.

**Second step**, clone it in the local file directory:

`git clone git@github.com:<github account name>/<repository name>.git`

For example, my GitHub account name is ***ruochenwang929***, and I create a new GitHub repository named ***Test***. Then I should enter the following command in my local repository:

`git clone git@github.com:ruochenwang929/Test.git`

Now you have cloned all the contents of the remote repository. You can modify things locally, but remember to pull before pushing:

`git pull origin master` or `git pull`

:purple_heart: ATTENTION :purple_heart:

Since October 1, 2020, the default branch name for new repositories in GitHub is now ***main*** rather than ***master***. If you set up a remote repository on GitHub first, all of the above local commands need to be changed from `master` to `main`.

For example, `git pull origin main`, `git push origin main` so on and so forth.

## Fork management
Create a new branch called ***test***: `git branch test`  
Switch to ***test*** branch: `git checkout test`  
:bell: Tips: the above two commands can be combined into one:
`git checkout -b test`

Check current branch: `git branch`
Display all local and remote branches: `git branch -a`

Now you can make changes and commit on the ***test*** branch, when your work is done on the ***test*** branch, you can switch back to ***master*** branch: `git check out master`.

Finally, you should merge the work of the ***test*** branch into the ***master*** branch: `git merge test`.

After merging ***test*** branch, you can choose to delete ***test*** branch: `git branch -d test`.

However, the above command can only delete branches of the local repository, if you want to delete branches of the remote repository, use: `git push origin --delete test`

# .gitignore
Configure .gitignore in IDEA:

1. Install **.gitignore** plugin in IDEA
2. In project level, New -> .ignore file -> .gitignore file(git)
3. Configure corresponding filtering files
4. The added.gitignore may not work, solutions:
   1. Go to the folder where the project package is located
   2. `git rm -r --cached .`
   3. `git add .`
   4. `git commit -m "update gitignore file"`

# git stash

      public static void main(String[] args) {
            System.out.println("Existing code on the feature branch");
            // ...
            System.out.println("Code under development");
      }
After using `git stash`

      public static void main(String[] args) {
            System.out.println("Existing code on the feature branch");
            // ...
      }
You can use `git stash list` to display list  
Finally you can use `git stash pop` or `git stash apply` to recover code