
# Lab Name: Who are you?  
**Category:** Web Exploitation, picoCTF 2021 | **Difficulty:** Medium

---

## Description

```
Let me in. Let me iiiiiiinnnnnnnnnnnnnnnnnnnn
```

---

## Initial Observation

When I first loaded it up I saw:

```
<screenshot>
```

Since it's regarding the browser, it was likely something to do with the **HTTP request headers**.  
I used **Burp Suite Proxy** to capture the request, and **Repeater** to test various modifications.

---

## Exploitation Steps

### Step 1 – Change the `User-Agent` Header

```
User-Agent: picobrowser
```

**Response:**
```
"I don't trust users from another site."
```

---

### Step 2 – Add `Referer` Header

```
User-Agent: picobrowser
Referer: mercury.picoctf.net:1270
```

_(The Referer value was taken from the host URL in the browser’s Inspect > Network tab)_

**Response:**
```
"This site only works in 2018"
```

---

### Step 3 – Add `Date` Header

```
User-Agent: picobrowser
Referer: mercury.picoctf.net:1270
Date: 2018
```

**Response:**
```
"I don't trust users who can be tracked"
```

---

### Step 4 – Add `DNT` (Do Not Track) Header

```
User-Agent: picobrowser
Referer: mercury.picoctf.net:1270
Date: 2018
DNT: 1
```

**Response:**
```
"This website is for people from Sweden"
```

---

### Step 5 – Spoof a Swedish IP with `X-Forwarded-For`

I looked up an IP address assigned to Sweden (e.g., `102.177.146.0`) and added:

```
User-Agent: picobrowser
Referer: mercury.picoctf.net:1270
Date: 2018
DNT: 1
X-Forwarded-For: 102.177.146.0
```

**Response:**
```
"You're from Sweden but don't speak Swedish?"
```

---

### Step 6 – Add Swedish Language Header

```
User-Agent: picobrowser
Referer: mercury.picoctf.net:1270
Date: 2018
DNT: 1
X-Forwarded-For: 102.177.146.0
Accept-Language: sv
```

**Response:**
```
(Flag returned)
```

---

## Reflection

This challenge required me to research various HTTP headers I hadn’t used before. I referred to the [MDN Web Docs](https://developer.mozilla.org/) extensively to understand the purpose and expected values of each header.

The process was a great hands-on learning experience in how websites can use multiple headers to inspect client metadata.
