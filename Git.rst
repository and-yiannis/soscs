###
Git
###


:code:`git config --list`
   * lists configuration

:code:`git config --global core.editor "vim"`
    * Set vim as the default editor

:code:`git config --global diff.tool "vimdiff"`
    * Set vimdiff as the default diff tool


Config
******

:code:`git config --list`
    * lists configuration

:code:`git config --global core.editor "vim"`
    * Set vim as the default editor

:code:`git config --global diff.tool "vimdiff"`
    * Set vimdiff as the default diff tool

Add files
*********

:code:`git init`
    * start tracking the current directory

:code:`git clone https://... [path/to directory]`
    * Clone the online repository in the current directory.

:code:`git add {files}`
    * Put files in the staging area.
    * :code:`-a`: commit the working area (i.e. skip the staged area)

:code:`git commit -m 'commit name'`
    * Add the staged files to the repository
    * :code:`--amend`: amend the last commit


Status, log, changes
********************

:code:`git status`
    * Get the status of the repository
    * :code:`-s`: short status

Log
***

:code:`git log`
    * Shows the history of commits
    * :code:`-p`: Shows the diffs between commits
    * :code:`-2`: Shows the two last commits only
    * :code:`--stat`: Shows summary of changes
    * :code:`--pretty=[oneline | short | full | fuller | format]`: Pretty output. Format has a multitude of arguments, see page 62 in the progit book.
    * :code:`--graph`: shows an ASCII graph of the branches
    * :code:`--since="2016-12-22 20:40"`: since the date given.
    * :code:`--until="2016-12-22 20:40"`: until the date given
    * :code:`--decorate`:  shows the branch pointers
    * :code:`--all`: shows all branches

Diff
****

:code:`git diff`
    * shows differences between the staged and unstaged files
    * :code:`--staged  --cached`: Shows differences between the staged files and the previous commit.

:code:`git diff HEAD~1 HEAD`
    * Shows the difference between the last but one commit and the last one.

:code:`git diff HEAD~k HEAD~n`
    * Shows the difference between the last but `k` commit and the last but `n` one. 

The :code:`HEAD~` above can be replaced with the actual commit's hash. 

Replacing :code:`diff` with :code:`difftool` will carry out the diff in the selected diff tool. 

:code:`git config --global diff.tool "vimdiff"`
    * Set vimdiff as the default diff tool

:code:`git difftool --tool-help`
    *  allows using an external diff tool.



Change files
************

:code:`git rm file`
    * Deletes file and also removes it from the staging area.
    * :code:`-f`: If the staged and the working directory files are different, they won't be removed. In this case use :code:`git rm -f file`.
    * :code:`--cached`: Remove a file from the staged area but do not delete from the working tree.

:code:`git mv file1 file2`
    * Renames files. equivalent to doing :code:`mv file1 file2; git rm file1; git add file2`

:code:`git reset HEAD file`
    * removes file from the staging area.

:code:`git checkout -- foo.txt`
    * replaces foo.txt in the working tree with foo.txt from the staged area if  this exists or with foo.txt file from the last commit file is not staged.

:code:`git checkout [checksum] -- foo.txt`
    * Check out the file from the commit with the provided checksum. 

:code:`git checkout master`
    * Return to the master branch. 


Remotes
*******

:code:`git remote -v`
    * Show which remotes you are using with aliases

:code:`git remote show`
    * shows information about the remote repositories in use 

:code:`git remote show <repository_short_name>`
    * shows information about the remote repository

:code:`git ls-remote`
    * list all remote references, branches, tags etc

:code:`git remote add <shortname> <url>`
    * Add the remote in url with the shortname <shortname>    

:code:`git fetch <shortname>`


Tags
****

:code:`git tag`
    * shows the tags
    * :code:`-l "expression"`: shows tags that match the expression

:code:`git log <tag_name>`
    * Show the specific tag

:code:`git log --oneline --decorate`
    * shows commits, branches and tags.

:code:`git tag -a <tagname> [commit's checksum] -m 'Tag comment'`
    * Creates an annotated tag. If the checksum is omitted then the latest commit is tagged.

:code:`git push origin <tag_name>`
    * Push tag to the remote

:code:`git tag -d <tag_name>`
    * delete tag locally

To delete a tag remotely

    * get the tag name at the repository
        * :code:`git ls-remote --tags`
    * delete the tag from the remote, e.g. git push origin v0.2
        * :code:`git push <remote_name> <tag_name>`


Tags are not pushed by default. It will have to be done explicitly.


Aliases
*******

:code:`git config --global alias.co checkout`
    * Create aliases for certain commands. The above allows to write git co instead of git checkout 
   
    
:code:`.gitignore`
    * file stating which files to ignore
        * blank lines or lines starting with # are ignored
        * standard glob patterns work
        * start patterns with / to avoid recursivity 
        * end patterns with / to specify directories
        * negate patterns starting them with '!'

In commands such as :code:`git rm`, the file name can contain glob patterns. 
The :code:`'*'` however has to be escaped, because git does its own filename expansion. 
Therefore use :code:`git rm log/\*.log` instead of :code:`git rm log/*.log` 


test begin
**********

:code:`git branch`
    * Lists all the branches

    -a 
       - lists all branches (local and remote)
    -r
        lists remote branches only 
    -v
        verbose
    -vv 
        shows also which online branches are being tracked
    --merged
        Shows the merged branches
    --no-merged
        Shows the unmerged branches

Branching
*********

:code:`git branch`
    * Lists all branches
    * :code:`-a`: lists all branches (local and remote)
    * :code:`-r`: lists remote branches only 
    * :code:`-v`: verbose
    * :code:`-vv`:  shows also which online branches are being tracked
    * :code:`--merged`: Shows the merged branches
    * :code:`--no-merged`: Shows the unmerged branches

:code:`git branch <branchname>`
    * Creates a new branch, but doesn't switch to that.

:code:`git branch -d <branchname>`
    * Deletes the branch
    
:code:`git checkout <branchname>`
    * Switches to that branch

:code:`git checkout -b <branchname>`
    * Is equivalent to git branch <branchname> and git checkout <branchname>

:code:`git merge <branchname>`
    * Merges the branch <branchname> into the current one


Remote Branching
****************

:code:`git checkout -b <local_name_branch> <online_branch_name>`
    * checks out an online branch and creates a local copy with the given name, e.g. git checkout -b serverfix origin/serverfix

:code:`git checkout --track origin serverfix`
    * shortcut for git checkout -b serverfix origin/serverfix

:code:`git checkout serverfix`
    * shorcut for git checkout -b serverfix origin/serverfix assuming that serverfix does not exist in the local repository and that the name exactly matches the branch on the server. 

:code:`git push origin serverfix`
    * pushes serverfix branch to the origin repository
    * :code:`--set-upstream`: Adds upstream (tracking) reference

:code:`git push origin --delete serverfix`
    * deletes serverfix online

:code:`git fetch -p origin`
    * When user1 deletes a branch online, this might still appear in the pointers of user2. In this case, user2 can prune the local "cache" of remote branches using the above. 


Rebasing
********

Basic rebase

:code:`git checkout branch1`

:code:`git rebase branch2`
    * The above will find all the changes to branch1 after its last common ancestor with branch2 and replay them at the head of branch2. The base of branch1 therefore, will be the head of branch2 (rebase)

:code:`git rebase --onto br1 br2 br3`
    * This will take all the changes of br3 after its last common ancestor with br2 and replay them on top of br1. In other words it rebases br3 after br2 on top of br1.  

:code:`git rebase -i HEAD~n`
    * A method for squashing the last n commits into 1. Best not to apply it on the main branch. After squashing, probably have to delete the old branch in the server, with :code:`git push origin --delete <branch_name>` and push the new branch upstream with :code:`git push --set-upstream origin <branch_name>`

Stashing
********

:code:`git stash`

:code:`git stash list`

:code:`git stash apply`

:code:`git stash drop`

:code:`git stash clear`

Ignore different line endings
*****************************

:code:`git config --global core.autocrlf true`


Git Tracing
***********

:code:`GIT_CURL_VERBOSE=1`

:code:`GIT_TRACE=1`

in windows, write 
:code:`set GIT_CURL_VERBOSE=1`

:code:`set GIT_TRACE=1`

Reset them to 0 when done. 

Git credential.helper
*********************

This can be set to :code:`<store>` or :code:`<cache>` for unix systems. In this case, and when we connect to a remote repository over https, which requires a password, the password is either stored in a .gitcredentials file or it is cached in memory, where it is deleted after a certain period of time. The commands to change between the 2 are

:code:`git credential.helper store`

:code:`git credential.helper cache`

These configurations can be changed from the :code:`.gitconfig` file as well.

For windows, the respective options are :code:`<wincred>` and :code:`<manager>`. The first uses the windows credentials and the second uses the credential manager. 

