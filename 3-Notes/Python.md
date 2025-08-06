[[Programming]]

# **Introduction**

you can use `"".join` to join character together
and can use `chr()` to convert number to text **using ASC II**
and can use `ord()` to do the opposite thing

## socket
the first thing you should do is making object , 
`obj = socket.socket(arg1,arg2)` for example you can use this socket.AF_INET(this use for ipv4) for arg 1 and socket.SOCK_STREAM for TCP connection 
then connect this , with `connect()` and it take one arg so you need to put another () if there's 2 arg and need to put them as one
and can use `socket.SOCK_DGRAM` fro UDP connection and in UDP you do not need `connect()` because UDP is less-connection(it do not do connect to the target) it send the data with `sendto(b"",(ip,port))` then you can use this 
`data, addr = recvform(4096)`  in normal it receve data and address in this way `(b"somedata"),(192.168.0.1,9994)` but if we use `data,addr` way data will receve `b""` and addr will receve `(192.168.0.1,9994)` then in the end of both of those (UDP , TCP ) use `print(data.decode())` to write the data then close it by `client.close()`