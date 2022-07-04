# Authentication vulnerabilities

https://portswigger.net/web-security/authentication

## Lab: Username enumeration via different responses

漏洞：无失败次数限制；用户名错误提示`Invalid username`，用户名正确、密码错误提示`Incorrect password`。

操作：使用Burp Intruder(Sniper)先枚举出有效的用户名，再爆破其密码。

## Lab: Username enumeration via subtly different responses

漏洞：开发者尝试解决上一个漏洞的问题，将错误提示统一改成`Invalid username or password.`，但其中一处拼写错误了，本质上还是跟上一个漏洞一样。

操作：同上

## Lab: Username enumeration via response timing

漏洞：用户名正确时，密码越长响应越慢，用户名错误时无影响。另外这一题为防止爆破，增加了IP限制，但可以通过`X-Forwarded-For`伪造IP

操作：使用Burp Intruder(Pitchfork)先枚举出有效的用户名（根据响应时间），再爆破其密码。

## Lab: Broken brute-force protection, IP block

漏洞：连续登录失败3次会临时锁定IP，登录成功会清除失败次数。

操作：将正确的用户名/密码插在要爆破的用户名/密码中间，然后使用Burp Intruder(Pitchfork)爆破。

## Lab: Username enumeration via account lock

漏洞：正确的用户名失败次数过多会限制登录，提示`You have made too many incorrect login attempts`，错误的用户名只是提示登录失败。

操作：使用Burp Intruder(Cluster bomb)枚举出有效的用户名，再爆破其密码。

## Lab: Broken brute-force protection, multiple credentials per request

https://portswigger.net/web-security/authentication/password-based/lab-broken-brute-force-protection-multiple-credentials-per-request

漏洞：看不懂这一题的逻辑

操作：将JSON报文的password改成数组

## Lab: 2FA simple bypass

漏洞：第2步邮箱验证是个摆设，不验证也不影响登录。

操作：完成第1步登录后，直接访问`/my-account`

## Lab: 2FA broken logic

漏洞：第2步未验证第1步的状态，即可跳过第1步直接进行第2步，并爆破验证码。

## Lab: 2FA bypass using a brute-force attack

漏洞：无失败次数限制

操作：使用宏完成登录的第1步，然后爆破第2步的验证码。

## Lab: Brute-forcing a stay-logged-in cookie

漏洞：可以推测出cookie格式，使用cookie登录失败无限制

操作：使用Burp Intruder和Payload processing爆破
