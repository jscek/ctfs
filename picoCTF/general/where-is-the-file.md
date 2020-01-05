# where-is-the-file

## Points:
200

## Description:
> I've used a super secret mind trick to hide this file. Maybe something lies in /problems/where-is-the-file_3_19c1a7766ac2747c446eb9666a9b4fb4.

## Solution:
This one is trivial. Let's list all file in given location

````console
staash@pico-2019-shell1:/problems/where-is-the-file_3_19c1a7766ac2747c446eb9666a9b4fb4$ ls -al
total 80
drwxr-xr-x   2 root       root        4096 Sep 28 22:05 .
drwxr-x--x 684 root       root       69632 Oct 10 18:02 ..
-rw-rw-r--   1 hacksports hacksports    39 Sep 28 22:05 .cant_see_me
````

Yeah, we can see the hidden file. Now when we print its content:
````console
staash@pico-2019-shell1:/problems/where-is-the-file_3_19c1a7766ac2747c446eb9666a9b4fb4$ cat .cant_see_me 
picoCTF{w3ll_that_d1dnt_w0RK_f28cde66}
````
