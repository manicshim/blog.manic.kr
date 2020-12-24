---
title: "Redis Desktop Manager on Mac OSX"
comments: true
date: 2018-11-20 14:13:08
categories: ["developer","database"]
tags: ["redis","db","database"]
thumbnail: https://user-images.githubusercontent.com/22788288/48998860-2ac9ee80-f198-11e8-9234-b9413b27630f.png
---
### Redis Desktop Manager on Mac OSX
* Install the App
	1. Press `Command+Space` and type **_Terminal_** and press **_enter/return_** key.
	2. Run in Terminal app:
		```
		ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)" < /dev/null 2> /dev/null ; brew install caskroom/cask/brew-cask 2> /dev/null
		```
		and press **_enter/return_** key. 
		If the screen prompts you to enter a password, please enter your Mac's user password to continue. When you type the password, it won't be displayed on screen, but the system would accept it. So just type your password and press ENTER/RETURN key. Then wait for the command to finish.
	3. Run:
		```
		brew cask install rdm
		```

Done! You can now use **Redis Desktop Manager**.