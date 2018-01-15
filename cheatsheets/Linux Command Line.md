# Linux Note

| command                  | result                                   |
| ------------------------ | ---------------------------------------- |
| date                     | print date                               |
| cal                      | print calender                           |
| df                       | print hard disk usage                    |
| free                     | print memory usage                       |
| pwd                      | print working directory                  |
| cd                       | change directory                         |
|                          | dot (.): current directory, dot dot (..): parent directory |
|                          | dash (-):  the previous working directory |
|                          | ~*user_name*: home directory of the user |
| ls                       | list directory contents                  |
| file                     | print file information                   |
| less                     | show text file                           |
|                          |                                          |
| cp                       | copy files and directories               |
| mv                       | move/rename files and directories        |
| mkdir                    | create directories                       |
| rm                       | remove files and directories             |
| ln                       | create hard and symbolic links           |
|                          |                                          |
| type                     | Indicate how a command name is interpreted |
| which                    | Display which executable program will be executed |
| help                     | Get help for shell builtins              |
| man                      | Display a command's manual page          |
| apropos                  | Display a list of appropriate commands   |
| info                     | Display a command's info entry           |
| whatis                   | Display a very brief description of a command |
| alias                    | Create an alias for a command            |
|                          |                                          |
| \> *file*                | redirect standard output to a file       |
| \>\> *file*              | append standard output to a file         |
| 2\> *file*               | redirect standard error to a file        |
| &> *file*                | redirect standard output and error to one file |
| &>> *file*               | append standard output and error to one file |
| 2> /dev/null             | throw it away                            |
|                          |                                          |
| cat                      | display files without paging             |
| cat [*file...*] > *file* | concatenate files                        |
| cat > *file*             | write new file                           |
| cat < *file*             | redirect input from a file               |
| sort                     | Sort lines of text                       |
| uniq                     | Report or omit repeated lines            |
| grep                     | Print lines matching a pattern           |
| wc                       | Print newline, word, and byte counts for each file |
| head                     | Output the first part of a file          |
| tail                     | Output the last part of a file           |
| tee                      | Read from standard input and write to standard output and files |
| *command1* \| *command2* | pipeline                                 |
|                          |                                          |
| echo                     | print                                    |
|                          |                                          |
| [[:upper:]]              |                                          |
| $((*expression*))        | Arithmetic Expansion                     |
| echo $(ls)               | Command Substitution                     |
| echo Number_{1..5}       | Brace Expansion                          |
| echo Front-{A,B,C}-Back  |                                          |
| echo $USER               | Parameter Expansion                      |
| " "                      | suppress expansions except parameter expansion, arithmetic expansion, and command substitution |
| ' '                      | suppress all expansions                  |
|                          |                                          |
| clear/Ctrl+L             | clear the screen                         |
| history                  | Display the contents of the history list |
| Ctrl+A                   | Move cursor to the beginning of the line |
| Ctrl+E                   | Move cursor to the end of the line       |
| Alt+F                    | Move cursor forward one word             |
| Alt+B                    | Move cursor backward one word            |
| Ctrl+K                   | Kill text from the cursor location to the end of line |
| Ctrl+U                   | Kill text from the cursor location to the beginning of the line |
| Ctrl+Y                   | Yank text from the kill-ring and insert it at the cursor location |
| \<tab\> \<tab\>          | Display list of possible completions     |
| Alt+*                    | Insert all possible completions          |
|                          |                                          |
| id                       | Display user identity                    |
| chmod                    | Change a file's mode                     |
| ps                       | Report a snapshot of current processes   |
| pstree                   | Outputs a process list arranged in a tree-like pattern showing the parent/child relationships between processes. |
| top                      | Display tasks                            |
| jobs                     | List active jobs                         |
| bg/fg                    | Place a job in the background/foreground |
| kill                     | Send a signal to a process               |
| killall                  | Kill processes by name                   |
| shutdown                 | Shutdown or reboot the system            |
| printenv                 | Print part or all of the environment     |
| /etc/profile             | A global configuration script that applies to all users for login shell sessions |
| ~/.bash_profile          | A user's personal startup file for login shell sessions |
| /etc/bash.bashrc         | A global configuration script that applies to all users for non-login shell sessions |
| ~/.bashrc                | A user's personal startup file for non-login shell sessions |