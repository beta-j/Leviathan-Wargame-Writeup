**Username:** ``leviathan0``

**Password:** ``leviathan0``


#


We start off the challenge by logging in as ``Leviathan0`` over ssh on Port ``2223``:

```console
ssh leviathan0@leviathan.labs.overthewire.org -p 2223
```


At first glance it looks like we're in an empty directory `/home/leviathan0`

```console
leviathan0@gibson:~$ pwd
/home/leviathan0
leviathan0@gibson:~$ ls
leviathan0@gibson:~$
```

However we can take a better look by using `ls` with the `-la` switch to show hidden files and directories:

```console
leviathan0@gibson:~$ ls -la
total 24
drwxr-xr-x  3 root       root       4096 Oct  5  2023 .
drwxr-xr-x 83 root       root       4096 Oct  5  2023 ..
drwxr-x---  2 leviathan1 leviathan0 4096 Oct  5  2023 .backup
-rw-r--r--  1 root       root        220 Jan  6  2022 .bash_logout
-rw-r--r--  1 root       root       3771 Jan  6  2022 .bashrc
-rw-r--r--  1 root       root        807 Jan  6  2022 .profile
```

It looks like our user; `leviathan0` has access to the hidden directory `.backup`.  So we can have a look at it's contents:

leviathan0@gibson:~$ ls -la .backup/
total 140
drwxr-x--- 2 leviathan1 leviathan0   4096 Oct  5  2023 .
drwxr-xr-x 3 root       root         4096 Oct  5  2023 ..
-rw-r----- 1 leviathan1 leviathan0 133259 Oct  5  2023 bookmarks.html

The `.backups` directory contains a dile called `bookmarks.html`.  From the size of the file it looks pretty large so let's have a quick peek at what it contains:

```console
leviathan0@gibson:~$ head -n 20 .backup/bookmarks.html
<!DOCTYPE NETSCAPE-Bookmark-file-1>
<!-- This is an automatically generated file.
     It will be read and overwritten.
     DO NOT EDIT! -->
<META HTTP-EQUIV="Content-Type" CONTENT="text/html; charset=UTF-8">
<TITLE>Bookmarks</TITLE>
<H1 LAST_MODIFIED="1160271046">Bookmarks</H1>

<DL><p>
    <DT><H3 LAST_MODIFIED="1160249304" PERSONAL_TOOLBAR_FOLDER="true" ID="rdf:#$FvPhC3">Bookmarks Toolbar Folder</H3>
<DD>Add bookmarks to this folder to see them displayed on the Bookmarks Toolbar
    <DL><p>
    </DL><p>
    <HR>
    <DT><A HREF="http://www.goshen.edu/art/" ADD_DATE="1133884188" LAST_CHARSET="ISO-8859-1" ID="rdf:#$2wIU71">Art Department</A>
<DT><A HREF="http://www.goshen.edu/art/ed/art-ed-links.html#links" ADD_DATE="1134961650" LAST_CHARSET="ISO-8859-1" ID="64936479">com for Bartel artwork</A>
<DT><A HREF="http://www.goshen.edu/%7Emarvinpb/MB_bio.htm" ADD_DATE="1124894614" LAST_CHARSET="ISO-8859-1" ID="13861712">Bio</A>
<DT><A HREF="http://www.goshen.edu/art/ed/art-ed-links.html#links" ADD_DATE="1131475703" LAST_CHARSET="ISO-8859-1" ID="60650012">Links</A>
<DT><A HREF="http://www.goshen.edu/art/ed/creativitykillers.html" ADD_DATE="1101295712" LAST_CHARSET="ISO-8859-1" ID="63341225">Creativity <b>Killers</b></A>
<DT><A HREF="http://www.bartelart.com/arted/transfer.html" ADD_DATE="1144619369" LAST_CHARSET="ISO-8859-1" ID="90301948">Teaching for <strong>Transfer</strong> of Learning</A>
```

This appears to be an html file containing a list of URLs and the associated text descriptions.  Maybe we can find something related to a password in here?  We can pipe the output to a `grep pass` command which will only display lines that contain the text string `pass`:

```console
leviathan0@gibson:~$ cat .backup/bookmarks.html | grep pass
<DT><A HREF="http://leviathan.labs.overthewire.org/passwordus.html | This will be fixed later, the password for leviathan1 is PPIfmI1qsA" ADD_DATE="1155384634" LAST_CHARSET="ISO-8859-1" ID="rdf:#$2wIU71">password to leviathan1</A>
```

And there it is!  We can see that the password for `leviathan1` is `PPIfmI1qsA`

