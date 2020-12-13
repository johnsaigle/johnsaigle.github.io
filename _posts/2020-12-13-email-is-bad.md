layout: post
title: "Email validation is weird and bad: Lessons in XSS"
date: 2020-12-13 00:00:00 -0000
categories: web, xss

**tl;dr** The payload linked here doesn't bypass HTML5 email validation and quite possibly other algorithms as well. It was probably designed to bypass PHP's `filter_var()` function specifically and it would be good to mark it as such.

---

Hi there,
I was up to no good this week and trying to execute a XSS payload against a web form that expects an email address.

I came to this page and picked up the exploit `"><svg/onload=confirm(1)>"@x.y` from this repo: https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/XSS%20Injection#bypass-email-filter. However, this was flagged as an invalid input on both the HTML5 input element and by whatever back-end library this server was using.

Much research later, I have learned about the nightmare of validating email addresses. The result is that the exploit, while RFC compliant as listed in this repo, is not really going to work as advertised.

HTML5 has [its own way of validating email addresses](https://html.spec.whatwg.org/#valid-e-mail-address) based on:
1) Upholding RFCs written subsequent to the one linked here, and
2) Willfully violating those same RFCs

So where did this payload come from?

My hunch is that it came from [brutelogic's blog](https://brutelogic.com.br/blog/xss-limited-input-formats/) where it's listed verbatim. Importantly, his example tests against PHP's `filter_var()` function which indeed has no trouble with the the same payload.

Given that HTML5 is likely to be found out in the wild I think it would be good to amend this entry in the XSS payloads to state that it is designed to be specifically run against PHP servers and will fail in many other cases.
