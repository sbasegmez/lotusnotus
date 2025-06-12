---
title: "Moved my blog: The third generation!"
slug: moved-blog-third-generation
date: 2025-06-12T10:18:30.824Z
tags:
    - blogging
    - mkdocs
categories:
    - News
---

It's been four years since my last blog post. Even back then, I mostly just shared a slide deck or a few brief updates — so I haven’t really blogged properly for nearly a decade.
<!-- more -->
I wouldn’t go as far as saying '*I’m back*' just yet. But over the past couple of years, sticking with Domino Blog gave me a convenient excuse to keep putting off the next post. To be clear, I’ve always liked the Domino Blog design — it’s a brilliantly built and very capable NSF design. I even spent a few hours over a beer [giving the blog a facelift with Bootstrap](../imported/2013-11-change-is-good....md), and ended up sharing that template with dozens of others. The issue isn’t with the blog template itself, but sadly with Notes. I won’t articulate this further, but the keywords are 'macOS on ARM' and 'Notes Rich Text Format'.

Meanwhile, static site generators have always been on my radar. I'd used MkDocs before for documentation. More recently, after a brief chat with [Paul Withers](https://paulswithers.github.io/), I decided to move my blog to [MkDocs Material](https://squidfunk.github.io/mkdocs-material/). And here we are...

## Why Mkdocs?

I had a few key criteria. First, I wanted something simple. That immediately ruled out most CMS platforms (like WordPress or Drupal). It’s not just the unnecessary complexity — they often require specific hosting environments. I just want to share technical thoughts, maybe a bit of code and a couple of screenshots. No need to make it more complicated than that.

There are also blogging platforms like Medium or Substack, but I’m not keen on relying on a service long-term — they change their rules far too often.

Static site generators tick all the boxes. They’re easy to set up and use — especially for tech-minded folks like us. Hosting is no problem either. There are free options, and I can even host this blog on my Domino server. Plus, switching platforms is easy. Everything’s written in Markdown, so I could move to Jekyll today and Hugo tomorrow without a fuss.

I compared [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/), [Jekyll](https://jekyllrb.com/), and briefly explored [Hugo](https://gohugo.io/). Jekyll is by far the most popular blogging platform, and Hugo seemed like a better fit in terms of design flexibility. But Material for MkDocs stood out for its simplicity — and the fact that it’s Python-based was a big plus for me.

## Existing blog posts

I had nearly 400 posts on my old blog — about half of them in Turkish — and I didn't want to leave them behind. That part was tricky.

This is where my developer side kicked in. I wrote a Java utility to convert all the posts to Markdown, clean up a few oddities, and add the front matter metadata. After some manual work, like fixing categories, tags, and code blocks, the migration was sorted.

!!! note
    Of course, I shared the [Domino Blog Converter](https://github.com/sbasegmez/Domino-Blog-Converter) code on my Github account. At worst, it ends up being a sample application for Domino JNX library.

## Where to host

Another question was how to host the new blog. The obvious answer is GitHub Pages — it’s very easy to set up and more than capable of hosting static pages. But it was missing one key feature I needed: server-side redirection. I wanted to create redirects for old URLs, as many people had linked to my blog from all sorts of places.

After some digging, I found out that GitLab Pages and Cloudflare Pages both provide server-side redirection out of the box. I didn’t want to open another repository on GitLab, and I already use Cloudflare for various things. So I picked [Cloudflare Pages](https://pages.cloudflare.com/).

## Writing posts

I suppose this part is a bit tricky. Given that I tend to find any excuse to procrastinate on writing, I need a practical platform to get things done. It’s not just about writing Markdown, but also embedding images, managing metadata (like tags and categories), and other details. Plus, I’m not a native English speaker, so I need ChatGPT to proofread me regularly.

Googled and tested a few alternatives. Normally, I use [Typora](https://typora.io/) for writing Markdown documents and [Bear](https://bear.app/) for note-taking, but neither of them works well for blogging. I tried several other writing apps, but each was missing one or two features I needed. In the end, I circled back to Visual Studio Code, which can be customised endlessly. I also discovered a brilliant plugin called [Front Matter CMS](https://frontmatter.codes), which fits my workflow perfectly.

## Finally

As of yesterday, I pointed my domain to the Cloudflare Pages address — and here we are.

My Turkish blog archive now lives on a separate site: [https://tr.lotusnotus.com](https://tr.lotusnotus.com). For a limited time, the old blog will remain online at [https://old.lotusnotus.com](https://old.lotusnotus.com) as a backup. I’ve removed comments from the new site and didn’t migrate old comments, not even as static content.

Finally, a big thank you to [Prominic](https://www.prominic.net/). For the past five years, they hosted my Domino server and blog databases — and I never once needed to log into the admin console. Everything just worked. Top job!
