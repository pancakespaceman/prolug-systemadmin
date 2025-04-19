# ProLUG System Administration for the Enterprise - Unit 1

## Syllabus

(Book)[https://professionallinuxusersgroup.github.io/lac/intro.html]


Set value, check value - only things you do in a CLI

## Primer

Getting Help and understanding syntax
Basic command utilities
file structure
  - hierachical structure
  - absolute vs relative path

### What is syntax

Syntax is is exact wording of how we issue a command to the CLI

- Command - mode commands are called by their names
- Options - sometimes called switches are typically preceded by a - character on the cli
- arguments - optional values passed to the command for execution (i.e. file names, locations, data)

```
pwd - present working directory
clear - clear terminal screen (not delete)
whoami

pid
ppid
ps
ps -ef
top
kill
uniq - removes duplicate lines from a file
diff - compares two files
```

The command prompt
  - case sensitive
```
$ - user prompt
# - root command prompt
```

Getting help

```
info - interactive command that list all commands available
man
--help

help with bash:
apropos, man -f command, whatis command
```


```

Find commands you need using:
    
apropos <keyword> - Each manual page has a short description available within it.   apropos searches the descriptions for instances of keyword. This helps to find if a command will suit your needs.
apropos -s <section_number> <keyword> - Limit the search of keyword to section <section_number> of the manpages.
Read this short refresher on Linux Man Pages and how they work, how sections work in manpages, how man command works! Before proceeding!
man <command> - Search for a particular command in all manpages and display the first instance
man -f <command> or whatis <command>  - Searches for the <command> across all sections of the manual and lists the results with the corresponding section number and the one-line description from each manpage.
man <manpage_section_number> <term> - Search for <term> in section <manpage_section_number> of the manpage and display it. Must for opening specific commands like adduser(8), where 8 is the <manpage_section_number>.
whereis <command> - Lists paths of where the command is installed and paths of its corresponding manpages.
which <command>- shows the exact location of a given executable. Only works for executable programs
```


- / is the root directory for all systems
- /boot holds static files of the bootloader
- /bin essential command binaries needed for single user mode
- /root is the root (admin's) home directory
- /home where user home directories are stored
- /usr info typically used by the system
  - /usr/bin contains linux standard utilities programs


