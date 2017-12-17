---
layout: post
title: How to Manage All Localized Strings in Your Project - Xcode?
categories: [ios]
tags: [swift, xcode, ios, localized strings, Localizable.strings, languages]
fullview: true
---

##### Using 'genstrings' tool from Apple which included into Xcode

###### Step 1. Generate Localizable.strings to get all of strings you want to localize.

- Open terminal and go to your project’s folder:

```swift
cd PathToYourProject
```

- Run:

```swift
find . -name '*.swift' -print | xargs -n1 genstrings -a
```

- After this step, a `Localizable.strings` will automatically generated

###### Step 2. Add Localizable.strings in your project.

![https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-23-at-9-47-21-pm.png](https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-23-at-9-47-21-pm.png)

###### Step 3: Add languages for your `Localizable.strings`:

- Now click on the file you just added `Localizable.strings`
- On right side of Xcode, choose `Show the File inspector` > `Localization` > Press button: `Localize...`

![https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-23-at-9-57-32-pm.png](https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-23-at-9-57-32-pm.png)

- One pop up will show to asks you which languages you wanna add, choose one please! 🤗

###### Step 4. Add languages which you want to localize to your project.

On the left side of Xcode, choose `Show the project navigator` > Choose your project:

![https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-23-at-10-13-36-pm.png](https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-23-at-10-13-36-pm.png)

###### Step 5: Setup your schemes:

- Edit your sheme

![https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-23-at-10-00-40-pm.png](https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-23-at-10-00-40-pm.png)

- Change your region + language that you want

![https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-23-at-10-01-07-pm.png](https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-23-at-10-01-07-pm.png)

# 🍻🍻🍻








