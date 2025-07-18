[[pwnlab.kr]] [[Week4]]

the first lab in this , and this is the description 
```
Mommy! what is a file descriptor in Linux?

* try to play the wargame your self but if you are ABSOLUTE beginner, follow this tutorial link:
https://youtu.be/971eZhMHQQw

ssh fd@pwnable.kr -p2222 (pw:guest)
```

(**I watch the video(walk throw**)

when you enter you will find some files , 
the first one is `flag` and others like `fd` and `fd.c` 
and if we use `ls -la` we will see this `-r-xr-sr-x 1 root fd_pwn 15148 Mar 26 13:17 fd`
and `-rw-r--r-- 1 root root 452 Mar 26 13:17 fd.c` , **if you notice** that there's `s` and that mean set id and way , if we cat fd.c we will see this
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>

char buf[32];

int main(int argc, char* argv[], char* envp[]) {
    if (argc < 2) {
        printf("pass argv[1] a number\n");
        return 0;
    }

    int fd = atoi(argv[1]) - 0x1234;
    int len = read(fd, buf, 32);

    if (!strcmp("LETMEWIN\n", buf)) {
        printf("good job :)\n");
        setregid(getegid(), getegid());
        system("/bin/cat flag");
        exit(0);
    }

    printf("learn about Linux file IO\n");
    return 0;
}

```

in summary : this code take arg then convert this arg to integer
after that it will made subtraction by  `0x1234` which is a hex value and it equal `4660` 
then use `read` that try to read but if it zero(the result from subtraction ) it will hang until we enter some value and here we can enter `LETMEWIN` and after enter this it will compere with "LETMEWIN" and this will be true and the `flag` file will be cat .