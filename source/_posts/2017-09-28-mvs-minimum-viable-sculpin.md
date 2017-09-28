---
title: MVS - Minimum Viable Sculpin
---

## MVS - Minimum Viable Sculpin

This site has sat here, unloved, for some years. I have good itentions for it - always have - but I keep [yak shaving](https://en.wiktionary.org/wiki/yak_shaving). Anyway, now it's up and running more-or-less the way that I've wanted, and that's because I was finally able to undertand what constitutes Minimum Viable Sculpin.

*[tl;dr - Here is [the repo for this site](https://github.com/joecwallace/joecwallace.github.com/tree/285bc5d56d20fae636547b40a7f7e4a5235b3aa3) at its most basic. You can even ignore the `output_prod/` folder and `CNAME` file in there.]*

### Requirements

You see - there are 2 factors to my site:

1. I want to use my personal domain for it.
1. I would rather not pay for it. (WHERE MY MISERS AT, Y'ALL?!?)

The best way I know to get both of these things is to use [GitHub Pages](https://pages.github.com/) and a static site generator, and since I mostly write PHP these days, I decided to give Sculpin a shot.

### GitHub Pages

It's out of the scope of this article to explain how to get started with GitHub Pages, so you can head on over to the documentation at the link above and figure that out. The important part about GitHub Pages, though, is that it's free hosting for a static site at your custom domain! That satisfies Requirements #1 and #2 up there, so we're off to a good start.

Now, we just need to generate a static site.

### Sculpin

Sculpin is the most prominent static site generator written in PHP. (I'm mainly making that up, but it *IS* the #1 Google search result right now for "PHP static site generator".) The main idea is that you create some templates, write some markdown, generate the site, *et voilà*.

I've gone down this road multiple times, and it - for me, at least - always ends in frustration. The [Get Started](https://sculpin.io/getstarted/) section of the Sculpin documentation suggests using their "Starter Kit", which is a great way to see a bunch of things the first time you attempt to generate your file. The problem is that I found it overwhelming and difficult to separate the wheat from the chaff, and that (previously) has always ultimately stymied me.

But now, I've figured it out! [This whole site](https://joecwallace.com) - [at the time of this writing](https://github.com/joecwallace/joecwallace.github.com/tree/285bc5d56d20fae636547b40a7f7e4a5235b3aa3) - is the product of 4 directories & 5 files (excluding Composer stuff), and if you don't care about styles at all (a look I like to call "retro-responsive"), you can eliminate 1 folder & 1 file. 💥

#### All the files and folders that you need.

Here's the whole tree:

```
(root)
|- source/
   |- _layouts/
      |- default.twig
      |- post.twig
   |- _posts/
      |- 2049-03-17-sexy-futuristic-post.md
      |- # repeat as necessary
   |- css/
      |- styles.css
   |- index.md
|- composer.json
|- composer.lock
```

#### The commands that get you there

Easy-peasy. To make this even easier, here are the commands that I used to get there:

```
$ mkdir sexy-futuristic-blog && cd sexy-futuristic-blog
$ composer init -q -s dev
$ composer require sculpin/sculpin
$ mkdir -p source/_layouts source/_posts source/css
```

If you're a regular bash user and have experience with Composer (and I'm assuming you do), then this should be obvious to you. I will note, though, that `composer init -q -s dev` creates a `composer.json` file for you without any interaction and sets the minimum stability to `dev` (which, at the time of this writing, seems to be required to install Sculpin).

Now you just need to add your content. Let's walk through the 5 required files.

#### Breaking it down

1. `source/_layouts/default.twig` - This is the HTML structure for you page. It's a `.twig` file, which may be intimidating if you've never used twig, but it's *just an HTML file*. And then in the middle - wherever you want your "content" to come out, you write: `{{ '{%' }} block content {{ '%}{%' }} endblock {{ '%}' }}`. Here's [my default.twig](https://github.com/joecwallace/joecwallace.github.com/blob/285bc5d56d20fae636547b40a7f7e4a5235b3aa3/source/_layouts/default.twig) right now; it's super simple.
1. `source/_layouts/post.twig` - [In my case](https://github.com/joecwallace/joecwallace.github.com/blob/285bc5d56d20fae636547b40a7f7e4a5235b3aa3/source/_layouts/post.twig), this doesn't really add anything, but Sculpin wants to find it. The important part is `{{ '{%' }} extends 'default' {{ '%}' }}`. If you don't have a footer block, then you that little snippet is literally all you need.
1. `source/_posts/2049-03-17-sexy-futuristic-post.md` - This is a blog post or article or whatever. It has a special format, but - as far as I can tell - the only thing that you really need here is the `title` field in the YAML header. After the header, it's just a markdown file. [My first post](https://raw.githubusercontent.com/joecwallace/joecwallace.github.com/285bc5d56d20fae636547b40a7f7e4a5235b3aa3/source/_posts/2017-09-27-hello-world.md) is about as simple as it gets.
1. `source/css/styles.css` - This is a stylesheet. You probably already know what it is, and - as mentioned previously - it's technically optional. End of discussion.
1. `source/index.md` - This is your home page! It needs some special YAML at the top, and if you want to display posts, you have to do a little extra work. Again though - [my version](https://raw.githubusercontent.com/joecwallace/joecwallace.github.com/285bc5d56d20fae636547b40a7f7e4a5235b3aa3/source/index.md) is very simple. It should be pretty easy to figure out. The 2 "trickiest" parts are the `use: ["posts"]` line and the `{{ '{%' }} for post in data.posts {{ '%}' }}` block, which are related and - you'll never guess this part - have to do with listing all of the posts.

#### Admiring your work

Now, you've achieved MVS. To prove it, run:

```
./vendor/bin/scupin generate --watch --server
open http://localhost:8000
```

### Fin.

There you have it: an empty fistful of excuses.

I'm sure that you - or some of you, at least - are thinking, "What about pagination and categories and tags *et cetera*?" To you, I say "Add that later. Just get this little setup working for you and start writing."
