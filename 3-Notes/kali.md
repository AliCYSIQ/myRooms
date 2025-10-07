[[Linux]]

source :[tlcl book](https://linuxcommand.org/tlcl.php)

command `cp` use with it `-v` to make it verbose and `-i` to make it interactive

`ln` command make links to file , for example `ln` alone will make hard link to any file, if you want to make sure that two files are hard links or no , is by using `ls -li` which will show the inode  and if they were hard links they will share the same inode number

hard link only make new names but the original file or name and the hard links are all of them refer to the same data because of that they will share the same inode but it have two disadvantages :
- hard links cant refer to directory 
- hard links cant refer to file that exist on different storage device 
