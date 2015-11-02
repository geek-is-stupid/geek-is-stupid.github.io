---
layout: post
title: "How to manage all localized strings in your project?"
date: 2015-10-23 21:46:19 +0700
comments: true
categories: 
---
### Using ```'genstrings'``` tool from Apple which included into Xcode

Step 1. Add languages which you want to localize.

On the left side of Xcode, choose ```Show the project navigator``` > Choose your project:

<a href="https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-23-at-10-13-36-pm.png"><img class="alignnone size-medium wp-image-227" src="https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-23-at-10-13-36-pm.png" /></a>

Step 2. Open terminal and go to your project's folder: ```cd YourProject```

Step 3. Run: ```find . -name '*.swift' -print | xargs -n1 genstrings -a```

After this step, you will see the files: 

- ```Localizable.strings```
- ```Base.lproj```
- ```vi.lproj``` 
- ```en.lproj```

Step 3. Add only file: ```Localizable.strings``` in your project

<a href="https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-23-at-9-47-21-pm.png"><img class="alignnone size-medium wp-image-227" src="https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-23-at-9-47-21-pm.png" /></a>

Step 4: On right side of Xcode, choose ```Show the File inspector``` > ```Localization``` > Press button: ```Localize...```

<a href="https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-23-at-9-57-32-pm.png"><img class="alignnone size-medium wp-image-227" src="https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-23-at-9-57-32-pm.png" /></a>

Step 5: Setup your build:

- Edit your sheme

<a href="https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-23-at-10-00-40-pm.png"><img class="alignnone size-medium wp-image-227" src="https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-23-at-10-00-40-pm.png" /></a>

- Change your region + language that you want

<a href="https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-23-at-10-01-07-pm.png"><img class="alignnone size-medium wp-image-227" src="https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-23-at-10-01-07-pm.png" /></a>