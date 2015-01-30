# Vim

## Motivation

Having a tool to quickly edit a file from the Unix command line is handy.

For example, we frequently log into remote Unix machines, where IntelliJ is not installed. However, vim generally is. 


## Goals

* Edit a file using vim


## Steps

1. Open a terminal
2. Create a file with some text in it:
```
echo "hi" > file.txt
```
3. Open the file:
```
vim file.txt
```
3. Enter "insert mode" by typing _i_ and hitting enter
4. Delete the "hi" text
5. Enter "command mode" by typing _esc_
6. Exit the file without saving by typing _:q!_ and _enter_
7. Re-open the file
8. Enter insert mode
9. Add some text
10. Save the file by typing _:w_ and _enter_
11. Exit by typing _:q_ and _enter_
12. Search online for "vim cheatsheet"
13. Observe there are many more commands

```