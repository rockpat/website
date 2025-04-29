## JAKUBWIELOCH.COM

The one and only Github Repository for my Website (jakubwieloch.com).


### How does it work?

I'm using [Hugo](https://github.com/gohugoio/hugo), as my static site generator of choice, [hugo-PaperMod](https://github.com/adityatelange/hugo-PaperMod) as my theme and I'm deploying my Website to Cloudflare (Pages). :-)

### History :-/

My Website, for the longest time in existense had issues, one of them game breaking...

> So as might have already noticed, my Website works (mostly) but is kinda fundamentally broken. Let me explain.
**The Problem is**, that if you navigate to a usermade site like my Blog, the Blog itself & the Blog-Posts send you to `Localhost:1313` instead of my domain.
>
> If you say I should just use `hugo` instead of `hugo server`, then you're wrong. Like said, this is a fundamental problem which is likely caused because some template code doesn't later change from localhost:1313 to my domain, even when I just use `hugo`.
>
> If you even just have an indicator for what the problem could be besides like really obvious shit like setting the baseURL or just running the hugo command, then please open an issue. 

This was quoted from my old README.md here. You might ask how long did this problem stuck around? 
**8 fucking Months.**

**I've fixed it today (as of writing), the 10.03.2025.**

Then you might ask me how did I fix it, especially after SUCH a long time.

*I don't know, exactly.*

But it was very likely it were a fuck ton of small over sights that ended in a disaster.

**If you're experiencing the exact same problem, I'll write a blog post about it.**

**TL;DR: Solution**
- Check your Cloudflare deployment & build setting/flags!
- If pages of your website are missing: use `_index.md` instead of `index.md` and [read this!](https://gohugo.io/content-management/page-bundles/)
- Question everything... 

### How to install?

```sh
git clone https://github.com/rockpat/website.git ~/Github/website
git submodule update --init --recursive # needed to also clone PaperMod
```

### Creating your own Website ;-)

Coming soon!



###### *Have a great day :-)*
