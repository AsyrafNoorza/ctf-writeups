
````markdown
# PicoCTF 2023 - WebDecode
````

**Category:** Web Exploitation  
**Author:** Nana Ama Atombo-Sackey  
**Challenge Link:** [WebDecode](https://play.picoctf.org/practice/challenge/427?category=1&page=1)

---

## Description

> Do you know how to use the web inspector?  
>
> **Hint 1:** Use the web inspector on other files included by the web page.  
> **Hint 2:** The flag may or may not be encoded.

---

## Walkthrough

1. After launching the challenge instance, I opened the webpage in the browser.
2. Using **Inspect Element** (Web Inspector), I checked the source code of different pages.
3. On the `/about` page, I found a suspicious attribute inside the HTML:

   ```html
   <section class="about" notify_true="cGljb0NURnt3ZWJfc3VjYzNzc2Z1bGx5X2QzYzBkZWRfMDdiOTFjNzl9">
     <h1>
       Try inspecting the page!! You might find it there
     </h1>
   </section>
````

4. The value of `notify_true` looked like **Base64**.
5. Decoding it with a Base64 decoder:

   ```
   cGljb0NURnt3ZWJfc3VjYzNzc2Z1bGx5X2QzYzBkZWRfMDdiOTFjNzl9
   ```

   → **picoCTF{web\_succ3ssfully\_d3c0ded\_07b91c79}**

---

## Flag

```
picoCTF{web_succ3ssfully_d3c0ded_07b91c79}
```

---