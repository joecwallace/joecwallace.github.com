---
title: Subtree Sculpin deployments
---

## Subtree Sculpin deployments

So, you read my last post about [Minimum Viable Sculpin](http://joecwallace.com/2017/09/28/mvs-minimum-viable-sculpin/) and made yourself a sweet Internet Web Site, but you want to deploy it to GitHub Pages - like I mentioned. *[Aside: I wrote that entire last blog post just so that I could write this one because I felt like an MFing WIZARD when I figured this out.]* The problem is that GitHub Pages wants this directory structure:

```
|- css/
   |- # stuff is in here
|- js/
   |- # stuff is in here
|- other-folder/
   |- # maybe some other stuff
|- index.html
```

But our good friend Sculpin wants this:

```
|- source/
   |- # all the stuff is in here
|- composer.json
|- composer.lock
|- # and maybe some other files, I don't know
```

Ehh... Those things are *not* the same.

### What to do?

If you're like me, you went HAM on the `master` branch in the last blog post, and - of course - for *personal* GitHub Pages, you deploy from the `master` branch. So now you're like "WTF?", and we're going to have to 💣💥🔥🔫🔪 that thing.

But let's back up some. The first thing that we want to do is preserve your work by creating a new `develop` branch. Or maybe you'll call yours `blogging` or `writing`, since it's the branch that you'll use to *produce* content. Anyway...

```
$ git checkout -b develop-or-whatever
$ ./vendor/bin/sculpin generate --env=prod
$ git add .
$ git commit -m 'Built production'
```

OK. We generated the production version of your site, which you may have done previously, and now you have an `output_prod` folder hanging out in there, which we've added and commited to the repository. This is good: `output_prod` is where the magic will happen.

### Tangent: Dafuq's a subtree?

What are subtrees? Well, they're a thing that Git does. And uh, they're like subfolders that have separate repositories (or at least separate branches, which is an important detail that we'll see later). And, y'know, they're like The Upside Down in Stranger Things because they're totally a different world, but they also have manifestations in the real world - aka your `develop-or-whatever` branch.

And - I'm going to be totally honest with you here - I don't know how to describe them to you, but lots of other projects use them successfully. Like all of the Laravel packages. Go check out the [Illuminate](https://github.com/illuminate) GitHub: they're almost all called "[READ ONLY] Subtree split", and they get all smooshed together in the [Laravel Framework](https://github.com/laravel/framework) package.

And here's the kicker if you go in the [`src/Illuminate`](https://github.com/laravel/framework/tree/5.5/src/Illuminate) folder, ***ALL OF THE SUBFOLDERS ARE STILL IN THERE - WHAT THE WHAT?!?***. But - they're really managed by the content of the subtrees. (This concept is the Demogorgon.)

In other words, a subtree is confusing - that's dafuq it is - but it's also better than a submodule if you've ever marched through that particular circle of Hell.

### Back to the action

You're on the `develop-or-whatever` branch, and you've got your full `master` branch up on GitHub just in case this goes bad. Now, you're going to *boldly* delete the `master` branch from your local copy of the repository.

```
$ git branch -D master
```

Why do you do that? Because you need just the folder structure that GitHub Pages wants in there - i.e. you need the `output_prod` folder.

Oh? We need `output_prod`, you say?

```
$ git subtree split --prefix=output_prod -b master
```

That just created a new version of the `master` branch for you. Its sole contents are the `output_prod` folder's contents of your current branch. And that's exactly what you want. You can verify this by running the following commands:

```
$ git branch -v
  # Helpfully shows you all the branches you have, including your swanky new master
$ git checkout master
$ ls -lah
  # Hey look, I was right. That looks just like the output_prod folder
```

Great. That was cool, so now you're feeling all cocky - cocky enough to run *this command*:

```
$ git push -f origin master:master
```

*You: Yo, did he just `push -f` to `master`?*

*Macho Man Randy Savage:*

![Just Relax](https://media.giphy.com/media/oCPglisSuGsA8/giphy.gif)

The nice thing is that you just pushed the newest version of your site to the `master` branch, so now it's deployed on GitHub Pages. The weird thing is that ... we're not done working yet. Bummer.

### Repeal and replace the subtree
```
$ git rm -r output_prod
$ git add -A
$ git commit -m 'Remove output_prod'
$ git subtree add --prefix=output_prod origin master
```

Phew. You deleted the `output_prod` folder and added it right back - as a subtree.

### Updating your site

From here on, you basically want to stay on your `develop-or-whatever` branch. Write an article, update your headshot, do whatever you do, and then run:

```
$ git add .
$ git commit -m 'Made the changes'
$ git push origin develop-or-whatever
$ ./vendor/bin/sculpin generate --env=prod
$ git add output_prod
$ git commit -m 'Build for production'
$ git subtree push --prefix=output_prod origin master
```

Did you notice how we committed the changes to the site *before* we generated the production version and then *isolated the changes to the `output_prod` folder*? That's important. Because reasons.

(Really, you have to remember that your `output_prod` subtree is the completely separate - and different - `master` branch, and if you crossed the streams, it would be bad - i.e. your history would be FUBAR.)

### One last note

Good news! Your new static web site earned you so much money that you bought a new computer, so now you need to clone the repository and publish a new article. Well, it's a tiny bit trickier than just cloning and hacking away. Of course, you'll still need to clone:

```
$ git clone https://github.com/your/site.git
```

But then you'll have to run the 4 commands in [Repeal and replace the subtree](#repeal-and-replace-the-subtree). And of course, you'll follow the procedure in [Updating your site](#updating-your-site) to publish updates.

### Bonus

You thought that last last note was the last note. Nope. Sucker.

These commands aren't *everyday* Git commands - for me, anyway. So you may want to alias them. Jam this in your `~/.zshrc` or whatever you do.

```
alias develop-site='./vendor/bin/sculpin generate --watch --server'
alias build-site='./vendor/bin/sculpin generate --env=prod'
alias publish-site='git subtree push --prefix=output_prod origin master'
```

We didn't cover that first one in this article; it's a freebie, but you'll use it often if you keep up the writing.

### Fin.

I don't know. Maybe this was obvious to you geniuses, but I was going down the road of maintaining 2 separate repositories for my site - one for the source and one for the build - until I thought of this. And - as with all good ideas - it was inspired by other similar ones (see below).

*Credit where due: I learned most of the strategy that I used here from [LosTechies "Using Git subtrees to split a repository"](https://lostechies.com/johnteague/2014/04/04/using-git-subtrees-to-split-a-repository/).*
