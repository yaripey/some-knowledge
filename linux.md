# Linux notes

These notes contain information that is needed for basic tasks while working with Linux.



## Home directories
When you login to your Linux system, you are automatically placed in your home directory. This is a main directory and any user has his own one. You can always refer to your home directory by using the _~_ character. Or you can use it to specify other user's directory if you follow _~_ by userID, like _~bookie_.

----------------------------------------------------

## Wildcard
When using commands with files, we can select multiple files or select files that we don't know the full name of. Wildcards come to help us. These are special characters, like _*_ or _?_.

### *
This character means _any number of any characters_. We can use just one _*_ to select all files in the current directory. Or, if we want all files, that has names ending with "ing", we can use _*ing_. But _*_ could also mean 0 characters, so _*ing_ will also select "ing" file.

### ?
This character means exactly one any character. It can be useful when we know the length of the filename and some of it's characters. For example, we can use _sp??t_ to selet "sport".

----------------------------------------------------

## Special characters
By default your user commands will print their return messages to the console, but sometimes we need to save them to a file or send somewhere else. In this case we can use special _>_ and _>>_ symbols. The syntax is simple:
``` cat file1 file2 >> file3 ```
What happened here is _cat_ command concatinated file1 and file2 and then appended it's return message to file3. If we would use _>_ instead, it will __replace__ file3's content.

Another special character is a pipe - _|_. This character is similar to arrows that were previously described, but this one sends output from one command as input to another one. Example:
``` cat file1 file2 | lpr ```
This example will concatenate file1 with file2 and send the output to the default printer.

----------------------------------------------------

## Useful commands

Important to know that linux commands are case sensitive.

### ls
Lists contents of the following directory.

#### -l
This flag allows you to view more information about each file with _ls_ command.

Example of the format of the output is below:

```
-rw-r--r-- keeper prim 547 9:31 chimps
```

- The first characters gives an idea of the file type, _-_ means it is a file, _d_ means it is a directory.
- Next nine characters (in this case - _rw-r--r--_, show the security)
- Next goes the owner of the file.
- Next is a group owner of the file.
- Next is filesize in bytes.
- Next goes last modification time.
- Final column is the filename.

Mor info can be found in the description of the _chmod_ command.

### more [filename]
Shows you the content of the specified file. To get to the next page you need to press _spacebar_.


### mkdir [dirname]
Creates a folder with a specified name.

### mv [filepath] [locationpath]
Moves a specified file to specified location.

Also allows you to rename files and directories by moving it to the same folder, but with different filename.

### cp [filepath] [locationpath]
Copies a file to a specified location.

#### -r
This flag allows you to copy not a file, but a directory.

### rm [filepath]
Removes a specified file. To remove a _non-empty_ folder with this command, give it the _-r_ flag.


### rmdir [dirname]
Removes a specified _empty_ directory.

### cd [path]
Changes your wokring directory to a specified one.

### pwd
Prints your working directory.

### chmod
Allows you to change _mode_ (permissions) of the file.

#### About security
- The _r_ means you can read the file's contents.
- The _w_ means you can write or modify the file's content.
- The _x_ means you can execute the file. (This permission is given only if the file is a program.)

So the sign _rw-r--r--_ actually tells us about rights on the file that are given to different people. First 3 characters are about the file owner, the second 3 are about the group owner and last 3 are about _The World_. _World_ referrs to anyone who can access the file.

#### Syntax
This is better shown on examples. Groups of users can be referred to as _u_ for _user_, _g_ for _group_ and _o_ for _others_. So you just need to give _chmod_ right users and parameters and the specify the filename. Like so:

```
chmod o+x gorillas
```

The command above will give everybody a permission to execute gorillas.

```
chmod ugo-rwx gorillas
```

In this case we are revoking all permissions from everybody.

### groups
Lists your group memberships.

### man
User manual for using Linux.

#### man -k
The _-k_ flag allows you to provide keywords for man to search for in commands descriptions.

### finger [user]
Shows you the specified user's usedID and home directory.

### find
Allows you to search for files in your filesystem.

Syntax looks like this:
```
find [starting location] [type] [what to search]
```

_Type_ flag here specifies _by what_ exactly are you searching. The most common will be _-name_, but you can also search by type, site, date etc.

### cat
Returns a concatenation of two given files.

### lpr
This command will send content to a printer.

#### -P
If you want to send it to a specific printer instead of the default one, you can specify a printer with this flag. Like so: ```lpr -P hp file```

### lpq
This command displays printer queue.

#### -P
Just as for _lpr_ command, you can specify which printer's queue to show by this flag.

### lprm
This command allows you to remove content from printer queue.

### df
Allows you to see how much space you have on your disk. If you provide this command a path to a file, it will show you the disk that file is stored on.

### ps
Get information about processes.

#### ps aux
You can get a detailed list of processes by running _ps aux_, but be aware that _aux_ is written without __-__.

### grep
A useful command for searching for patterns in data. It can search for data in a file and will return lines that contain the data that was searched for. It is also useful with a pipe. For example, we can search for information about a particular process like so:
``` ps aux | grep process_name```

### kill
This command allows you to kill a certain process, you just need to specify the PID.

#### -9
Sometimes, a simple _kill_ call is not enough to shutdown a process, as it allows it to cleanup after itself. But if process ignores the kill signal we can initiate a _kill immediately_ signal by runnin ```kill -9 PID```.