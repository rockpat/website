## Hello, Schön guten Tag or Bonjour :-)

This is my Github Repository for my Website (jakubwieloch.com).

### BIG Problem :-/

So as might have already noticed, my Website works (mostly) but is kinda fundamentally broken. Let me explain.
**The Problem is**, that if you navigate to a usermade site like my Blog, the Blog itself & the Blog-Posts send you to `Localhost:1313` instead of my domain.

If you say I should just use `hugo` instead of `hugo server`, then you're wrong. Like said, this is a fundamental problem which is likely caused because some template code doesn't later change from localhost:1313 to my domain, even when I just use `hugo`.

What I use: OS = Debian Testing, Hosting = Cloudflare pages & Hugo version v0.131.0+extended (debian package)

If you even just have an indicator for what the problem could be besides like really obvious shit like setting the baseURL or just running the hugo command, then please open an issue. 

Have a great day :-)
