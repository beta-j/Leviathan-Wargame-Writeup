**Username:** ``leviathan2``

**Password:** ``mEh5PNl10e``


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

So the logical next step is let's see if we can have a look at the stored ssh password for `leviathan3`:
```console
leviathan2@gibson:~$ ./printfile /etc/leviathan_pass/leviathan3
You cant have that file...
```

OK - so it won't be so easy then!
