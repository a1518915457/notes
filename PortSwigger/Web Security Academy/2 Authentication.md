# Authentication vulnerabilities

https://portswigger.net/web-security/authentication

## Lab: Username enumeration via different responses

漏洞：用户名错误提示`Invalid username`，用户名正确、密码错误提示`Incorrect password`

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
