---
{"dg-publish":true,"permalink":"/sec-250/assignments/gruyere-class-activity/sec-250-gruyere-class-activity/","dg-note-properties":{}}
---

# Google Gruyere Class Activity

#### Notes
Instance ID: 578949796762543157915982823272021788477
I created two users
- User 1:
	- username: asdf
	- password: 1234
- User 2:
	- username: qwer
	- password: 1234

**Objective:**

You're going to spend this activity attacking a website on purpose. Google built Gruyere specifically to be full of security holes, and your job is to find them, exploit them, and figure out how you'd fix them if they were real. By the end, you'll have hands-on experience with the same kinds of bugs that show up in actual data breaches.

* ***Heads up:** Only attack the Gruyere instance you start for this activity\! Don't try any of this on a real site, app, or account that isn't yours. That's illegal, even if it seems harmless.*

**Instructions:**

**Key terms**

A few words show up over and over below. If you blank on what one means, just come back to this list.

| Session cookie | A small piece of data a website hands your browser after you log in, so you're not typing your password on every single page. If someone gets an exact copy of that cookie, they can pretend to be you. |
| :---- | :---- |
| **XSS (Cross-Site Scripting)** | Getting a website to run a script someone else wrote, inside a completely different person's browser. |
| **CSRF (Cross-Site Request Forgery)** | Tricking a logged-in user's browser into doing something on a website that they never meant to do. |
| **XSSI (Cross-Site Script Inclusion)** | A website hands out private data in a format that another site can read. |
| **Source code** | The programming behind a website. This is not what shows up when you view a page, but the instructions that build it. |
| **Black-box vs. white-box** | Black-box means you're only clicking around like a regular visitor. White-box means you're allowed to read the site's source code to hunt for the bug. |
| **Template engine** | How a website builds a page. |
| **Privilege / admin** | How much access an account has. An "admin" account can usually do a lot more than a regular one. |
| **Path / directory traversal** | Getting a website to hand over a file it was never supposed to give you, usually by messing with the file path (think ../). |

# **Part 1: Open Gruyere**

1. In a web browser, navigate to https://google-gruyere.appspot.com/. Click "Continue" at the bottom of the page, and keep this page open in a tab, since it's Gruyere's own walkthrough and it's got hints and solutions for extra challenges later on.

2. In a new tab, open https://google-gruyere.appspot.com/start and click the start button. Gruyere spins up a private, sandboxed copy of the app just for you and redirects you to a URL like https://google-gruyere.appspot.com/\<a-long-instance-id\>/.

3. Write down your instance id somewhere handy: a sticky note, a scratch doc, whatever works. You'll be pasting it into a lot of URLs below, and nobody wants to retype that number from memory.

4. Click "sign up" in the top right corner of Gruyere. Create two accounts with basic, memorable credentials. You'll use both later on.

5. Sign into one of your accounts and click "New Snippet" in the top toolbar. Post a snippet with a simple sentence about anything, just so you get a feel for how the app normally works.

# **Part 2: Guided Challenges**

Complete all 9 challenges below in order. Each one walks you through what the bug is, why it actually matters in the real world, and the exact steps to pull it off yourself. 

We'll get started in class, but you're not expected to finish all 9 during the period. Treat class time as your head start and finish the rest as homework. You're welcome to work together, so talk through the tricky ones with a classmate if that helps.

Submit everything as one combined document, not a separate file per challenge. For every challenge, I'll need:

* A screenshot of every step you take, with a quick description under each one so I can follow along with what you did.

* A screenshot of the result of your "attack."

* Answers to the three reflection questions at the end of each challenge (these might take a little outside digging, and that's expected\!).

One more thing before you dive in: next to each challenge title, you'll see either \[black-box\] or \[white-box\]. Black-box means you can find the bug just by poking around like a normal user. White-box means the bug isn't visible anywhere in the app itself. You would have to read Gruyere's source code to find it. That source is browsable, separate from your own instance, at:

| https://google-gruyere.appspot.com/code/ |
| :---- |

Bookmark that too. A couple of challenges below will send you straight to a specific file in there.

## **Challenge 1: File Upload XSS**  *\[black-box\]*  **(2 points)**

What it is: we trust files more than we probably should. If a website lets your browser run an uploaded file as if it belongs to the site, what stops you from uploading malicious code?

* *Why this matters: This attack vector keeps showing in content-management systems, forums, and ticketing tools that serve uploads from the same address as the main site.*

1. Open a plain text editor (Notepad, TextEdit, VS Code, whatever you've got) and create a new file named poc.html with exactly this in it:
``` html
<script>
  alert(document.cookie);
</script>
```
![Pasted image 20260914122733.png](/img/user/SEC-250/Assignments/Gruyere%20Class%20Activity/_assets/Pasted%20image%2020260914122733.png)


2. In Gruyere, click "Upload" in the top menu and upload poc.html.
![Pasted image 20260914122836.png](/img/user/SEC-250/Assignments/Gruyere%20Class%20Activity/_assets/Pasted%20image%2020260914122836.png)

3. Gruyere will give you a confirmation page with a direct link to your file, something like https://google-gruyere.appspot.com/\<instance-id\>/\<your-username\>/poc.html.
![Pasted image 20260914122902.png](/img/user/SEC-250/Assignments/Gruyere%20Class%20Activity/_assets/Pasted%20image%2020260914122902.png)
* ***Screenshot**: The upload confirmation page showing that link.*

4. Open that link directly in a separate tab.
![Pasted image 20260914122954.png](/img/user/SEC-250/Assignments/Gruyere%20Class%20Activity/_assets/Pasted%20image%2020260914122954.png)
* ***Screenshot**: The alert box that pops up. It should show a long string that looks like GRUYERE=...|yourusername|...*

That string is your session cookie, meaning the thing keeping you logged in without retyping your password every time. Since your uploaded file ran as if it were part of Gruyere itself and read that cookie, you've just proven a file you uploaded can run with the full access of whoever's logged in, not just you.

### Reflection questions

Which OWASP Top 10 category (or categories) does this fit into? ([owasp.org/Top10](http://owasp.org/Top10))
 **This fits into:** *A06:2025 Insecure Design*

What kind of impact could this vulnerability have?
 **This vulnerability could allow an attacker to give users seemingly legitimate popups or notifications that look like they are coming from Google Gruyere itself in order to trick a user into sharing their login credentials or downloading malware.**

How would you fix this vulnerability?
**I would fix this by limiting the types of files that can be uploaded to the server via the upload button.**

## Challenge 2: Reflected XSS  *\[black-box\]*  **(2 points)**

What it is: some pages grab a value straight out of the URL and print it right back onto the page, no questions asked. If the site doesn't clean that value up first, whatever you put in the URL runs as code in the browser of whoever opens the link. 

* *Why this matters: eBay's own site had a bug like this back in 2014\. Someone could craft a link that dropped a convincing fake login form right over a real eBay page. Anyone who clicked it and typed in their password handed it straight over.*

1. Visit this URL (swap in your own instance id): `https://google-gruyere.appspot.com/<instance-id>/snippets.gtl?uid=doesnotexist123`
![Pasted image 20260914123914.png](/img/user/SEC-250/Assignments/Gruyere%20Class%20Activity/_assets/Pasted%20image%2020260914123914.png)
* ***Screenshot**: The resulting page: look for your made-up value ("doesnotexist123") echoed back somewhere on it.*

1. Now try this URL instead: `https://google-gruyere.appspot.com/<instance-id>/snippets.gtl?uid=<script>alert(document.cookie)</script>`
![Pasted image 20260914124235.png\|759](/img/user/SEC-250/Assignments/Gruyere%20Class%20Activity/_assets/Pasted%20image%2020260914124235.png)
* ***Screenshot**: The alert box that pops up, showing your live session cookie.*

Nothing was uploaded this time, and nothing got saved anywhere. The script ran the second the page loaded, straight off the URL. If someone texted or emailed you this link, or hid it behind a shortened URL, it'd work the same way. 

### Reflection questions

Which OWASP Top 10 category (or categories) does this fit into? ([owasp.org/Top10](http://owasp.org/Top10))
**This fits into:** *A05:2025 Injection*

 What kind of impact could this vulnerability have?
 **This could allow an attacker to run any arbitrary JavaScript code they want on the website as if it was coming directly from the website**

How would you fix this vulnerability?
**I would fix this by adding input sanitation to the code that handles the input from the URL**

## **Challenge 3: Stored XSS**  *\[black-box\]*  **(2 points)**

What it is: this time the malicious script gets saved to the database, so it shows up for every future visitor. No link to click, no upload, nothing. Whoever looks at the page next just becomes a victim automatically.

* *Why this matters: The 2005 "Samy" worm on MySpace is the classic story here. One profile carried a script that added "Samy is my hero" to the page and copied itself into anyone who viewed it. It hit over a million profiles in about 20 hours.*

1. Log in with your first account and create a new snippet (Gruyere allows a limited set of HTML tags in snippets; that's the whole attack surface here). Instead of ordinary text, post exactly this as the snippet content: 
	`<b onmouseover="alert(document.cookie)">hover over me</b>`
	![Pasted image 20260914124911.png](/img/user/SEC-250/Assignments/Gruyere%20Class%20Activity/_assets/Pasted%20image%2020260914124911.png)
2. Log in with your second account and view the snippet.
3. Hover your mouse over the bolded text. Don't click, just hover.
![Pasted image 20260914125246.png](/img/user/SEC-250/Assignments/Gruyere%20Class%20Activity/_assets/Pasted%20image%2020260914125246.png)

* ***Screenshot**: The snippet as it appears on the page, then the alert box that fires when you hover over it.*

Notice what's different from Challenge 2? Nobody had to click a crafted link this time. The payload just sits there, waiting for the next person to look, which in a real app could easily be an administrator moderating flagged content.
### Reflection questions

Which OWASP Top 10 category (or categories) does this fit into? ([owasp.org/Top10](http://owasp.org/Top10))
**This fits into:** *A06:2025 Insecure Design*
 
What kind of impact could this vulnerability have?
**This could allow an attacker to trick users into handing over account information or downloading malware. It lets an attacker impersonate the website.**
 
How would you fix this vulnerability?
**I would fix this by adding input sanitization to the text input on the New Snippet page.**
	
## **Challenge 4: Elevation of Privilege**  *\[white-box\]*  **(2 points)**

What it is: some apps let your browser tell the server what role you have instead of the server keeping track itself. If you can edit the message your browser sends, you might be able to edit your own permissions right along with it.

* *Why this matters: First American Financial Corp. exposed roughly 885 million real-estate and mortgage documents back in 2019\. This included bank account numbers, Social Security numbers, wire transfer records, all of it. Its document viewer trusted a number in the URL with no check that the requester actually owned that document.*

1. Log in and open your profile edit page ("Profile" in the menu). Notice the form only has fields for name, password, icon, website, color, and a private snippet, nothing about being an admin.

2. So if a hidden "admin" setting exists, it isn't in this form. It has to live in the code that handles the request when you hit Update. Go pull up Gruyere's published source and open this file directly:

| https://google-gruyere.appspot.com/code/resources/editprofile.gtl |
| :---- |

3. Use Ctrl+F (or Cmd+F) and search that page for is\_admin. You'll find code checking whether \_cookie.is\_admin is true, and further down, a hidden form field the admin version of this page includes that yours doesn't.
![Pasted image 20260914130142.png](/img/user/SEC-250/Assignments/Gruyere%20Class%20Activity/_assets/Pasted%20image%2020260914130142.png)
* ***Screenshot**: The part(s) of the source code where is\_admin shows up.*

4. Now visit this URL (swap in your own instance id):
`https://google-gruyere.appspot.com/<instance-id>/saveprofile?action=update&is_admin=True`

5. Nothing will look different right away. Your current session cookie still says you're not an admin. Log out and back in to grab a fresh one.
![Pasted image 20260914130323.png](/img/user/SEC-250/Assignments/Gruyere%20Class%20Activity/_assets/Pasted%20image%2020260914130323.png)
* ***Screenshot**: Your profile page after logging back in: look for a new admin-only option like "Manage the server."*

The page never showed you that field. It was not hidden, not grayed out, just not there at all. The only way to find it was to read the server's own source code, not to inspect the page harder. That's really the whole difference between black-box and white-box testing.
### Reflection questions

Which OWASP Top 10 category (or categories) does this fit into? ([owasp.org/Top10](http://owasp.org/Top10))
**This fits into:** *A05:2025 Injection*

What kind of impact could this vulnerability have?
**This could allow an attacker to edit the details of any user account on the website.**

How would you fix this vulnerability?
**I would fix this by  changing the code to stop trusting input directly from the URL without verifying it some other way.**
 

## **Challenge 5: Cross-Site Script Inclusion (XSSI)**  *\[white-box\]*  **(2 points)**

What it is: some endpoints hand back data dressed up to look like a JavaScript file. Browsers don't restrict who can load a \<script src=...\> tag the way they lock down other kinds of requests, so if that "script" happens to contain your own private data, any other page that knows the URL can pull it in and read it.

* *Why this matters: This same pattern (private data served in a format any page can execute and read) keeps turning up in real APIs as an "open by default" leak, handing account data to whatever page bothers to ask.*

1. Go to your profile and create a private snippet. Make sure to save\!
![Pasted image 20260914131027.png](/img/user/SEC-250/Assignments/Gruyere%20Class%20Activity/_assets/Pasted%20image%2020260914131027.png)
2. Log out of this account and go into the second one. You **shouldn’t** see the private snippet from your other profile.

3. While logged in, open this URL directly in your browser (swap in your instance id):
`https://google-gruyere.appspot.com/<instance-id>/feed.gtl`

4. Take a look at what comes back: it should contain your snippet data, formatted like a script instead of a normal web page.
![Pasted image 20260914131454.png](/img/user/SEC-250/Assignments/Gruyere%20Class%20Activity/_assets/Pasted%20image%2020260914131454.png)

* ***Screenshot:** The raw output of feed.gtl showing your private snippet data.*

This one's a little different from Challenges 1 through 3\.  Nobody injected anything into Gruyere. Gruyere's own, totally legitimate response is the leak. The problem is it never checks who's asking before handing back private data in a format anyone can run.
### Reflection questions

Which OWASP Top 10 category (or categories) does this fit into? ([owasp.org/Top10](http://owasp.org/Top10))
**This fits into:** *A06:2025 Insecure Design*

What kind of impact could this vulnerability have?
**This could leak private or sensitive data from user's accounts.**

 How would you fix this vulnerability?
 **I would fix this by adding code to check if the thing requesting the information has permission to view that thing before sending it to them.**

## **Challenge 6: Path Traversal (Information Disclosure)**  *\[black-box / white-box\]*  **(2 points)**

What it is: if a filename comes from something you typed or clicked, and the server just tacks it onto a folder path without double-checking it, then ".." (which just means "go up one folder") can walk your request right out of the folder it was supposed to stay in.

* *Why this matters: Citrix's ADC and Gateway appliances shipped with a path traversal bug (CVE-2019-19781, disclosed in December 2019\) that let an unauthenticated attacker plant and run their own code on the device. It got mass-exploited within weeks and became a favorite way in for ransomware crews.*

1. Gruyere ships a file called secret.txt one folder above the public files it normally serves.
2. Visit this URL directly (swap in your instance id):
`https://google-gruyere.appspot.com/<instance-id>/..%2fsecret.txt`

* *Heads up: That %2f is an encoded slash, on purpose. If you type a literal ../ into the address bar, your browser "cleans up" the URL and strips it out before the request is sent.*
![Pasted image 20260914134117.png](/img/user/SEC-250/Assignments/Gruyere%20Class%20Activity/_assets/Pasted%20image%2020260914134117.png)
* ***Screenshot**: The contents of secret.txt showing up in your browser, a file the app was never supposed to let you near.*

### Reflection questions
Which OWASP Top 10 category (or categories) does this fit into? ([owasp.org/Top10](http://owasp.org/Top10))
**This fits into:** *A01:2025 Broken Access Control* 

What kind of impact could this vulnerability have?
**This could allow an attacker to view important files on the server like /etc/shadow.**
 
How would you fix this vulnerability?
**I would make sure the server is not on a vulnerable version and properly configure the server do disallow this.**
 

## **Challenge 7: Configuration / Information Disclosure**  *\[white-box\]*  **(2 points)**

What it is: debug pages that were handy for a developer during testing sometimes get left in the app when it ships, and they usually show way more than any normal page would.

* *Why this matters: Forgotten debug and admin consoles (exposed database pages, unauthenticated dashboards, debug endpoints nobody remembered to turn off) show up constantly in real breach reports and bug-bounty writeups.*

1. Visit this URL on your own instance (swap in your instance id):
`https://google-gruyere.appspot.com/<instance-id>/dump.gtl`
* *Screenshot: The full contents of the database this page dumps out, including other users' usernames and passwords in plaintext.*
![Pasted image 20260914134547.png](/img/user/SEC-250/Assignments/Gruyere%20Class%20Activity/_assets/Pasted%20image%2020260914134547.png)

This wasn't some complicated exploit. It's just a page a developer built for debugging and never locked down. Most real-world information disclosure looks exactly like this… nothing fancy, just something somebody forgot to switch off.

### Reflection questions
Which OWASP Top 10 category (or categories) does this fit into? ([owasp.org/Top10](http://owasp.org/Top10))
**This fits into:** *A06:2025 Insecure Design*
 
What kind of impact could this vulnerability have?
**This allows an attacker to see everything about all users on the website including their passwords. If an attacker found this they would completely own the website.**
 
How would you fix this vulnerability?
**I would remove the page from the website.**

## **Challenge 8: Code Execution (Template Injection)**  *\[white-box\]*  **(2 points)**

Wheilled-in data gets run back through the template process a second time. So if what you typed happens to look like a template instruction, it gets carried out as one. Plain text you typed turns into a command the server runs. This whole bug class is called server-side template injection, and it's behind some of the nastiest real-world remote code execution vulnerabilities out there.

* *Why this matters: The 2017 Equifax breach is the big example here: attackers used a known, already-patched code-execution bug in the Apache Struts framework to run their own code on Equifax's servers, eventually exposing sensitive data (including Social Security numbers) for about 143 million people.*

1. Log in and open your profile edit page.

2. In the "Private Snippet" box, instead of ordinary text, type exactly this:
`{{_db:pprint}}`
![Pasted image 20260918112140.png](/img/user/SEC-250/Assignments/Gruyere%20Class%20Activity/_assets/Pasted%20image%2020260918112140.png)
3. Click Update to save it.

4. Go back to your profile page and take another look.
![Pasted image 20260918112158.png](/img/user/SEC-250/Assignments/Gruyere%20Class%20Activity/_assets/Pasted%20image%2020260918112158.png)

* ***Screenshot:** Your profile page showing the entire database printed out, instead of your snippet text.*

Every challenge up to this point got an attacker something out of the app whether it’s a cookie, a deleted snippet, a file. This one's different… The plain text you typed became an instruction the server followed. That's the real line between "the app leaked data" and "the attacker is now running code as the app."

### Reflection questions
Which OWASP Top 10 category (or categories) does this fit into? ([owasp.org/Top10](http://owasp.org/Top10))
**This fits into:** *A05:2025 Injection* 

What kind of impact could this vulnerability have?
**This allows attackers to run code as the application itself. It could compromise all data in the application and possibly even let an attacker modify data on the server.**

How would you fix this vulnerability?
**This can be fixed by adding input sanitization to the text box.** 

## **Challenge 9: Denial of Service (Quit the Server)**  *\[white-box\]*  **(2 points)**

What it is: some admin or maintenance actions are just sitting there as ordinary links, with no extra confirmation or login check. If anyone can visit that link, anyone can use it to take the whole app down for every user on that instance.

* *Why this matters: The October 2016 Mirai botnet attack on Dyn's DNS infrastructure is the large-scale version of this same idea: by flooding one piece of shared infrastructure with more traffic than it could handle, attackers knocked Twitter, Netflix, Reddit, and a bunch of other major sites offline for hours.*

* ***Heads up:** Save this one for LAST\! It ends your Gruyere instance. You won't be able to go back and redo an earlier challenge afterward without spinning up a brand-new instance from https://google-gruyere.appspot.com/start.*

1. Log in as an admin (from Challenge 4\) and open the server management page.

2. Notice a "quit the server" link sitting right next to "reset the server", both are just links, no extra login step, no confirmation.
![Pasted image 20260918112741.png](/img/user/SEC-250/Assignments/Gruyere%20Class%20Activity/_assets/Pasted%20image%2020260918112741.png)

3. Go ahead and click "quit the server."
![Pasted image 20260918112803.png](/img/user/SEC-250/Assignments/Gruyere%20Class%20Activity/_assets/Pasted%20image%2020260918112803.png)
![Pasted image 20260918112828.png](/img/user/SEC-250/Assignments/Gruyere%20Class%20Activity/_assets/Pasted%20image%2020260918112828.png)
* ***Screenshot:** The management page showing the "quit the server" link before you click it, then the error or blank page you get afterward when you try to reload Gruyere.*

This is the simplest bug in the activity, and honestly one of the most damaging in the real world too. A destructive action that should've needed authentication and a confirmation step, and instead was just... a link.

**Reflection questions**

Which OWASP Top 10 category (or categories) does this fit into? ([owasp.org/Top10](http://owasp.org/Top10))
 **This fits into:** *A06:2025 Insecure Design*

What kind of impact could this vulnerability have?
**This allows an attacker or literally anybody to take down the server simply by visiting a link. The website could even be taken down accidentally**

How would you fix this vulnerability?
**To fix this, i would add authentication and a confirmation message before quitting the server. I would also make it be something other than a link.**

 **Wrap-Up**

Once you've wrapped up all 9 challenges, look back over your reflection answers and chew on these:

* Which of these ten bugs could a teammate have caught just by reviewing the code, before the app ever shipped?
**All of these attacks except for challenge 6 could have been caught by careful code review before the app shipped.**

* Which ones would only ever show up by actually running and attacking the app?
**Challenge 6 would only show up when running the app because it involves the web server's configuration/version along with the apps code.**

* If you could only add one fix to Gruyere's codebase to wipe out the most bugs at once, what would it be, and why
**The one most important fix i would add to the codebase is input sanitation on all user inputs. Most of these attacks would not have been possible if user input was treated more carefully.**

*Want more practice? The Gruyere codelab you bookmarked in Part 1 has several more challenges we didn't get to here: Cookie Manipulation, Stored XSS via HTML Attribute or AJAX, Reflected XSS via AJAX, Denial of Service by Overloading, and a few more configuration and AJAX bugs. They each come with their own hints and solutions on the official site, so dig in if you want extra credit or just want to keep going.*