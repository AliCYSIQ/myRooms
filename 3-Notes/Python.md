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


# learn 
## strings
you can let every first char from your message is upper case by `message.title()`
and can let every chr is upper case by `message.upper()` or lower `message.lower`

`\n` mean new line
`\t` mean tab, and it space if there's no space and start of line if it with `\n`

`.strip` it function use to remove whitespace from both side of a word for exampl `' python '` will be `'python'` after using it , and there's **`.lstrip`** for left only and **`.rstrip`** for right only 

`.removeprefix()` which will remove anything you want **e.g. `https://example.com`** you use it like this `url.removeprefix('https//')` and it will be removed and be **`example.com`** but only if it start with it , if it end it with it like `file.txt` then will need `removesuffix('.txt')` which will remove it 

## list

### introduction 
you can introduce this by [] like `list = ['anything', 1, 'ali']` and as you say it can have **strings** and **integers** and it start from zero until ...  
you can print it by the index of what you want like `print(list[0])` which will print `anything` 
you can access last one in list by `print(list[-1])` which will write `ali` and you continue like this , `-2` will be the one before last one and `-3` is the third but from end and so on 

### changing and adding
you can change element in list in this way `list[0] = 'nothing'` 
and to add  a completely new one in the end , do this `list.append('tah')`
and if you want  to add a new one but in specific position , do this `list.insert(0,'woow')` now it will be in the first and you can control the index 
This operation shifts every other value in the list one position to the right.
###  removing
you can remove any element by using this `del list[0]` 
and you remove the last one by this `list.pop()` which will remove the last one
and you can use `pop()` to remove any element in any position`list.pop(2)` it will remove the   third element    
the different here between `del` and `pop()` is the same  in removing but `del` will remove an element completely and `pop()` can be sigh  the removed element to a variable like this  `removeELEMENT = list.pop()` that's right it will remove it from  the list but it will sign to the variable 
Sometimes you won’t know the position of the value you want to remove
from a list. If you only know the value of the item you want to remove, you
can use `list.remove('ali')`
#### list.remove()
it use to remove element by its value not its position/index

### sorting

#### list.sort()
this function will sort the list alphabetically (from a to z)
and it can be reversed like this `list.sort(reverse=True)` (from z to a)
for both cases the order of the list is permanently changed

#### list.sorted()
this use to sort (from a to z)  
and it can be reversed like this `list.sort(reverse=True)` (from z to a) 
but its change will be temporary 

#### list.reverse()
which reverse the original order (it not like z to a , no , it just reverse it order)
before: \['bmw', 'audi', 'toyota', 'subaru']
after: \['subaru', 'toyota', 'audi', 'bmw']

The reverse() method changes the order of a list permanently

#### len(list)
let you find the length of anything , and if it it use on list as now it will give how many element in it and if it use on normal string will find its length(how many letters in the word)



#### min(list)
to get the smallest number 
#### max(list) 
to get the biggest number  
#### sum(list) 
to get the the sum of all numbers in the list 

#### Comprehensions

it let or it mean that you could use `for` loop inside list like this :
`list = [i for i in range(0,100,2)]` instead of writing a more than 3 lines you can do it in one

#### Slicing
which let you slice  a list like this `print(list[0:3])` it will start from start until 2 like this `0,1,2`
and if you don't write a number it will start from start or end with the last one like this
`print(list[:])` this will start from first one and end with the last one 
don't forgot negative numbers like this `[-3:]` which will start from the third one from the end and end with the last one 

can use it to make full copy of list  like this `new_list = list[:]` which will copy every element 


## for


just the normal things i already knoow and this is the syntax
```Python
for i in list:
	print(i)
```

#### range()
it function use to make range between two numbers for example 
```Python
for i in range(1,100):
	#do something
```

here it will go from 1 until 99 (last number is excluded)
and can use variable in it like 
```Python
for i in range(len(list)):
	#do something
```

##### **\*here we use `len()` to get the number of how many element in the list***
now it will go from the first element until the last one

`range()` can use anywhere not only in `for` loop like: `list = range(0,100)`
it will be like this `list = [0,1,2,3,4,...,99]`and as we say the last number is **excluded** in range

`list = range(1,10,2)` here it will start with 1 and end in 9 but it will go 2 by 2 like this:
`list = [1,3,5,7,9]`


## Tubles

**Defining a Tuple**
A tuple looks just like a list, except you use parentheses instead of square
brackets. Once you define a tuple, you can access individual elements by
using each item’s index, just as you would for a list.
it use as the list use but the different you can't change the elements in `tubles` 
this is the only different 


## Dictionaries

Python’s dictionaries, which allow you to connect pieces of related information. You’ll learn how to access the information once it’s in a dictionary and how to modify that information. Because dictionaries can store an almost limitless amount of information
#### Working with Dictionaries
A dictionary in Python is a collection of key-value pairs. Each key is connected to
a value, and you can use a key to access the value associated with that key. A
key’s value can be a number, a string, a list, or even another dictionary. In fact,
you can use any object that you can create in Python as a value in a dictionary.
In Python, a dictionary is wrapped in braces (`{}`) with a series of key-value
pairs inside the braces, for example :

`ahmed = {'name': 'ahmed ali','age':19,'country':'iraq'}`

you can add or edit by this : 
edit: `ahmed['name']= 'ahmed ali thaseen`
add: `ahmed['have_sisters']=true`

`new_age = ahmed['age']+ 5`
this mean `19+5` 

you can use `del` to remove key and its value 

#### get()
it use to get a value of key and if this doesn't exists it will show a message
`ahmed.get('aga','there's nothing from what you say)` 
the first `arg` is **the key**
the second `arg` is `the message`


