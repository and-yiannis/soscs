####
Bash
####

Disk Usage
**********
:code:`du` provides information about the disk usage

* :code:`-h` shows the results in human readable form

* :code:`-d` specifies the depth of the directories to look in (mac)

* :code:`--max-depth` specifies the depth of the directories to look in (linux)

Examples:

* :code:`du -hd0` will show the size of the current directory

* :code:`du -hd0 *` will show the size of each file/folder in the current directory (apart from the hidden)

* :code:`du -hd0 .*` will show the size of the hidden files/directories

Wget
****
:code:`wget` web get. Fetches web pages

* :code:`-p -convertlinks` download a page with all its links and images.

* :code:`-i 'filename'` Download the pages stored in the 'filename'

* :code:`-o 'filename'` write the results (log) in 'filename'

* :code:`-rl2` : recursive for 2 levels

Examples: 

.. code-block:: bash

    wget -p -convertlinks http://some.server/webpage.html -o log txt

Producing sequences of numbers
******************************

.. code-block:: bash

    seq -f"%.0f" 1 10

For loops
*********

.. code-block:: bash

    for i in $(seq 1 10); do echo $i; done

.. code-block:: bash

    for i in $(seq 1 10)    
    do
      printf -v N "name%03d" $i   #N will be name001, name002,...
      echo $N                     #The values will be printed 
    done

The for part can be written also as 

.. code-block:: bash

    for i in `seq 1 10`    #Attention: inverse apostrophe

Rsync
*****
Copy the contents of directory b into c.

.. code-block:: bash

    rsync -av b/  c/ 

Copy the directory b itself into c.

.. code-block:: bash

    rsync -av b   c/ 

The same as rsync -av b c/, not as rsync -av b/ c/.

.. code-block:: bash

    rsync -av b   c

Delete the contents of c which are not in b.

.. code-block:: bash

    rsync -av --delete b/ c/ 

(Update) will not copy the contents of b that already exist in c and are newer. 

.. code-block:: bash

    rsync -avu b/ c/ 

Say what you'll do without actually doing it (dry-run).

.. code-block:: bash

    rsync -avn b/ c/ 

.. note:: FAT32 cannot hold all the extended information that more modern file systems store. As a result, repeated issues of the command "rsync -av /nonfat32/b /fat32/c will copy the files across repeatedly. 

Find
****

Find ``pattern`` in ``directory``.

.. code-block:: bash

    find ~/directory -regex "pattern" 

Find ``pattern`` in ``directory``, case insensitive.

.. code-block:: bash

    find ~/directory -iregex "pattern" 

Delete the found files

.. code-block:: bash

    find ~/directory -regex "pattern" --delete 

Search in a directory depth of 2 only. Depth of 1 represents the contents of the directory specified after the find command. 

.. code-block:: bash

    find ~/directory -d 2 -regex "pattern" 

Find in ~/dir1 files matching regex pattern, and move them in dir2

.. code-block:: bash

    find ~/dir1 -regex "pattern" -exec mv '{}' ~/dir2 \;

'{}' represents the files found. The command must finish with the escaped (\;) character.  Commands other then mv can also be used (e.g. cp, rm etc).

Find files only in the current directory.

.. code-block:: bash

    find ./ -maxdepth 1 -regex .... 

Set the minimum depth of the search 

.. code-block:: bash

    find ./ -mindepth 1 -regex .... 

Produce a custom print out (of the file name and size in this case)

.. code-block:: bash

    find ./ -printf 'Name: %16f Size: %6s\n' 

Find the files that have size different than 0.

.. code-block:: bash

    find ./ ! -size 0

Grep
****

Grep searches for text in files. Its syntax is

.. code-block:: bash

    grep options pattern file

e.g.

.. code-block:: bash

    grep -flags "pattern" ./ 

    flags: -r :recursive
           -e : regular expression
           -E : extended regular expressions
           -l : show files only
           -i : case insensitive

:code:`--include="*scen*"` considers only the files that have 'scen' in their name.  :code:`--exclude` works similarly.

:code:`ls -R | grep -regex "pattern"` applies grep to the results of :code:`ls -R`.

Diff
****
Compare the files in directories a and b recursively.

.. code-block:: bash

    diff -r a/ b/ 

Compare the files in directories a and b recursively and outputs only the files that differ, without showing what their differences are.

.. code-block:: bash

    diff -rq a/ b/ 

Test
****
Tests can be used to evaluate conditional expressions e.g.

.. code-block:: bash

    test -d foo.txt && rm foo.txt

which means that if foo.txt exists, then remove it. 

Sed
***

.. code-block:: bash

    ls ./ || sed 's/\(.*\)\..*/#1'

passes the results of ls ./ to sed.

The command sed :code:`'s/regexp/sub'` substitutes the regular expression with sub. In the above regular expression :code:`.` means any character.  :code:`.*` means any number of any characters. :code:`\.` is an escaped dot (literal).  So, :code:`.*\..*` matches an expression that has any number of characters, followed by a dot, followed by any number of characters (e.g. a file name as :code:`foo.txt`). Including :code:`.*` in escaped parentheses as :code:`\(.*\)` allows to refer later to the matched symbols using :code:`#1`. So, sed :code:`'s/\(.*\)\..*/#1'` matches all the file names of the form foo.txt and returns what preceeds the dot (i.e. :code:`foo`). On the other hand, :code:`sed 's/\(.*\)\.\(.*\)/#2'`, will return the extensions (i.e. :code:`txt`).

Source
******
.. code-block:: bash

    . ./script_name 

or 

.. code-block:: bash

    source ./script_name 

runs script_name, within the shell's process (not in a new subshell defined by the script). As a result any commands issued, such as 'cd' affect the shell from which the source command was issued. 

Readline library
****************
The Readline Library allows vi mode in terminal applications, such as python and R.

.. code-block:: bash

    set editing-mode vi

Cron
****

Cron can be used to run jobs at specified times

To add a job run 

.. code-block:: bash

    crontab -e

This will open the file where the job can be written. 

The format is as follows

::

    # ------------min (0-59)
    # | ----------hour (0-23)
    # | | --------day of month (1-31)
    # | | | ------month (1-12)
    # | | | | ----day of week (0 - 6) (0 to 6 are Sunday to 
    # | | | | |   Saturday, or use names; 7 is also Sunday)
    # | | | | |
    # * * * * *   command to execute

Some examples

::

    * * * * * <cmd>        # Every minute
    */5 * * * * <cmd>      # Every 5 minutes
    5 * * * * <cmd>        # Every hour at minute 5
    0, 5, 10 * * * * <cmd> # Every hour at minutes 0, 5, 10
    0 0 * * 1-5 <cmd>      # Every day from Monday to Friday at 00:00

The cron job logs can be found in :code:`/var/log/cron`.


Ls
**
:code:`-i` shows the file inode.

dircolors
*********

The colors used by :code:`ls` can be modified using :code:`dircolors`. The coloring scheme is stored in the :code:`LS_COLORS` environment variable. 

Use 

.. code-block:: bash

    dircolors -p > ~/.dircolors

to export the coloring scheme to the file :code:`~/.dircolors`. After modifying the contents of the file, load its contents to the :code:`LS_COLORS` variable using

.. code-block:: bash

    dircolors ~/.dircolors


Installing in the home folder
*****************************

How to install something locally that depends on another locally installed library
e.g. install tmux locally when libevent is installed locally as well

1. Install libevents in :code:`~/.local`.

2. Configure with 

.. code-block:: bash

    CPPFLAGS="-I/PathToHome/.local/include" LDFLAGS="-L/PathToHome/.local/lib" ./configure --prefix=$HOME/.local

3. Make with 

.. code-block:: bash

    CPPFLAGS="-I/PathToHome/.local/include" LDFLAGS="-L/PathToHome/.local/lib" make 

4. Make install with 

.. code-block:: bash

    make install

`<http://www.linuxquestions.org/questions/linux-software-2/installing-tmux-from-source-as-non-root-user-857098/>`_

glob patterns
*************
:code:`*` matches one or more characters.
:code:`[abc]` matches any character.
:code:`?` matches a single character.

Volumes
*******
List block devices

.. code-block:: bash

    lsblk

Partition disk

.. code-block:: bash

    fdisk

List the partitions in a device 

.. code-block:: bash

    sfdisk -l /dev/vdb: 


Make file system

.. code-block:: bash

    mkfs

Then you can make a directory (:code:`mkdir /mnt/data`) and mount the created partition using

.. code-block:: bash

    mount /dev/vdb1 /mnt/data

Awk
***
Suppose the the output of :code:`fdisk -l /dev/vda` is

+-----------+-----+---------+----------+---------+-----+---+--------------------+
|Device     |Boot |   Start |     End  |Sectors  |Size |Id |Type                |
+===========+=====+=========+==========+=========+=====+===+====================+
|/dev/vda1  |*    |    2048 |15988735 1|5986688  |7.6G |83 |Linux               |
+-----------+-----+---------+----------+---------+-----+---+--------------------+
|/dev/vda2  |     |15990782 |16775167  | 784386  |383M | 5 |Extended            |
+-----------+-----+---------+----------+---------+-----+---+--------------------+
|/dev/vda3  |     |16775168 |83886079  |67110912 | 32G |83 |Linux               |
+-----------+-----+---------+----------+---------+-----+---+--------------------+
|/dev/vda5  |     |15990784 |16775167  | 784384  |383M |82 |Linux swap / Solaris|
+-----------+-----+---------+----------+---------+-----+---+--------------------+

then:

.. code-block:: bash

    fdisk -l /dev/vda | grep '/dev/vda5' | awk '{print $3}'

will output the :code:`/dev/vda5` line and will feed it to awk, which will return the third space separated word, which is 16775167.

SSH keys
********

General
=======

To generate a private- public key pair use 

.. code-block:: bash

    ssh-keygen -t rsa -b 2048 


The private key will be given the name :code:`id_rsa`, the public key will be called :code:`id_rsa.pub`

You can call these keys differently if you want. You can also give a pass phrase to them but you will need to enter this each time you want to connect to the respective servers.

Keep the private key in your :code:`~/.ssh folder` and give it permission :code:`600`.

Put the public key (the text) in the :code:`~/.ssh/authorized_keys` file on the server you want to connect. An easy way to do this is using

.. code-block:: bash

    cat id_rsa.pub | ssh username@servername.com "mkdir -p ~/.ssh/ && cat >> ~/.ssh/authorized_keys"

The .ssh folder in the server should have permission :code:`700` and the :code:`authorized_keys` file should have permission :code:`600`. Wrong permissions is a common cause of connectivity issues.


Config files
============

A shortcut to connect to the server can be made by creating a :code:`~/.ssh/config` file on the client machine that will have

.. code-block:: bash

    Host gecko
      User username
      Hostname server.com
      IdentityFile ~/.ssh/id_rsa

where :code:`gecko` is the new alias for the server/username combo, :code:`username` is the username :code:`server.com` is the name of the server we are trying to connect to. 

Having made this, typing

.. code-block:: bash

    ssh gecko

will have the same result as typing

.. code-block:: bash

    ssh username@server.com

(i.e. connecting to the server. )

Port forwarding
===============

If the server sits behind a firewall, such that we first have to connect to server1 before connecting to server2, we can solve this using port forwarding. The config file entry can be modified as follows

.. code-block:: bash

    Host gecko
      User username2
      Hostname server2.com
      ProxyCommand ssh username1@server1.com -W server2.com:22
      IdentityFile ~/.ssh/id_rsa

Then typing 

.. code-block:: bash

    ssh gecko 

will have the same effect as if we first typed

.. code-block:: bash

    ssh username1@server1.com

and once logged on to server1 we typed

.. code-block:: bash

    ssh username2@server2.com

For this to happen without passwords, you'd need to have :code:`id_rsa` in :code:`~/.ssh` of the client machine and server1, and :code:`id_rsa.pub` to be copied in the :code:`~/.ssh/authorized_keys` of server1 and server2.

Bash input arguments
********************

:code:`$#` is the number of arguments

:code:`$@` is a list of all the input arguments and 

:code:`for i in $@` will iterate over all input arguments

Processor and memory 
*********************

To check the memory and processors and memory of a unix machine:

.. code-block:: bash

    $ less /proc/cpuinfo
    $ less /proc/meminfo

Using the output of a command as input to another
*************************************************

One method of achieving this is using :code:`$()`.

This causes the shell to run the command inside the parentheses and then substitute its output at that point in the larger line. Then the shell runs the line as a whole. For example

.. code-block:: bash

    cp $(find ./ -regex ".*Dex.*") ../

will find in the current directory all the files that contain 'Dex' and will copy them in the directory above. 

List too long problem
*********************

When applying a ls or a rm command to a large number of files, unix returns the 'list too long' error. One way around this is the following:

.. code-block:: bash

    find . -name "*.pdf" -print0 | xargs -0 rm

xargs, builds and executes command lines from the standard input

Difference between > and >>
***************************
:code:`>` creates a file or overwrites one that exists.
:code:`>>` appends to a file.

:code:`cat a > b`   a will replace b
:code:`cat a >> b`  a will be appended onto b 

Count files
***********
.. code-block:: bash

    ls -l *.* | wc -l

Work as root
************

.. code-block:: bash

    su

Domain resolution issues
************************
WSL (Windows Subsystem for Linux) sometimes encounters DNS resolution problems. E.g.

https://github.com/microsoft/WSL/issues/4285#issuecomment-522201021

https://gist.github.com/coltenkrauter/608cfe02319ce60facd76373249b8ca6

A solution in this case is the following:


1. Create a file: /etc/wsl.conf.
2. Put the following lines in the file in order to ensure the your DNS changes do not get blown away

.. code-block:: 

    [network]
    generateResolvConf = false

3. In a cmd window, run wsl --shutdown
4. Restart WSL2
5. Create a file: /etc/resolv.conf. If it exists, replace existing one with this new file.
6. Put the following line in the file

.. code-block:: 

    nameserver 8.8.8.8 

7. Repeat step 3 and 4. You will see git working fine now.

In the :code:`nameserver` part, you can use a different DNS server instead of of 8.8.8.8 which is a Google's. 

You can use for example the DNS server of the windows machine that runs wsl. This can be found by running :code:`ipconfig /all` in a cmd window. It can also be found by running :code:`nslookup` in a cmd window as well. 



