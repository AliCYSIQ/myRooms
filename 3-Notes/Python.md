[[Programming]]

# Black hat two(book)
## **Introduction**

you can use `"".join` to join character together
and can use `chr()` to convert number to text **using ASC II**
and can use `ord()` to do the opposite thing

### socket
the first thing you should do is making object , 
`obj = socket.socket(arg1,arg2)` for example you can use this socket.AF_INET(this use for ipv4) for arg 1 and socket.SOCK_STREAM for TCP connection 
then connect this , with `connect()` and it take one arg so you need to put another () if there's 2 arg and need to put them as one
and can use `socket.SOCK_DGRAM` fro UDP connection and in UDP you do not need `connect()` because UDP is less-connection(it do not do connect to the target) it send the data with `sendto(b"",(ip,port))` then you can use this 
`data, addr = recvform(4096)`  in normal it receve data and address in this way `(b"somedata"),(192.168.0.1,9994)` but if we use `data,addr` way data will receve `b""` and addr will receve `(192.168.0.1,9994)` then in the end of both of those (UDP , TCP ) use `print(data.decode())` to write the data then close it by `client.close()`

# leetcode

**i will write only the new things or what i always forget it**
## EASY

### TWO-SUM
to solve this you need to use indeces and for that you caan use this:
`for i in (len(nums))` 
and youu need to use instaaed **for loop** 

like this:
```python
for i in (len(nums)):
	for j in range(i+1,len(nums))
```

**range(first, n)** here start from **first** and end with **n-1** which mean if **first = 0** and **n = 5** this is mean it will be like this **\[0,1,2,3,4]**
### ROMAN-TO-INTEGER
we use dict and this is the sol
```python
class Solution(object):
    def romanToInt(self, s):
        d = {'I':1, 'V':5, 'X':10, 'L':50, 'C':100, 'D':500, 'M':1000}
        total = 0 
        i = 0
        while i < len(s):
            # if there's a next character and current < next -> subtractive pair
            if i + 1 < len(s) and d[s[i]] < d[s[i+1]]:
                total += d[s[i+1]] - d[s[i]]
                i += 2      # skip the next character because we already used it
            else:
                total += d[s[i]]
                i += 1
        return total
```


# wow
## strings
you can let every first char from your message is upper case by `message.title()`
and can let every chr is upper case by `message.upper()` or lower `message.lower`

`\n` mean new line
`\t` mean tab

`.strip` it function use to remove whitespace from both side of a word for exampl `' python '` will be `'python'` after using it , and there's **`.lstrip`** for left only and **`.rstrip`** for right only 

`.removeprefix()` which will remove anything you want **e.g. `https://example.com`** you use it like this `url.removeprefix('https//')` and it will be removed and be **`example.com`**
