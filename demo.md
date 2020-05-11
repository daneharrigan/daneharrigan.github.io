---
title: A Demo of Every Element
layout: post
date: 2020-05-10
---

First there was a paragraph that explained the problem I was facing. Next I
describe what this problem did to me. And why I had to solve it. Something else
dramatic happens and then we begin.

```
package main

import (
	"net/http"
	"encoding/json"
)

http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request){
	json.NewEncoder(w).Encode("lol")
})
```

Of course I say a few things and call out certain functions like
`http.HandleFunc` and `Encode`. And only after I do that do I list the problems
this causes:

* This is a bad thing
* I can't believe this happens too
* Why can't everything be easy

## A dramatic title of my first solution

Of course this doesn't work. Who ever gets a solution right on the first try?
Certainly not me. I probably started investigating and testing theories. None of
them panned out, but I'll show you a quote because it was cheeky.

> This was pretty funny so I thought I'd share it. One day you too will say
> something funny.

Luckily I found an piece of a comic that illustrates my point so I decided to
embed it here. I hope this is helpful.

![Demo image of Dr. Doom](/assets/images/demo-image.jpg)
Author's Last Name, First name. _Title of Series_. No. 1, Marvel Comics, 2010

## Some kind of conclusion

Now that I've shown you the following:

1. Paragraph
2. Code snippet
3. Headers
4. Quotes
5. Embedded images

I can rest assured that all of the elements that I care about or at least
anticipate using are covered. Now it's time to style this up.

