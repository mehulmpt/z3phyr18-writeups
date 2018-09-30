# Web

## 1. Hmmmmmmmm

This challenge was relatively easier. Going to given URL, you'd apparently see nothing but a pewdiepie image. No cookies, nothing. On web, to hide something, you really just have 3 locations: web page itself, cookies sent and the HTTP request (which basically contains both cookies and web content). Intercept the request with any network tools (like burp suite, or even chrome network debugger) would reveal a header called 'X-Flag' which contained the flag value.

## 2. 46easB

Hint 1: see how base64 is written. It is reversed. All given links were in the format:

`/<base64_string>`

this base64_string was a number in all examples. `MQ==` in base64 is 1, `Mg==` in base64 is 2 and so on.
Number 5, i.e. `/NQ==` showed an interesting result. Instead of outputting 5 on screen, it outputted 'hmm..' which implies that the answers to these calls are somehow hardcoded on backend.

This pattern worked till number 9 (except 5). For number 10, the link given was `/MDE=`, however `MDE=` actually is "01" in base64, hence 46esab.
11 was given as "MTE=" which is okay because 11 reads same both forward and backward.
12 was given as "MjE=" which is actually "21" when decoded.
No further number worked after this point.
No other value worked.

At this time, you have a couple of observations:

1. The values are hardcoded
2. The `/<base64_string>` here is reversed, i.e. to get what you want, you have to not only base64 encode it, but reverse it also.

You had to `base64_encode("flag")`, but it won't work, hence, you had to encode `"galf" => "Z2FsZg=="` and visit `/Z2FsZg==` to get the flag.

## 3. Cookie Treat

Visiting the given URL takes you to a page which just says `{ status: 'ok' }`, not very helpful. But the challenge name suggests there's something going on with cookies. Checking the cookies, we find that there's a `secretFlag` cookie set, however it looks like a garbage value. Refreshing the page changes that value. Hmm..
Try to modify cookie value a bit and refresh the page. It starts all over again. More like this:

```
<YOU LAND ON PAGE>
secretFlag = abc
REFRESH
secretFlag = def
REFRESH
secretFlag = ghi
REFRESH
secretFlag = jkl
<MODIFY secretFlag to say 123>
REFRESH
secretFlag = abc
REFRESH
secretFlag = def
```

Actual cookie values looked garbage, I'm using `abc`, `def`, `ghi` here for simplicity.

So what do we observe here? There's no pattern in cookies but if you modify the cookie server starts all over again. It probably means server is checking if your set cookie is valid and if it is, assigning another cookie in a well defined manner (array?)

You had to create a script to visit this page, grab the new cookie value, set the cookie in the request and visit the page again. It'll then give you another fresh cookie value and keep doing this.

Server actually consisted of the following code:

```js
cookies = ["<random cookie>", "<random cookie>", "...162 more random cookies here", "Z3PHYR{<your flag value>}", "<random cookies>", "more random cookies hre..."]
```

You had to create a script which stopped when it found Z3PHYR keyword, the format of the flag. Flag was stored at index 164, hence you needed 164 refreshes to make secretFlag contain actual flag.

Hope you enjoyed the CTF! If you have any questions regarding any solution, reach out to me at bWVodWxtcHRAZ21haWwuY29t (looks familiar?)
