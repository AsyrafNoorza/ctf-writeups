```markdown
# picoCTF 2022 - Web Exploitation: heapdump
```

## Challenge Description
> Welcome to the challenge! In this challenge, you will explore a web application and find an endpoint that exposes a file containing a hidden flag.  
>  
> **Hint**:  
> - *Explore backend development with us*  
> - *The head was dumped*  

The site is a simple blog running **picoCTF News**, with an article mentioning **API Documentation**.  
Hidden within the application is a debugging endpoint that exposes a **heap dump** file, containing sensitive information (including the flag).

---

## Step-by-Step Walkthrough

### 1. Explore the site
Opening the challenge site, we see a basic blog interface.  
One article mentions **API Documentation**:

![screenshot-blog-api-docs](images/blog_api_docs.png)

This hints that developer/debug endpoints might exist.

---

### 2. Check for hidden endpoints
Testing common developer paths, we find:

```

http\://<challenge-site>/heapdump

```

This reveals a downloadable file:

```

heapdump-1755618337319.heapsnapshot

````

![screenshot-heapdump-download](images/heapdump_download.png)

---

### 3. Analyze the dump file
We download the file and run `strings` + `grep` to look for the flag:

```bash
strings heapdump-1755618337319.heapsnapshot | grep picoCTF
````

**Output:**

```
picoCTF{Pat!3nt_15_Th3_K3y_13d135dd}
```

![screenshot-grep-output](images/grep_flag.png)

---

## Tools Used

* **Browser** → Explore and find hidden endpoints.
* **strings** → Extract human-readable text from binary files.
* **grep** → Quickly search for the `picoCTF` pattern.

---

## Flag

```
picoCTF{Pat!3nt_15_Th3_K3y_13d135dd}
```

---

## Key Takeaways

* Many frameworks (e.g., Spring Boot, Node.js, etc.) expose **debug endpoints** such as `/heapdump`, `/actuator`, `/debug`.
* These endpoints should **never** be exposed in production as they may leak sensitive data, including **secrets, API keys, or flags**.
* When given a binary dump, tools like `strings` + `grep` are your best friends.

---

## References

* [picoCTF Practice Challenges](https://play.picoctf.org/practice)
* [Heap Dumps in Web Apps](https://owasp.org/) (related to sensitive data exposure)

```