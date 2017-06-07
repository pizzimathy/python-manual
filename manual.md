**June 6, 2017**

This is the definitive guide to setting up Python on a Macintosh. The bold sections are the ones that contain just the bare essentials, but the other stuff is pretty interesting as well (feel free to skim).

## Table of Contents
1. [Prerequisites](#1)
	- [Reading](#1.0)
	- [UNIX-flavored](#1.1)
	- [macOS Root Folders](#1.2)
	- [Bash and Environment Variables](#1.3)
	- [**Run All the Checks!**](#1.4)
2. [Homebrew](#2)
	- [**Installation**](#2.1)
	- [Housekeeping](#2.2)
3. [**Python**](#3)
	- [Warnings](#3.0)
	- [Python 2](#3.1)
	- [Python 3](#3.2)
	- [Pip](#3.3)
	- [Errors](#3.4)
	- [Running a Program](#3.5)
4. [Development Tools](#4)
    - [Editors and IDEs](#4.1)
    - [Version Control](#4.2)
    - [Style](#4.3)
5. [Conclusion](#5)

## <a name="1">Prerequisites</a>
Running a few checks prior to diving into setup will make things run more smoothly, and can help prevent problems later on. The goals of the prerequisite section are to find out which tools the computer already has, which ones need installing, and how they relate to Python itself.

### <a name="1.0">Reading</a>
In this instruction manual, there's a lot of explanation. For sanity's sake, I've linked all the sections in the manual to their spot in the table of contents.

We'll be using Terminal a lot, so make sure that you always have that window open. If you don't know how to find it, there are two ways to get there:

1. Open a new Finder window and go to the `Applications` folder. In `Applications`, there is a folder called `Utilities`. Open that, and the Terminal application is in there.
2. Use the `⌘ + <spacebar>` command to open Spotlight, and type in "Terminal". It'll find Terminal, and then just hit enter (or double-click, whichever you prefer).

The notation I use for describing a Terminal command is like this:

`$ <command> [option 1] ... [option n]`

The `$` is just a separator, and it's persistent in all new shells. The `<command>` and `[option 1] ...` parts are to show the command and options we'll be using with the shell. In short, you don't have to type the `$`.

Essentially any computer text will be formatted `like this`, so it can be differentiated from regular post text.


### <a name="1.1">UNIX-flavored</a>
macOS is based on UNIX, an operating system architecture developed at Bell Labs in the 70s. UNIX, as one of its main principles, allows shell scripting so that users can interact with the operating system. Because of this, macOS is quite modular - users can quickly and easily import third-party software and use it right out of the box with no configuration. We are going to be installing third-party software (namely Python), and a tool included with Python allows you to install even *more* third-party packages for Python to use.

### <a name="1.2">macOS Root Folders</a>
In most UNIX filesystems, there is a folder called `/`. This is the root directory, and every single file that your computer uses is inside that folder. It makes sense, then, that some of those folders are restricted so that inexperienced users ("haxors", the computer-illiterate, pets, etc.) can't write or delete from them and potentially cause harm. This is where the `/usr/` folder comes in.

`/usr/` is the directory that stores the majority of a computer's programs that interface with the operating system, but are still operable to the user. Many of these are stored in subdirectories of user, namely `/usr/bin/`, `/usr/lib/`, and `/usr/local/`.

`/usr/bin/` is mainly used to store binaries. Typically commands like `ssh`, `which`, and the like are stored there. `/usr/lib/` contains libraries for programming languages like PHP, Ruby, the system itself, and default installations for Python (in macOS Sierra, Python 2.6 and 2.7 are installed). `/usr/local/`, however, is our sandbox. This is the folder dedicated to local or third-party programs, and is managed by the system's administrator.

Additionally, many of the tools baked into a macOS installation are written by **GNU**'s **N**ot **U**nix (or just GNU). This group of hackers provides multiple suites of useful tools, namely `gcc`, a set of compilers, `coreutils`, `screen`, and the **B**ourne-**a**gain **sh**ell, or Bash - the default shell for macOS.

### <a name="1.3">Bash and Environment Variables</a>
Bash (commonly referred to as a Bash shell, which doesn't make much sense because, expanded, that's just "bash shell shell") is the default shell for macOS. When a user begins to use Terminal, they are "logged in" to the default login shell. While this user is logging in, Bash reads from three files -  `~/.bash_profile`, `~/.bash_login`, and `~/.profile` in that order. This allows any user to set preferences for Bash itself - how the prompt looks, special-use variables, sourcing other files, running background tasks, etc. We will be modifying some of these files later on, but we'll be doing it responsibly. Hopefully you'll become familiar enough to modify any of those three files later on and work some cool Bash magic.

To move around in Bash, use the commands `$ cd <directory>`, `$ ls`, and `$ pwd`. `cd` means "change directory", `ls` means "list out everything in this directory", and `pwd` means "print the path to the directory I'm currently in". When you open a new shell, you start at the `~` folder, which is your user's folder. Try navigating to some files on your desktop and listing them. Practicing this is key to using the command line efficiently.

To "step back" to a previously visited directory, you can run `$ cd ..`. For example, if I'm in `~/desktop/`, running `$ cd ..` will take me back to `~/`. To stay in or reference the current directory, you can simply use `.` (e.g. `$ cd .` will do nothing, as you stay in the same directory).

Bash keeps track of a set of global presets called environment variables. These values are available in all new shells and open to all languages that interface with the OS once they have been set (or changed). They have universal read access, and are used by countless programs to find installations of software, packages, and other necessary programs.

When a command is run in Bash, it uses the `PATH` environment variable to search for the executable file the command is tied to. You can look at what's stored in `PATH` by running `$ echo $PATH`. Mine returns a huge list of directories where the commands I run *could* be located.

### <a name="1.4">Run All the Checks!</a>
In order to install Python, we're going to install a package manager called [Homebrew](https://brew.sh). Homebrew manages the versions and installations of all packages. It's written in Ruby, so we're going to check that your pre-packged Ruby installation is still there.

Run

`$ which ruby`

which should return `/usr/bin/ruby/`. Next, we're going to check that you have git installed.

Git is a version control system, which is basically a tiny internal database that tracks all the changes made to a file (or set of files, or directory). Nearly every single minor or major collection of programs uses git to track changes and manage different versions of files (even Homebrew uses git to track itself). macOS comes with a pre-installed version of git, but it's old(er) and isn't as useful as newer versions. To check that git is installed, run

`$ which git`,

which should return `/usr/bin/git/`. Running

`$ git --version`

should return `git version 2.11.0 (Apple Git-81)`. Since we have those, we can now move on to installing Homebrew.

## <a name="2">Homebrew</a>
Homebrew is the a package manager that should be in every developer's toolbox. From here you can effectively manage hundreds of packages, each of which is useful for development. Many major languages and runtimes have their packages registered with Homebrew (think Python, Node.js, etc.) as well as countless other useful programs.

### <a name="2.1">Installation</a>
To install Homebrew, copy this command and paste it into Terminal. 

`/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`

You can also go [here](https://brew.sh/) and read the instructions from the Homebrew people.

### <a name="2.2">Housekeeping</a>
It's important to keep your formulae (Homebrew-installed packages) and Homebrew itself up to date. You can run a few commands to help with maintaining your packages and keeping the number of extra packages small.

[**Here**](http://docs.brew.sh/Manpage.html) is the man page for Homebrew if you want to look at these commands in more detail.

1. `$ brew update` fetches the newest version of Homebrew from Github.
2. `$ brew upgrade` checks all formulae for updates, and if any exist, install those updates. Check the man page for more options
3. `$ brew doctor` returns potential issues with any of your formulae or Homebrew itself.
4. `$ brew prune` removes all dead symlinks from `/usr/local/bin/` so you don't have any broken links.

Make sure to take a look at the man page and find some other commands that could be useful. StackOverflow also has a pretty extensive set of questions about Homebrew, so any questions you have can be answered there better than here.

## <a name="3">Python</a>
This is the section we've all been waiting for - installing Python. It's actually quite simple, and there shouldn't be too many hiccups. We'll go over how to install both versions of Python (2.7.x and 3.x.x) as well as any errors you may encounter.

### <a name="3.0">Warnings</a>
- Do *not* use `sudo` when installing any Homebrew, Pip, NPM, or any other third-party packages. `/usr/local/` is for locally installed or third-party software, and the use of `sudo` can affect the read/write permissions of `/usr/local` by changing its owner. When installing Homebrew, it asked for your password. This is for *write permissions* to `/usr/local/`. After it's installed, Homebrew takes care of this for you by installing packages in `/usr/local/Cellar/` and then creating symlinks to executable files and placing them in `/usr/local/`, whose permissions are managed by Homebrew.

- There is a difference between `python`, `python3`, `pip`, and `pip3`. `python` and `pip` run scripts and install packages for Python 2.x respectively, while `python3` and `pip3` run scripts and install packages for Python 3.x.x respectively.

### <a name="3.1">Python 2</a>
Python 2.7 comes preinstalled on all Macs. However, it usually isn't the most current version, and Python's package manager, Pip, isn't installed.

To install Python 2, run

`$ brew install python`.

That's it! If you have errors, check the section on Errors.

### <a name="3.2">Python 3</a>
Many new applications and frameworks are written with Python 3, as it is considered the standard Python implementation. Support for Python 2.7 will end in 2020 (per Guido van Rossum, Python's creator).

To install Python 3, run

`$ brew install python3`. Again, for any errors, the Errors section.

### <a name="3.3">Pip</a>
Pip is the package management ecosystem for Python. The Python Software Foundation keeps a registry of useful third-party Python addons that are typically open-source and publicly available.

When using Python, just like in the warnings above, `pip` and `pip3` are different. Installing a package with `pip install <package>` can't be found by `python3` or `pip3`, and vice versa.

However, Pip is extremely useful, and [the man page](https://pip.pypa.io/en/stable/) is worth looking at.

### <a name="3.4">Errors</a>

If `$ brew install python` or `$brew install python3` gives you an error saying that:

- The bottle can't be compiled because `gcc` is not installed, run `$ xcode-select --install` to install Apple Command Line Tools (which includes `gcc`, the GNU Compiler Collection). Then, run `$ brew install python3`.
- `/usr/local/bin` may not be writable, change the permissions of the directory by running ``$ sudo chown -R `whoami` /usr/local/``.

If `$ which python` or `$ which python3` doesn't return `/usr/local/bin/python/` or `/usr/local/bin/python3` respectively, or when you run `$ python` or `$ python3` you get a `command not found` error, there may be a linking issue. Follow these instructions in order, and stop when the problem is fixed.

1. Run `$ brew doctor` to find any problems, and then run `$ brew unlink python && brew link python` (or `python3`) to try and rewrite the existing symlinks.
2. Run `$ brew link --dry-run --overwrite python` (or `python3`) to check if a `python3` already exists in `/usr/local/bin/`. If it does, you can overwrite it by running `$ brew link --overwrite python`.
3. If none of the above have worked, run `$ echo $PATH`. If the directory `/usr/local/bin` doesn't appear there, you'll have to modify your `PATH` environment variable. You can do this by editing `~/.bash_profile` or `~/.profile` using the [Vim editor.](https://www.linux.com/learn/vim-101-beginners-guide-vim) Add `export PATH=/usr/local/bin/:$PATH` and save the file. This adds `/usr/local/bin/` to the list of locations Bash looks for executable programs.

### <a name="3.5">Running a Program</a>
Let's write a little test program. Navigate to the desktop in Terminal and create a new file called `test.py` (you can do this by running `$ touch test.py`). Edit it in the editor of your choice, and have the only line be

```
print("Hello, World!")
```

That's it! We can run the program in Terminal by running `$ python3 test.py`, and it'll print `Hello, World!` happily into the console window. If we want to add a layer of complexity, we'll get into hashbangs. Go back to editing the file, and add an additional bit of text *on the first line*.

```
#!/usr/bin/env python3
print("Hello, World!")
```

Let's break this down.

- `#!` tells Bash that this program has a specific interpreter.
- `/usr/bin/env` is not a location, but a command that lists all environment variables. You can actually see these by running `$ env`. In any case, Bash is looking in the `PATH` variable listed by `/usr/bin/env` in order to find a specific interpreter, which reads the program and executes it.
- `python3` is the interpreter that Bash looks for. Awesome!

Then, instead of running `$ python3 test.py` (which can still be run), we can do something more interesting. First, we change the permissions of the file using `chmod` (change mode) by running `$ chmod 700 ./test.py`. Then, we can simply run `$ ./test.py` as an executable file, and get back `Hello, World!`


## <a name="4">Development Tools</a>
Python is great by itself, but usually developers like a little help when it comes to actually writing the code (we're lazy at heart, which is why we're writing code in the first place). A brief overview of some reliable addons to your toolbox is in order.

### <a name="4.1">Editors and IDEs</a>
Every programmer has their preferred suite of code editing and management software. But for Python's purposes, a few stand out.

All Python installations come with a program called IDLE installed. This is basically a user-friendly Python shell. IDLE should be used for programs that are < 10 lines in length (I don't even use it at all). The Python 2 IDLE can be opened by running `$ idle` and Python 3 by `$ idle3`.

Another option is our old friend Vim. I've customized Vim fairly heavily, to the point where it has search-and-highlight functionality, directory structures, etc. Vim is great to use with small- to mid-sized projects consisting of a few directories and a reasonable amount of files.

Finally, for big projects, JetBrains is the way to go. Their IDEs have awesome debugging, can run programs in the window, keep track of version control, and have all the cool bells and whistles. PyCharm, JetBrains' Python IDE, is the editor of choice for anything with tons of files, directories, and a lot of code to keep track of.

### <a name="4.2">Version Control</a>
Remember when we installed Git earlier? Well, it's a crazily useful tool for version control. I recommend that every project you start be managed with Git. It has a learning curve, but it'll save your ass in a pinch.

The basics are thus:

Say you have a directory `project/`. You just made it, and you have a bunch of files in there, like `main.py` and `setup.py`. Using terminal, navigate into your project folder by running `$ cd /path/to/project`.

- To start a new Git repository, run `$ git init`. This initializes a new mini-database, and it'll keep track of all your files in a hidden folder in `project/` called `project/.git`. If (for some reason) you want to remove the repository, delete the `.git` folder by running `$ rm -rf /path/to/project/.git/`.
- You can check the status of your files by running `$ git status`. This shows which files are new, which have changes made to them, and which are saved in the database already.
- When you want to save changes to a file, you create what's called a commit. You are, essentially, committing changes to be permanently stored in the tiny database. To do this, run `$ git add <files>`, and then `$ git commit -m "message about the commit"`. 

You can also put your repositories up online by using a remote repository. Git has a built-in feature that allows you to shove your files into a remote server and have the code available to the internet (if you so desire). Go to [GitHub](https://github.com) and follow their instructions on how to use remotes. Furthermore, read up as much as you can on Git - it's a widely-used version control system and extremely important to development practices.

### <a name="4.3">Style</a>
Follow the [PEP guidelines for code layout](https://www.python.org/dev/peps/pep-0008/). Plain and simple.

## <a name="5">Conclusion</a>
Hopefully this has been helpful. If you'd like to suggest revisions, additions, or anything else, you can [contact me](http://apizzimenti/#/contact) or open a pull request on GitHub.