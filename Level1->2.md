**Username:** ``leviathan1``

**Password:** ``PPIfmI1qsA``


#


We start off the challenge by logging in as ``Leviathan1`` over ssh on Port ``2223``:

```console
ssh leviathan1@leviathan.labs.overthewire.org -p 2223
```


This time we have a file named `check` in our home directory which appears to be an executable:

```console
leviathan1@gibson:~$ ls -la
total 36
drwxr-xr-x  2 root       root        4096 Oct  5  2023 .
drwxr-xr-x 83 root       root        4096 Oct  5  2023 ..
-rw-r--r--  1 root       root         220 Jan  6  2022 .bash_logout
-rw-r--r--  1 root       root        3771 Jan  6  2022 .bashrc
-r-sr-x---  1 leviathan2 leviathan1 15072 Oct  5  2023 check
-rw-r--r--  1 root       root         807 Jan  6  2022 .profile
leviathan1@gibson:~$ file check
check: setuid ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, BuildID[sha1]=c7acb418cff514a706855be5cb59e985ca67b6d3, for GNU/Linux 3.2.0, not stripped
```

If we try to run the executable we get asked for a password and are then kicked out:
```console
leviathan1@gibson:~$ ./check
password:
test
Wrong password, Good Bye ...
```

In such situations it's usually a good idea to check for any human-readable strings in the executable by using the `strings` command:

```console
leviathan1@gibson:~$ strings check
```

This gives some interesting strings but nothing that appears to be a password.  One interesting cadidate is the word `love`, but if we try to enter this as the password it is rejected.

Next thing we can try is to use `ltrace` to determine the library calls that the executable is making:

```console
leviathan1@gibson:~$ ltrace ./check
__libc_start_main(0x80491e6, 1, 0xffffd6a4, 0 <unfinished ...>
printf("password: ")                                                                                                             = 10
getchar(0xf7fbe4a0, 0xf7fd6f90, 0x786573, 0x646f67password: test
)                                                                              = 116
getchar(0xf7fbe4a0, 0xf7fd6f74, 0x786573, 0x646f67)                                                                              = 101
getchar(0xf7fbe4a0, 0xf7fd6574, 0x786573, 0x646f67)                                                                              = 115
strcmp("tes", "sex")                                                                                                             = 1
puts("Wrong password, Good Bye ..."Wrong password, Good Bye ...
)                                                                                             = 29
+++ exited (status 0) +++
```

We try entering a dummy password of `test` and we can see that the executable is running a String Compare (`strcmp`) of the first three letters of the password we entered to the string `sex`, before giving us a "Wrong Password" output.  This means that the program is expecting us to enter `sex` as the password:

```console
leviathan1@gibson:~$ ./check
password: sex
$ whoami
leviathan2
```

And there it is...by entering the password `sex` we are now logged in as `leviathan2` and we can now retrieve the ssh password from `/etc/leviathan_pass/leviathan2`:
```console
$ cat /etc/leviathan_pass/leviathan2
mEh5PNl10e
```



