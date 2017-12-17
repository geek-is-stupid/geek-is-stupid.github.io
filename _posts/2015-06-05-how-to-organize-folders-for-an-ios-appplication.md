---
layout: post
title: How to organize folders in your iOS applications?
categories: [ios]
tags: [swift, xcode, ios, ornanize, folders]
fullview: true
---

With my experiences, here are my persional ideas to apply for MVVM pattern.

##### Application Structure:

###### Project:

- Engine: Shared classes, enums, structures, or logic, business on backend, networks, parsers, helpers, caches, data store, etc…
	- Manager: Contains Singleton class to handle Application product
		- Authentication manager
		- Push notifications manager
		- REST manager files
		- …
	- Constant:
		- AppConstants file
		- RESTConstants file
		- Constants file
		- …
	- Utility:
		- AppUtils file
		- RESTUtils file
		- …
	- Config: Contains everything contribute for configuration app
		- AppConfig file: Configure all setting in app: Feedback, geting started with third party libraries, configure server’s hosts, configure logger
		- …
	- Extension:
		- Extend class / struct / enum
		- …
	- Service:
		- ApiService: Call API
		- SystemService: Handle system service (Push Notification, …)
		- …
	- Persistence: Store entities here
- ViewModule: Every module contains View, ViewModel & anything related to front end.
	- Component: The views that they will be share between multiple screens. With each screen should be has a folder for itself
	- View: Represent for the screens. With each screen should be has a folder for itself
		- E.g. Feed:
			- `FeedViewController.swift` file
			- `FeedViewModel.swift` file
			- …
		- …
- Resource: Can be contains Images, Sounds, Layout config file, etc.
	- Assets.xcassets
	- Localizable.strings
	- …
- Storyboard: Put all storyboards at here. With each screen should be has a storyboard file for itself => Reduce conflicts when merge Pull Request
- AppDelegate.swift
- Info.plist

###### ExternalLibraries:
- All of your external libraries which could not include into your Podfile from some reasons

##### Naming Style:

The name of folder follows CamelCase style, and be Singular.