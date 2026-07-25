## `WRITEUP.md` template (copy this into a file named WRITEUP.md)

```markdown
# Assignment 4 Write-up — <your name>

## V1 — SQL Injection

- Exploit (login bypass): <payload> — why it worked in one sentence.
- Exploit (UNION exfil): <payload> — what data you recovered.
- Fix: <what you changed in /login and /search> (mention bcrypt only if you did the bonus).

V1 – SQL Injection
Exploit (login bypass):
The login bypass payload was quartermaster' -- . It worked because the SQL query concatenated user input directly into the statement, allowing the password check to be commented out.
Exploit (UNION exfil):
The payload used in sqli.sh performed a UNION SELECT to retrieve usernames and passwords from the users table through the search results. This demonstrated that sensitive information could be extracted from the database. (Use the exact payload from your sqli.sh file if your professor wants it shown.)
Fix:
I changed both the /login and /search routes to use prepared statements with parameterized queries instead of concatenating user input into SQL strings. This prevents SQL injection attacks because user input is treated as data rather than executable SQL.

## V2 — Stored XSS

- Exploit: <payload> — where it was stored, who it affected.
- Fix: <encoding change> + <the CSP header you added> + <HttpOnly>.
- One sentence: why CSP helps even if you miss an escaping bug.

Exploit:
The payload was posted as a comment on an item, where it was stored in the database. When another user viewed the item page, the browser executed the injected JavaScript, allowing an attacker to steal the victim's session cookie.
Fix:
I escaped user-generated content before rendering it, added a Content Security Policy (CSP), and set the session cookie to HttpOnly. These changes prevent injected JavaScript from executing or accessing the session cookie.
Why CSP helps:
Even if an escaping mistake is missed in the future, the Content Security Policy provides an additional layer of defense by restricting which scripts the browser is allowed to execute.

## V3 — CSRF

- Exploit: <how csrf-poc.html works> — why the cookie was enough.
- Fix: <the token mechanism> + <SameSite change>.
- One sentence: why the attacker cannot forge a valid token.

Exploit:
The csrf-poc.html page automatically submitted a hidden form to /wallet/transfer. Because the browser automatically included the user's session cookie, the forged request was accepted without the victim's knowledge.
Fix:
I generated a random CSRF token when the session was created, stored it in the session, added it as a hidden field in the transfer form, and verified it on every transfer request. I also changed the session cookie to SameSite: "strict" so it is not sent with cross-site requests.
Why the attacker cannot forge a valid token:
The attacker cannot read or guess the server-generated CSRF token, so they cannot include the correct value in a forged request.

## Time spent

<hours> — anything that took longer than expected?

3 hours and 15 minutes. The CSRF implementation took the longest because I needed to generate the token, include it in the form, validate it on the server, and verify that all three vulnerabilities passed the provided verification script.
```
