---
layout: post
title: Customize my Theme
description: "我是如何定制我的主题的？"
modified: 2022-07-14
tags: [Theme set-up]
image:
  path: /images/abstract-10.jpg
  feature: abstract-10.jpg
---

> This theme is powered by [Jekyll](http://jekyllrb.com/), using [HPSTR](https://mademistakes.com/work/hpstr-jekyll-theme/), If you are fond of this Theme and would like to get the same theme, see [theme set-up](/theme-setup/).


Last night I was tossing and turning, thinking of some fabulous ideas about how I customized this theme. And I'm gonna do a quick write-down in case my ideas are all gone. Click [here](https://mmistakes.github.io/jekyll-theme-hpstr/) to take a quick look at the original look of this theme. Let's get it started.

___

## Minimalisation[^1]

My goal is to **minimalize** my blog.

At first you can see all the content of all posts just at the **Home page**, even with a picture featuring, whice are exacly what they're proud of. But to me it just like **chaos** though. I decided to hide the content and pictures at the Home page. As you can see now, there're only titles and description at the Home page, which looks a whole lot better to me. When I saw the effect I know that was exacly what I wanted. And that's probably the first step that I started to customize my own theme.

By the way, does this design make you feel more clear and comfortable when you first time came to ASTROWORLD? I asked a few friends of mine for advice. They tended to feel confused when something out of order suddenly popped to their eyes.

## Size Change

I changed the size of **main content** from 800px to 1000px, otherwise it'll seems sort of wired that your content only takes less than a half when the width of whole page is 2000px.

To apply to that change, I also changed the `font size` of each word from 12px to 16px. It seems to be more friendly for myopic friends(x.

## Sticky On Top

I wanted to stick one of my posts on top of the home page. But it did't seem to be easy for me to implement due to lack of understanding of Jekyll grammer.

With the help of my kind roomate [Will](https://willyangywt.cc), The design was successfully implemented with '**violence**', which is not a good implementation. I will think of a plan to better solve this problem.

Now, as you can see, the top post of my home page is [Guide/向导](https://lucameng.github.io/Guide/), which aims to give people some tips when they first come to ASTROWORLD.

## Code Highlight Block

The original code highlight block is great. But when I saw something better, 你会发现它瞬间不香了. At first the block was rectangle with sharp corner, which might be uncomfortable to some people(include me). After I turn the `border-radius` from 0 to `.75em`, the block has fillet[^2] now.

Also I changed the back ground color. It was totally **black** before, I turned it to be a **lighter dark** that is similar to the default code block color of [CSDN](https://www.csdn.net/). The contrast between code block and plain block is lessened, which is visually better.

The code higtlight seems to be not friendly to `bash` grammer. I'm going to figure out a good way to fix it though.

## Social Share

I'm gonna expand the social share interface, probably add Weibo and Wechat. It will be implemented a few days later(after the deadlines of so many projects).


## Folk's Blog

Add an interface of `Friends & Folks` at the upper left **Menu Navigation**. Now you can check out my friends' blog through that interface.

___

# Ideas need to be realized

## Table of Content to be improved

## Modify time to be disaplayed

### To be continued...

<br/>
<br/>

___

[^1]: minimalisation: 极简
[^2]: fillet: 圆角
