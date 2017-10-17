---
title: My First Composer Package
---

## My First Composer Package

Friend, I have never written a Composer package before, so in this article, I'm going to walk through trying to do it for the first time. It might be a train wreck.

Without further ado...

***&lt;record scratch&gt;***

I started writing this thing, and then I realized it was going to get YUGE. It is now a series. This is part 1.

Here are the other parts:

1. [What now? Digital Ocean, I guess](/2017/10/10/2-what-now)

### Requirements

I want a library that creates servers on cloud providers. To just spitball some syntax, I'm thinking something like this:

```
$configArray = ['some' => 'useful', 'stuff' => 'like', 'api' => 'keys'];
$provisioner = new \MyPackage\Provisioner($configArray);
$provisioner
    ->server()
    ->named('HAL9000')
    ->in('east')
    ->from('image')
    ->sized('large')
    ->launch('aws');
```

This is obviously subject to change, but basically, I've supplied a name, region, image, size, & provider for the instance. Later, we'll need to stop and/or terminate instances, but we'll focus only on creation right now.

### Boilerplate

I made a subdirectory in the directory that has all my codestuffs in it already. I called it `multicloud`. I hate it, but let's keep moving.

I then created `src/` & `tests/` folders, initialized Composer, initialized Git, and installed my first Composer dependency - `phpunit`.

Here are the commands:

```
$ mkdir multicloud && cd multicloud
$ mkdir {src,tests}
$ composer init
$ git init
$ composer require --dev phpunit/phpunit
```

Here's the contents of my `composer.json`:

```
{
    "name": "joecwallace/multicloud",
    "description": "Create cloud servers across multiple providers - AWS, Digital Ocean, etc",
    "type": "library",
    "license": "MIT",
    "authors": [
        {
            "name": "Joe C. Wallace, III",
            "email": "joe.c.wallace@gmail.com"
        }
    ],
    "autoload": {
        "psr-4": {
            "Multicloud\\": "src/"
        }
    },
    "require": {},
    "require-dev": {
        "phpunit/phpunit": "^6.4"
    }
}
```

Notice the `autoload` entry. I had to set that up manually.

Finally, I created a basic `.gitignore` file:

```
composer.lock
vendor/
```

Basic. Let's get to the good stuff.
