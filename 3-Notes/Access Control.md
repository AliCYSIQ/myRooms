[[Bug]]

## What is access control?

**Access control** is the application of constraints on who or what is authorized to perform actions or access resources. In the context of web applications, access control is dependent on authentication and session management:

- **Authentication** confirms that the user is who they say they are.
- **Session management** identifies which subsequent HTTP requests are being made by that same user.
- **Access control** determines whether the user is allowed to carry out the action that they are attempting to perform.

### Vertical access controls

Vertical access controls are mechanisms that restrict access to sensitive functionality to specific types of users.
### Horizontal access controls

Horizontal access controls are mechanisms that restrict access to resources to specific users.

### Context-dependent access controls

Context-dependent access controls restrict access to functionality and resources based upon the state of the application or the user's interaction with it.
Context-dependent access controls prevent a user performing actions in the wrong order. For example, a retail website might prevent users from modifying the contents of their shopping cart after they have made payment.

----

### Vertical privilege escalation

If a user can gain access to functionality that they are not permitted to access then this is vertical privilege escalation. For example, if a non-administrative user can gain access to an admin page where they can delete user accounts, then this is vertical privilege escalation.

#### changing URL

you can change the url it self to go to  a page you should not get it , like `website.com/admin`
sometimes the link be unguessable  

#### Parameter-based access control methods

Some applications determine the user's access rights or role at login, and then store this information in a user-controllable location. This could be:

- A hidden field.
- A cookie.
- A preset query string parameter.

most of it just focus on the response or cookie 
#### Broken access control resulting from platform misconfiguration
some website might configure rules like this one:

`DENY: POST, /admin/deleteUser, managers`

This rule denies access to the `POST` method on the URL `/admin/deleteUser`, for users in the managers group. Various things can go wrong in this situation, leading to access control bypasses.
it will deny it if `/admin/deleteUser` in the URL  but some of them let you do this 
```
POST / HTTP/1.1 
X-Original-URL: /admin/deleteUser
```
there's no `admin` in the URL so it will passed but the header here will let you perform `/admin/deleteUser`

you need to see [portswigger access control](https://portswigger.net/web-security/access-control)



### Horizontal privilege escalation
Horizontal privilege escalation occurs if a user is able to gain access to resources belonging to another user, instead of their own resources of that type. For example, if an employee can access the records of other employees as well as their own, then this is horizontal privilege escalation.

sometimes every user have its unique `id` and if this id was easy to guess like `?id=1` then this is a bug 
other times it be hard to guess like (GU-IDs) This may prevent an attacker from guessing or predicting another user's identifier. However, the GU-IDs belonging to other users might be disclosed elsewhere in the application where users are referenced, such as user messages or reviews.

in some application even if you have this `id` the application detect you here you need to see the response it will have the cookies of the users and you can use it to log-in 

### Access control vulnerabilities in multi-step processes

Many websites implement important functions over a series of steps. This is common when:

- A variety of inputs or options need to be captured.
- The user needs to review and confirm details before the action is performed.


Sometimes, a website will implement rigorous access controls over some of these steps, but ignore others. Imagine a website where access controls are correctly applied to the first and second steps, but not to the third step. The website assumes that a user will only reach step 3 if they have already completed the first steps, which are properly controlled. An attacker can gain unauthorized access to the function by skipping the first two steps and directly submitting the request for the third step with the required parameters.