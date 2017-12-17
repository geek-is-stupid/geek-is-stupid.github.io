---
layout: post
title: How to show some warnings when added //TODO //FIXME //ERROR in Xcode?
categories: [ios]
tags: [swift, xcode, ios, todo, fixme, error, warnings]
fullview: true
---

If you make a note in xcode, and you dont want to miss that, there are some solution for that, I will show you one:

```swift
//TODO: Let read this blog in 15 mins
//FIXME: I need you fix me man :)
//ERROR: Noooooooooo, get out of here the error
```

OK now how to make xcode understand to show your wanrning and error:

Go to `Build Phases` and create new Run script:


![https://raw.githubusercontent.com/geek-is-stupid/geek-is-stupid.github.io/master/images/post/2016-08-17-how-to-show-warning-when-add-slash-slash-todo-and-slash-slash-fixme-in-xcode/1.png](https://raw.githubusercontent.com/geek-is-stupid/geek-is-stupid.github.io/master/images/post/2016-08-17-how-to-show-warning-when-add-slash-slash-todo-and-slash-slash-fixme-in-xcode/1.png)

Add this script:

```swift
TAGS="TODO:|FIXME:"
ERRORTAG="ERROR:"
find "${SRCROOT}" \( -name "*.h" -or -name "*.m" -or -name "*.swift" \) -print0 | xargs -0 egrep --with-filename --line-number --only-matching "($TAGS).*\$|($ERRORTAG).*\$" | perl -p -e "s/($TAGS)/ warning: \$1/" | perl -p -e "s/($ERRORTAG)/ error: \$1/"
```

