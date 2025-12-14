1. `date`
	output the date

2. `echo hello`
	The shell parses the command by splitting it by whitespace, and then runs the program indicated by the first word, supplying each subsequent word as an argument that the program can access. If you want to provide an argument that contains spaces or other special characters (e.g., a directory named “My Photos”), you can either quote the argument with `'` or `"` (`"My Photos"`), or escape just the relevant characters with `\` (`My\ Photos`).

3. `echo $PATH`
	If the shell is asked to execute a command that doesn’t match one of its programming keywords, it consults an _environment variable_ called `$PATH` that lists which directories the shell should search for programs when it is given a command.
	When we run the `echo` command, the shell sees that it should execute the program `echo`, and then searches through the `:`separated list of directories in `$PATH` for a file by that name. When it finds it, it runs it (assuming the file is _executable_; more on that later). We can find out which file is executed for a given program name using the `which` program. We can also bypass `$PATH` entirely by giving the absolute path to the file we want to execute.

4. `which echo`
	output the path of the first executive file in $PATH


5. `pwd` `cd` `.` `..`
	A path on the shell is a delimited list of directories; separated by `/` on Linux and macOS and `\` on Windows. On Linux and macOS, the path `/` is the “root” of the file system, under which all directories and files lie, whereas on Windows there is one root for each disk partition (e.g., `C:\`). We will generally assume that you are using a Linux filesystem in this class. A path that starts with `/` is called an _absolute_ path. Any other path is a _relative_ path. Relative paths are relative to the current working directory, which we can see with the `pwd` command and change with the `cd` command. In a path, `.` refers to the current directory, and `..` to its parent directory

6. `ls` `ls -h` `ls --help`
	To see what lives in a given directory, we use the `ls` command. Unless a directory is given as its first argument, `ls` will print the contents of the current directory. Most commands accept flags and options (flags with values) that start with `-` to modify their behavior. Usually, running a program with the `-h` or `--help` flag will print some help text that tells you what flags and options are available. 

7. `man ls`
	In macOS, this command  equals to `ls -h`, press `q` to exit.

8. `ls -l` `ls -l ~/hackerrr`
	The output just like:
	 `drwxr-xr-x  42 hackerrr  staff   1.3K 12  5 21:47 CppHomework/`
	 This gives us a bunch more information about each file or directory present. First, the `d` at the beginning of the line tells us that `CppHomework/` is a directory. Then follow three groups of three characters (`rwx`). These indicate what permissions the owner of the file (`hackerrr`), the owning group (`staff`), and everyone else respectively have on the relevant item. A `-` indicates that the given principal does not have the given permission. Above, only the owner is allowed to modify (`w`) the `missing` directory (i.e. add/remove files in it). To enter a directory, a user must have “search” (represented by “execute”: `x`) permissions on that directory (and its parents). To list its contents, a user must have read (`r`) permissions on that directory. For files, the permissions are as you would expect. Notice that nearly all the files in `/bin` have the `x` permission set for the last group, “everyone else”, so that anyone can execute those programs.

9. `mv` `cp` `rm`
	`mv`to rename or move a file, `cp` to copy a file(i.e. `cp food.md ../eat.md`,if in `..` there is no `eat.md`,then we create a new file and copy `food.md` to it),`rm` to remove .

10. `mkdir` `rmdir`
	`mkdir` to create a directory(remember `My\ photos`).`rmdir` only allows you to remove a directory when it is empty

11. `^L`

12. `< file` `> file` `cat` `>>`
	In the shell, programs have two primary “streams” associated with them: their input stream and their output stream. When the program tries to read input, it reads from the input stream, and when it prints something, it prints to its output stream. Normally, a program’s input and output are both your terminal. That is, your keyboard as input and your screen as output. However, we can also rewire those streams!
	The simplest form of redirection is `< file` and `> file`. These let you rewire the input and output streams of a program to a file respectively:
	```shell
	missing:~$ echo hello > hello.txt
	missing:~$ cat hello.txt
	hello
	missing:~$ cat < hello.txt
	hello
	missing:~$ cat < hello.txt > hello2.txt
	missing:~$ cat hello2.txt
	hello
	```
	Demonstrated in the example above, `cat` is a program that when given file names as arguments, it prints the contents of each of the files in sequence to its output stream. But when `cat` is not given any arguments, it prints contents from its input stream to its output stream (like in the third example above).
	You can also use `>>` to append to a file. 


13. `|`
	Where this kind of input/output redirection really shines is in the use of _pipes_. The `|` operator lets you “chain” programs such that the output of one is the input of another:
	```shell
	missing:~$ ls -l / | tail -n1
	drwxr-xr-x 1 root  root  4096 Jun 20  2019 var
	```
	note that `tail -n1`means print the last n line of the input.


14. `sudo` `sudo su` 
	On most Unix-like systems, one user is special: the “root” user. You may have seen it in the file listings above. The root user is above (almost) all access restrictions, and can create, read, update, and delete any file in the system. You will not usually log into your system as the root user though, since it’s too easy to accidentally break something. Instead, you will be using the `sudo` command. As its name implies, it lets you “do” something “as su” (short for “super user”, or “root”). When you get permission denied errors, it is usually because you need to do something as root. Though make sure you first double-check that you really wanted to do it that way!
	
	One thing you need to be root in order to do is writing to the `sysfs` file system mounted under `/sys`. `sysfs` exposes a number of kernel parameters as files, so that you can easily reconfigure the kernel on the fly without specialized tools. **Note that sysfs does not exist on Windows or macOS.**
	
	For example, the brightness of your laptop’s screen is exposed through a file called `brightness` under
	
	```shell
	/sys/class/backlight
	```
	
	By writing a value into that file, we can change the screen brightness. Your first instinct might be to do something like:
	
	```shell
	$ sudo find -L /sys/class/backlight -maxdepth 2 -name '*brightness*'
	/sys/class/backlight/thinkpad_screen/brightness
	$ cd /sys/class/backlight/thinkpad_screen
	$ sudo echo 3 > brightness
	An error occurred while redirecting file 'brightness'
	open: Permission denied
	```
	
	This error may come as a surprise. After all, we ran the command with `sudo`! This is an important thing to know about the shell. Operations like `|`, `>`, and `<` are done _by the shell_, not by the individual program. `echo` and friends do not “know” about `|`. They just read from their input and write to their output, whatever it may be. In the case above, the _shell_ (which is authenticated just as your user) tries to open the brightness file for writing, before setting that as `sudo echo`’s output, but is prevented from doing so since the shell does not run as root. Using this knowledge, we can work around this:
	
	```shell
	$ echo 3 | sudo tee brightness
	```
	
	Since the `tee` program is the one to open the `/sys` file for writing, and _it_ is running as `root`, the permissions all work out. You can control all sorts of fun and useful things through `/sys`, such as the state of various system LEDs (your path might be different):
	
	```shell
	$ echo 1 | sudo tee /sys/class/leds/input6::scrolllock/brightness
	```

15. `xdg-open` open`
	The first one is on Linux, while second one on macOS.


16. `touch`
	```shell
	touch  ~/food.md
	```
	touch create new files.

17. `chmod`
	```shell
	chmod  u+x  food.file
	chmod  g-w  food.file
	chmod  o=r  food.file
	```

18. `diff`
	```shell
	diff file1.txt file2.txt; echo "退出码: $?"
	```
	
	```shell
	$ diff file1.txt file2.txt
	2c2
	< 
	---
	> 
	5d4
	< 
	7a7
	>
	
	2 means the second line
	c means change
	d means delete
	a means add
	```

18. `convert`
	[[ImageMagick|convert]]

19. `tldr`
	[[tldr]]

20. `ffmpeg`
	[[ffmpeg]]

21. `grep` `grep -R`

22. `rg`
	[[ripgrep|rg]]
	```shell
	rg -u --files-without-match "^#\!" -t sh --stats ~/desktop
	```

23. `broot` `tree`
	```shell
	br
	:cd
	```
24. `nnn`

25. Other information
	[[ShellSupplement|Click here]]