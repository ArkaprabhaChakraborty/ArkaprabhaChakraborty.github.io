+++
showdate = true
title = "The OWASP Top 10 (Theory notes made by googling and reading different articles)"
date = 2023-02-01
url = "/blog/owasp_top_10"
+++

# [What is OWASP Top 10?](https://owasp.org/www-project-top-ten/)
Well it's simply a list of top 10 web application security vulnerabilities. The link to the actual source material has been given already, so you can read more about it there. The list was last updated in 2021. Many security researchers including the very best in OWASP Foundation believe it's still relevant in that order, even tho I kinda disagree :P.

Anyway, let's get started with the list. I'll be going through "What's What" and "How to fix it" for each of the top 10 vulnerabilities. If you wanna jump to the more technical stuffs, check [The Official OWASP Top 10:2021 Guide](https://owasp.org/Top10/).

# [A01:2021-Broken Access Control](https://owasp.org/Top10/A01_2021-Broken_Access_Control/)
In simple terms this happens when "confidential" information/data is viewed or can be accessed by a user who shouldn't have access to it. Access control is making sure people have {{< colour "#FFBF00" "ACCESS">}} to {{< colour "#00FF00" "what they're supposed to">}} and {{< colour "#FF0000" "DO NOT">}}have access to {{< colour "#FF0000" "WHAT THEY ARE NOT">}}. Broken Access Control is the evil alter-ego of this.

What happens if people have access to stuffs they aren't supposed to? Well then :). It's a GOOD way to {{< colour "#FF0000" "BREACH">}} the entire [CIA triad](/blog/cia_triad) of your application -> system -> organization.

Now that we know what it is, how do we fix it? It is actually simple:
- Use {{<colour "red" "strict">}} degree of access to make sure people have access to what they're supposed to.
- Use ACL (Access Control List) to make sure people have access to what they're supposed to.
- Enabling RBAC (Role Based Access Control) is a good way to quickly implement access control by grouping users into roles and defining the permissions associated with each role.
- Implement control mechanisms on CORS (Cross Origin Resource Sharing) to make sure your server doesn't allow access to resources from other untrusted domains easily.
- Test your access control mechanisms thoroughly and manually.
- {{<colour "red" "Deny access by default">}} if a resource isn't meant to be shared with the general public.
- Use {{<colour "yellow" "Logging and Monitoring">}} for all activities.

# [A02:2021-Cryptographic Failure](https://owasp.org/Top10/A02_2021-Cryptographic_Failures/)
In simple terms, this happens when we don't use cryptography properly, or the cryptographic implementation is weak, and a logic flaw has been found in it. This is quite common if let's say a pseudo-random number generator is used to generate a key for encryption. This is a very common mistake, and it's a good idea to use a cryptographically secure pseudo-random number generator (CSPRNG) instead.

But the above said reason is not the only reason why this happens. The following situations are also considered as Cryptographic Failures:
- Using weak encryption algorithms.
- Transmitting sensitive data in clear text, aka, missing encryption.
- Using weak key lengths, or default or re-used crypto keys.
- Use of weak hashing algorithms, like MD5.
- Presence of sensitive data in source control.
- Use of hard-coded passwords in configuration files :).
- Easily exploitable cryptographic error messages.
- Caching of sensitive data in memory.

How do we fix it? It's simple:
- Use {{<colour "red" "strong">}} encryption algorithms like AES, SHA256, RSA cryptosystem etc.
- Encrypt data both in transit and at rest.
- Keep a catalogue of all sensitive data.
- {{<colour "red" "Never">}} use hard-coded passwords in configuration files.
- {{<colour "red" "Never">}} use default or re-used crypto keys.
- {{<colour "red" "DISCARD">}} all unused sensitive data from memory as soon as possible.
- Use authenticated encryption to prevent tampering of data, instead of plain or simple encryption.
- Enforce key rotation policy, to prevent the usage of same keys again and again. Automatic secure key generation and rotation is a good way to do this.
- Use updated and secure libraries for cryptographic operations.

# [A03:2021-Injection](https://owasp.org/Top10/A03_2021-Injection/)
In simple terms, this happens when an attacker is able to inject malicious code into an application. This can be done by exploiting a vulnerability in the application's code, or by exploiting a vulnerability in the application's dependencies. This class of vulnerabilities include the infamous Cross-Site Scripting (XSS), SQL Injection (SQLi) and Command Injection (CMDi).

When does it happen?
- An application uses {{<colour "red" "untrusted data">}} in a {{<colour "red" "query">}} without {{<colour "red" "proper validation">}}.
- An application uses {{<colour "red" "untrusted user input">}} in a {{<colour "red" "command">}} without {{<colour "red" "proper validation">}}.
- No proper input validation is done on {{<colour "red" "user input">}}.

How do we fix it? 
- Use {{<colour "red" "proper validation">}} on all user input.
- Use {{<colour "red" "parameterized queries">}} instead of {{<colour "red" "string concatenation">}}.
- Proper input sanitization is a must.
- Proactively test the application with automated DAST tools during development phase and during deployment phase to fix any vulnerabilities found.


# [A04:2021-Insecure Design](https://owasp.org/Top10/A04_2021-Insecure_Design/)
In simple terms, this happens when the application is designed in a way that makes it vulnerable to attacks.