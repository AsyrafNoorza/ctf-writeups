# Cookie Monster Secret Recipe — Write-up (picoCTF)

**Category:** Web Exploitation
**Difficulty:** Intro
**Challenge link:** [https://play.picoctf.org/practice/challenge/469?page=1](https://play.picoctf.org/practice/challenge/469?page=1)  
**Goal:** Find the hidden “secret recipe” (flag) on the site.

https://play.picoctf.org/practice/challenge/469?page=1
---

## Overview

The challenge website sets a cookie named `secret_recipe`. Its value is **URL-encoded Base64**.
Decoding (in the correct order) reveals the flag.

---

## Steps

1. **Launch the instance** from the challenge page.

2. **Open Developer Tools** → **Application/Storage** → **Cookies** and select the challenge domain.

3. **Find the cookie**:

   ```
   Name:  secret_recipe
   Value: cGljb0NURntjMDBrMWVfbTBuc3Rlcl9sMHZlc19jMDBraWVzXzc3MUQ1RUIwfQ%3D%3D
   ```

   The `%3D%3D` is `==` URL-encoded — a common Base64 padding hint.

4. **Decode it (URL-decode → Base64-decode)**

   **Browser console (JS):**

   ```js
   const raw = "cGljb0NURntjMDBrMWVfbTBuc3Rlcl9sMHZlc19jMDBraWVzXzc3MUQ1RUIwfQ%3D%3D";
   const urlDecoded = decodeURIComponent(raw);
   const base64Decoded = atob(urlDecoded);
   console.log(base64Decoded);
   // picoCTF{c00k1e_m0nster_l0ves_c00kies_771D5EB0}
   ```

   **Command line (Python):**

   ```bash
   python3 - << 'PY'
   import urllib.parse, base64
   s = "cGljb0NURntjMDBrMWVfbTBuc3Rlcl9sMHZlc19jMDBraWVzXzc3MUQ1RUIwfQ%3D%3D"
   print(base64.b64decode(urllib.parse.unquote(s)).decode())
   PY
   ```

---

## Why it works

* Web apps frequently **URL-encode** cookie values for safe transport.
* The value has Base64 characteristics (length, alphanumerics, `==` padding).
* Decoding **must** be done in order: **URL-decode first**, then **Base64-decode**.

---

## Flag

```
picoCTF{c00k1e_m0nster_l0ves_c00kies_771D5EB0}
```

---

## Verification Checklist

* [x] Located `secret_recipe` cookie in DevTools.
* [x] URL-decoded value (`%3D%3D` → `==`).
* [x] Base64-decoded result to plaintext flag.

---

## Notes / Tips

* Seeing `%3D` inside a suspicious string? Consider “URL-encoded Base64.”
* If using CLI `base64 -d`, make sure to URL-decode first or decoding fails.
* Prefer local decoding over online tools for privacy.
