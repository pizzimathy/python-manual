## UIowa Remote Logins
This is a guide to remoting into the University of Iowa Computer Science Department's Linux cluster.

## Table of Contents
1. [Windows](#windows)
    - [Logging in](#1)
    - [Navigating to the global folder](#2)

## <a name="windows">Windows</a>

### <a name="1">Logging in</a>
1. Open the windows menu and type in `PuTTy`, and open it. `PuTTy` is a telnet program that allows users to remotely log into another computer.
2. In `PuTTy`'s configuration window, type `linux.cs.uiowa.edu` into the `Hostname (or IP address)` box, and click `Open`.
3. The terminal that is opened will prompt you with `login as:` where you should type your HawkID. Then, type in your password. **When you enter your password, it will not be displayed. This is correct behavior. Enter your password and hit `Enter`.**

### <a name="2">Navigating to the global folder</a>
1. After you log in, you should be directed to a specific folder. If you run the `pwd` command, this will **p**rint **w**orking **d**irectory, or the directory you're currently in. You should be in `/space/<hawkid>`.
2. To move to another directory, use the command `cd`, or **c**hange **d**irectory. To navigate into the global folder, run `cd /group/bigdata`. This will get you into the global folder.

3. Once you're in this global folder, running `ls` will **l**i**s**t the contents of the directory. This should 