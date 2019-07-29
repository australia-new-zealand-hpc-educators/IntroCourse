# The Linux Command Line Environment 

## This is a GNU/Linux CLI World 
<img src="https://raw.githubusercontent.com/UoM-ResPlat-DevOps/SpartanIntro/master/Images/gnulinux.png" align="center" height="25%" width="25%" vspace="5" hspace="5" />

## This is a GNU/Linux CLI World
* The command­line interface provides a great deal more power and is very resource efficient. 
* GNU/Linux scales and does so with stability and efficiency.
* Critical software such as the Message Parsing Interface (MPI) and nearly all scientific programs are designed to work with GNU/Linux. 
* The operating system and many applications are provided as "free and open source" which are better placed to improve, optimize and maintain.

## File System Hierarchy
* When a user logs in on a Linux or other UNIX-like system on the command line, they start in their home directory (`/home/<<username>>`). Project directory in `/data/projects/<<projectID>>`.
* See `https://swcarpentry.github.io/shell-novice/fig/standard-filesystem-hierarchy.svg`
* "Everything in the UNIX system is a file" (Kernighan & Pike, 1984, 41). 

## Exploring The Environment
| Command     | Explanation                                                                |
|:------------|:--------------------------------------------------------------------------:|
|`whoami`   | "Who Am I?; prints the effective user id                                     |
|`pwd`      | "Print working directory"							   |
|`ls`       | "List" directory listing                                                     |	

## Command Options
* Linux commands often come with options expressed as: `<command> --<option[s]>`
* Options can be expressed as full words or abbreviated characters.

| Command     | Explanation                                                                |
|-------------|:--------------------------------------------------------------------------:|
|`ls -lart`   | Directory listing with options (long, all, reverse time)                   |
|`ls -lash`   | Directory listing with options (long, all, size in human readable)	   |

## The Online Manual
Linux commands come with "man" (manual) pages, which provide a terse description of the meaning and options available to a command. A verbose alternative to man is info. 

| Command             | Explanation                                                      |
|:--------------------|:-----------------------------------------------------------------|
|`man <command>`       | Display the manual entry for the command                        |
|`info <command>`     | A verbose description of the command                             |
|`whatis <command>`  | A terse description of the command                                |

## Pipes
Linux also have very useful 'pipes' and redirect commands. To pipe one command through another use the '|' symbol.

| Command            | Explanation                                                                |
|:-------------------|:--------------------------------------------------------------------------:|
| <code>who -u  &#124; less</code> | "Who" shows who is logged on and how long they've been idle. |
| <code>who &#124; wc -l</code> | "Who" piped through wordcount command, count lines.             |
| <code>ps -afux &#124; less</code> | "ps" provides a list of current processes. Check `man ps`   |

## Redirects
To redirect output use the '>' symbol. To redirect input (for example, to feed data to a command) use the '<'. Concatenation is achieved through the use of '>>' symbol. 

| Command           | Explanation                                                          |
|:------------------|:--------------------------------------------------------------------:|
| `w > list.txt`  | 'w' is a combination of who, uptime and ps -a, redirected            |
| `w >> list.txt` | Same command, concatenated                                           |

## Files and Text Editing
* Linux filenames can be constructed of any characters except the forward slash, which is for directory navigation. However it is best to avoid punctuation marks, non-printing characters (e.g., spaces). It is *much* better to use underscores or CamelCase instead of spaces, newlines etc (including in job names).
* Linux is case-sensitive with its filenames (e.g., list.txt, LIST.txt lisT.txT are different).

## Files and Text Editing
* Files do not usually require a program association suffix, although you may find this convenient (a C compiler like files to have .c in their suffix, for example). 
* The type of file can be determined with the `file` command. The type returned will usually be text, executable binaries, archives, or a catch-all "data" file.
* There are three text editors usually available on Linux systems on the command-line. These are `nano` (1989, as `pico`) and `vim` (or `vi`), and or `emacs` (both 1976). See `https://www.vimgolf.com/`

## Copying Files to a Local Systems
To get a copy of the files from an external source to your home directory, you will probably want to use `wget`, or `git`, or `scp`.

| Command           | Explanation                                                          |
|:------------------|:--------------------------------------------------------------------:|
| `wget URL`      | Non-interactive download of files over http, https, ftp etc.           |
| `git clone URL` | Clone a repository into a new directory.                               |

## Copying Files Within a Local Systems 
To copy a file from within a system use the `cp` command. Common options include `-r` to copy an entire directory

| Command           | Explanation                                                          |
|:------------------|:--------------------------------------------------------------------:|
| `cp source destination`      | Copy a file from source to destination                    |
| `cp -r source destination` | Recursive copy (e.g., a directory)		           |
| `cp -a source destination` | Recursive copy as archive (permissions, links)		   |

## Copying Files Between Systems
To copy files to between systems desktop use SCP (secure copy protocol) or SFTP (secure file transfer protocol), combining the ssh and cp functionality. The `cp` options can also be used. The source or destination address should also require a remote shell login.

| Command           | Explanation                                                          |
|:------------------|:--------------------------------------------------------------------:|
| `scp source.address:/path/ destination.address:/path/`| Copies files on a network        |

## Synchronising Files and Directories
* The `rsync` utility provides a fast way to keep two collections of files "in sync" by tracking changes.    
* The source or destination address should also require a remote shell login.    
For example; `rsync -avz --update lev@spartan.hpc.unimelb.edu.au:files/workfiles .`

## Synchronising Files and Directories

| Command           | Explanation                                                          |
|:------------------|:--------------------------------------------------------------------:|
| `rsync source destination`| General rsync command  |
| `rsync -avze ssh username@remotemachine:/path/to/source .` | With ssh encryption |

## Synchronising Files and Directories
* The `rsync -avz` command ensures that it is in archive mode (recursive, copies symlinks, preserves permissions), is verbose, and compresses on transmission. 
* The --update restricts the copy only to files that are newer than the destination. 
* Note that rsync is "trailing slash sensitive". A trailing / on a source means "copy the contents of this directory". Without a trailing slash it means "copy the directory".

## Synchronising Files and Directories IV
* Rsync can be used in a synchronise mode with the --delete flag.  Consider this with the `-n`, or `--dry-run` options first!

| Command           | Explanation                                                          |
|:------------------|:--------------------------------------------------------------------:|
| `rsync -avz --update source/ username@remotemachine:/path/to/destination| Synchronise, keep older files  |
| `rsync -avz --delete source/ username@remotemachine:/path/to/destination| Synchronise, absolutely |

## Creating Directories, Moving Files
* Directories can be created with the `mkdir` command (e.g., `mkdir braf`).
* Files can be copied with the `cp` command (e.g., `cp gattaca.txt gattaca2.txt`)
* Files can be moved with the `mv` command (e.g., `mv gattaca2.txt braf`)

## File Differences
* File differences can be determined by timestamp (e.g., `ls -l gattaca.txt braf/gattaca.txt`)
* Content differences can be determined by the `diff` command (e.g., `diff gattaca.txt braf/gattaca.txt`)
* For a side-by-side representation use the command `sdiff` instead.
* The command `comm` can compare two files, lines by line (e.g., `comm gattaca.txt braf/gattaca.txt`).

## Searches and Wildcards
* To search for files use the find command (e.g., `find . -name "*.txt"`). Compare with `locate` and `whereis`.
* To search within files, use the `grep` command (e.g., `grep -i 'ATEK' braf/*` or `grep -l`)
* The most common wildcard is `*`, but there is also `?` (single character). Expansion is 'globbing'.
* There are also range searches (e.g., `[a-z]` any character between a and z, inclusive)

## Deletions
* Files can be deleted with the `rm` command (e.g., `rm gattaca.txt`)
* Empty directories can be deleted with the `rmdir` command (e.g., `rmdir braf`)
* Directories, files, subdirectories etc can be delted with `rm -rf <<dir>>`
* BE VERY CAREFUL, ESPECIALLY WITH WILDCARDS!
* Consider the difference between `rm matlab *` to `rm matlab*`.

## Why The File Differences Mattered
<blockquote>
BRAF is a human gene that makes a protein (imaginatively) named B-Raf. This protein is involved in sending signals inside cells, which are involved in directing cell growth. In 2002, it was shown to be faulty (mutated) in human cancers. In particular the difference that between the two files "ATVKSRWSGS" and "ATEKSRWSGS" is the difference which leads to susceptibility to metastatic melanoma. 
</blockquote>


