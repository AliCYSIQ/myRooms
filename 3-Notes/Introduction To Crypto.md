[[Crypto]]

ASC II is 7-bit encoding let you representation of text using integers
and you can use `chr()` in python to do this and use `ord()` to do opposite 
_____________
you can use` byte.fromhex()` to convert hex to bytes and use `.hex()` to do the opposite
and you can use this then use `base64` which allows us to represent binary data as an ASCII string using an alphabet of 64 characters by this code `base64.b64encode` and `base64.b64decode`
_____
`from Crypto.Util.number import *` from this library you can use `long_to_bytes()`  or `long_to_bytes()`  like if you number like this 
**1151519506386231889993168548881374739577551628728968263649996528271463725**
you need to use `long_to_bytes()` to convert it to normal strrings 