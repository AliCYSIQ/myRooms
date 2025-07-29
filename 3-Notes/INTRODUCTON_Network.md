[[Network]]


**TCP/IP Stack & OSI Model**

This understanding will ensure we grasp how networking traffic and the host applications interact.


the different between **TCP** and **UDP**
both of them are protocol to connect two hosts eaisly but **TCP** is more reliable but slower than **UDP** and **UDP** is faster that **TCP** but it's notreliable 

#### TCP VS. UDP

|**Characteristic**|**TCP**|**UDP**|
|---|---|---|
|`Transmission`|Connection-oriented|Connectionless. Fire and forget.|
|`Connection Establishment`|TCP uses a three-way handshake to ensure that a connection is established.|UDP does not ensure the destination is listening.|
|`Data Delivery`|Stream-based conversations|packet by packet, the source does not care if the destination is active|
|`Receipt of data`|Sequence and Acknowledgement numbers are utilized to account for data.|UDP does not care.|
|`Speed`|TCP has more overhead and is slower because of its built-in functions.|UDP is fast but unreliable.|