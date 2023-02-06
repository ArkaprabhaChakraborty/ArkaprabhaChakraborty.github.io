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