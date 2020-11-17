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
######

:code:`git config --list`
    * lists configuration

:code:`git config --global core.editor "vim"`
    * Set vim as the default editor

:code:`git config --global diff.tool "vimdiff"`
    * Set vimdiff as the default diff tool

Add files
#########

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
####################

:code:`git status`
    * Get the status of the repository
    * :code:`-s`: short status

Log
###

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
####

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
############

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


Tags
####

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
#######

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
##########

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
#########

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


Remotes
#######

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


Remote Branching
################

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

:code:`git fetch`
    * Downloads objects and references from the repository

:code:`git fetch -p` or :code:`--prune`
    * When a branch is deleted on the server, it might still appear in the pointers of the local repository. Pruning removes these references. 


Rebasing
########

**Basic rebase**

.. code-block:: bash

   git checkout dev
   
   git rebase master

The above will find all the changes to dev after its last common ancestor with master and replay them at the head of master. The new base of dev therefore, will be the head of master (rebase)

**Rebase onto**

.. code-block:: bash

   git rebase --onto br1 br2 br3

This will take all the changes of br3 after its last common ancestor with br2 and replay them on top of br1. In other words it rebases br3 after br2 on top of br1.  


**Squashing commits with rebase**

Suppose the following commits in branch

`c1 -- c2 -- c3 -- c4 -- HEAD`

and that we want to squash commits `c3` and `c4`. This can be done interactively using 

.. code-block:: bash

   git rebase -i HEAD~3

or

.. code-block:: bash

   git rebase -i c2

(where c2 is the commit number)

An interactive window will then open, where git asks if we want to squash, keep or even drop a commit. The commits will appear in a list as 

.. code-block:: bash

    pick c3 <commit_message> 
    pick c4 <commit_message>
    pick HEAD <commit_message>
 
    # Rebase 42e44ea..bbdf516 onto 42e44ea (4 commands)
    #
    # Commands:
    # p, pick = use commit
    # r, reword = use commit, but edit the commit message
    # e, edit = use commit, but stop for amending
    # s, squash = use commit, but meld into previous commit
    # f, fixup = like "squash", but discard this commit's log message
    # x, exec = run command (the rest of the line) using shell
    # d, drop = remove commit
    #
    # These lines can be re-ordered; they are executed from top to bottom.
    #
    # If you remove a line here THAT COMMIT WILL BE LOST.
    #
    # However, if you remove everything, the rebase will be aborted.
    #
    # Note that empty commits are commented out
  

The top commit (c3) cannot be squashed. If it needs to be removed, simply delete the line. 

If you simply want to squash the 3 commits into 1, change the 1st 3 lines to 

.. code-block:: bash

   r c3 <commit_message>
   f c4 <commit_message>
   f HEAD <commit_message>

  
This will squash the HEAD and c4 into c3, and will open a new interactive window for editing the overall commit message. 


This method should be used for branches that haven't been pushed to the server. 



Stashing
########

:code:`git stash`

:code:`git stash list`

:code:`git stash apply`

:code:`git stash drop`

:code:`git stash clear`

Ignore different line endings
#############################

:code:`git config --global core.autocrlf true`


Git Tracing
###########

:code:`GIT_CURL_VERBOSE=1`

:code:`GIT_TRACE=1`

in windows, write 
:code:`set GIT_CURL_VERBOSE=1`

:code:`set GIT_TRACE=1`

Reset them to 0 when done. 

Git credential.helper
#####################

This can be set to :code:`<store>` or :code:`<cache>` for unix systems. In this case, and when we connect to a remote repository over https, which requires a password, the password is either stored in a .gitcredentials file or it is cached in memory, where it is deleted after a certain period of time. The commands to change between the 2 are

:code:`git credential.helper store`

:code:`git credential.helper cache`

These configurations can be changed from the :code:`.gitconfig` file as well.

For windows, the respective options are :code:`<wincred>` and :code:`<manager>`. The first uses the windows credentials and the second uses the credential manager. 

Gitlab setup 
############

Installation
************

Set up instructions can be found in 
https://docs.gitlab.com/omnibus/docker/

To install a gitlab version run the following


.. code-block:: bash

    export GITLAB_HOME=/srv/gitlab
    mkdir -p $GITLAB_HOME/data
    mkdir -p $GITLAB_HOME/logs
    mkdir -p $GITLAB_HOME/config

    docker stop gitlab
    docker rm gitlab

    docker run --detach \
      --publish 80:80 --publish 443:443  --publish 22:22 \
      --name gitlab \
      --restart always \
      --volume $GITLAB_HOME/config:/etc/gitlab \
      --volume $GITLAB_HOME/logs:/var/log/gitlab \
      --volume $GITLAB_HOME/data:/var/opt/gitlab \
      gitlab/gitlab-ce:11.7.12-ce.0

This will install the :code:`gitlab-ce:11.7.12-ce.0` version of gitlab.

To see a list of images, visit
https://hub.docker.com/r/gitlab/gitlab-ce/tags

If there is no web address to which the gitlab will be listening to, just remove the :code:`hostname` from the above. 

The ip address where the container is running can be found using

.. code-block:: bash

   docker network ls
   docker network inspect bridge

and looking for the address of the specific container, typically something like :code:`172.17.x.x/xx`

**Updating**

To update to a newer version, run the above code again, replacing the 
:code:`gitlab-ce:11.7.12-ce.0` part with the newest version, as found from 
https://hub.docker.com/r/gitlab/gitlab-ce/tags

Note that updates between different major versions, have to follow a specific schedule. For example for going from :code:`11.7.0` to :code:`13.3.4` the following schedule has to be followed

:code:`11.7.12` ->
:code:`11.11.8` ->
:code:`12.0.12` ->
:code:`12.10.14`->
:code:`13.0.12` ->
:code:`13.3.4`

For more info visit 
https://docs.gitlab.com/ee/policy/maintenance.html#upgrade-recommendations




Backup
******

https://docs.gitlab.com/ee/raketasks/backup_restore.html

Backing up data
===============

.. code-block:: bash

   # For Gitlab 12.1 and earlier
   docker exec -t <container_name> gitlab-rake gitlab:backup:create

   # For Gitlab later than 12.1
   docker exec -t <container_name> gitlab-backup create

The backup will be found in $GITLAB_HOME/data/backups.

Backing up configuration files
==============================

In addition to backing up data, the

.. code-block:: bash

   $GITLAB_HOME/config

folder has to be backed up, in a **separate** location from the rest of the data. This folder contains the encryption keys to the data, and storing them together defeats the purpose of encryption.

Restore
*******

A backup can only be restored to **exactly the same version and type (CE/EE)**.

Preparation: 

1. The backup file should be in the :code:`$GITLAB_HOME/data/backups` folder.
2. Restore also the :code:`$GITLAB_HOME/config` folder.

Once everything's in place, run the following...


.. code-block:: bash

    # Stop the processes that are connected to the database
    docker exec -it <name of container> gitlab-ctl stop unicorn
    docker exec -it <name of container> gitlab-ctl stop puma
    docker exec -it <name of container> gitlab-ctl stop sidekiq
    
    # Verify that the processes are all down before continuing
    docker exec -it <name of container> gitlab-ctl status
    
    # Run the restore
    docker exec -it <name of container> gitlab-backup restore BACKUP=mybackup

    # Use this for <= 12.1
    # docker exec -it <name of container> gitlab-rake gitlab:backup:restore BACKUP=mybackup
    
    # Restart the GitLab container
    docker restart <name of container>
    
    # Check GitLab, after the container is up and healty...
    docker exec -it <name of container> gitlab-rake gitlab:check SANITIZE=true

* Note that gitlab appends a :code:`_gitlab_backup.tar` suffix to the argument of the :code:`BACKUP`. So, if the backup file name is :code:`mybackup_gitlab_backup.tar`, the restore command should use :code:`BACKUP=mybackup` only.

* Sometimes there can be a permissions issue with the backup file. In these cases do the following:

.. code-block:: bash

    docker exec -it <container_name> /bin/bash
    cd /var/opt/gitlab/backups
    chown git:git <name_of_the_back_up_file>

SSH
***

**Enabling ssh to a different port**

To enable ssh access to gitlab from a different port start the gitlab container using

.. code-block:: bash

    docker run ... \
      ... \
      --publish 6173:22 \
      --env GITLAB_SHELL_SSH_PORT=6173 \
      ... \
The above exposes the port 22 (ssh) to the host's port 6173, and sets the environment variable :code:`GITLAB_SHELL_SSH_PORT` accordingly.

**Permissions**

When the ssh key files are copied during a restore, it is possible that the correct permissions, especially if the files have been to a windows environment. To restore permissions log in the gitlab container :code:`docker exec -it gitlab /bin/bash` and set the permissions of the , :code:`ssh_host_rsa_key`, :code:`ssh_host_ecdsa_key` and :code:`ssh_host_ed25519_key` to 600. i.e.

.. code-block:: bash

    docker exec -it gitlab /bin/bash
    cd /etc/gitlab
    chmod 600 ssh_host_rsa_key ssh_host_ecdsa_key ssh_host_ed25519_key

**Troubleshooting**

To check issues with ssh access the logs are in 
:code:`/srv/gitlab/logs/sshd/current`




Gitlab Runner
*************

Installation
============

Create the config directory for the gitlab-runner

.. code-block:: bash

  mkdir -p /srv/gitlab-runner/config

Get the docker image and start the runner.

.. code-block:: bash

  docker run -d --name gitlab-runner --restart always \
    -v /srv/gitlab-runner/config:/etc/gitlab-runner \
    -v /var/run/docker.sock:/var/run/docker.sock \
    gitlab/gitlab-runner:v11.7.0


Things to watch: 

* The :code:`gitlab-runner` tag (:code:`11.7.0` in the example above) should match the version of the gitlab installation. A list of tags can be found here: 
https://hub.docker.com/r/gitlab/gitlab-runner/tags/

Registration
============

Once the runner is installed, it must be registred. It can be registred within gitlab either as a shared runner from the gitlab admin, either as a project specific runner from the individual project. We will describe how it can be registered as a shared runner.

In gitlab, go to the Admin Area > Runners.

Within there you will see a *Set up a shared Runner manually* section. You will need the following information

* :code:`URL`
* :code:`registration token`

In the command line run 

.. code-block:: bash

  docker run -it gitlab-runner gitlab-runner register

The first :code:`gitlab-runner` is the name of the container and the second is a command to be executed within the container. 

Fill in the following information

.. code-block:: 

   Please enter the gitlab-ci coordinator URL (e.g. https://gitlab.com/):
   <enter the URL from above>

   Please enter the gitlab-ci token for this runner:
   <enter the registration token from above> 

   Please enter the gitlab-ci description for this runner:
   <enter a name for this runner>

   Please enter the gitlab-ci tags for this runner (comma separated):
   <enter the tags for this runner>

   Please enter the executor: ...
   docker

   Plsease enter the default Docker image (e.g. ruby:2.1):
   alpine

Notes
-----

**Default image**
The default docker image will be the one used if there is not one specified in the .yml file. Usually, it should be specified, so the default image might not matter that much

**Tags**
Runners won't start unless the job has the same tag as the runner does.  For example, if the runner has the tag production, then the job in the .gitlab-ci.yml file has to have the tag production as well.

**Clone_url**

In the :code:`config.toml` file, add the url of the gitlab installation in the :code:`[[runners]]` section. 

.. code-block::

   [[runners]]
     ...
     ...
     clone_url = "http://my.gitlab.url.com"
     ...


If the clone is not set, the runner may not know where is the repository, and it can fail during the cloning, returning an error like 


.. code-block:: 

    fatal: unable to access 'http://gitlab-ci-token:xxxxxxxxxxxx@s98usdofjjf/path/to/repo.git': Could not resolve host: s98usdofjjf


For more info see: https://docs.gitlab.com/runner/configuration/advanced-configuration.html#how-clone_url-works


If the installation was made with docker, the address can be the address that pops out as :code:`Gateway` when running 

.. code-block:: bash

   docker network inspect bridge 

which could be something like :code:`172.17.0.1`

After finishing registration, it's worth restarting the gitlab-runner container by running

.. code-block:: bash

   docker restart gitlab-runner


The gitlab-ci.yml file
======================

Now create a :code:`.gitlab-ci.yml` file at the top level of the project. A minimal file that can be used for testing is the following

.. code-block:: 

    default:
        image: alpine
        tags: 
            - tag1
        script: 
            - ls
            - <any other command you want to execute> 
      

The reference for the file can be found in https://docs.gitlab.com/ee/ci/yaml/README.html   

When commiting the code, gitlab will notice the existence of the :code:`.gitlab-ci.yml` file and will run this automatically. 


Linking runners to specific projects
====================================

Going to :code:`Admin > Overview > Runners` and selecting a specific runner, allows linking a runner to a specific project, by selecting :code:`Enable` in the :code:`Restrict projects for this Runner`.

This makes the specific runner not a shared runner any more, and can be only used to serve specific projects. **Note** This process is **non-reversible**. Once a runner stops being a shared runner by means of the above process, it cannot be a shared runner again. 
