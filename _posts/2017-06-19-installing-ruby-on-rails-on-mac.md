---
layout: post
title: Installing Ruby on Rails on Mac
share: true
categories: How-To Ruby OSX
---

If you're new to Ruby on Rails, or perhaps it's been too long since you've set up a new install, here's a quick and easy way to set up Ruby and Rails on your computer. I've also included some common aides in the process, such as Homebrew and RVM. If you encounter any errors along the way, feel free to drop me a line.

## Upgrade OSX
First things first: if you're going to be doing a fresh install of Ruby on Rails, now would be an excellent time to run all necessary updates on your Mac. You can check if you have the latest updates by accessing **Apple menu > About This Mac > Overview > Software Update...**. This will load the App Store and check for any available OSX updates.

## Install XCode Developer Tools
XCode is the Apple suite of tools and compilers necessary for doing development on a Mac. To install XCode, open a new terminal window and type:
```
  $ xcode-select --install
```
Once that has installed, you'll want to check to make sure the install was successful.
```
  $ gcc -v
```
If the above command returns something like `Apple LLVM version X.X.X`, then you should be good to go.

## Install Homebrew
Homebrew is the most common package manager for Mac. With Homebrew you can easily install frameworks quickly and easily. You'll want to run the command below:
```
  ruby -e "$(curl fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
This may take a while to install, but once it completes, run:
```
  brew doctor
```
You should receive `Your system is ready to brew.`. If you get any errors, you may have non-Homebrew installed packages or other problems.

## Install RVM for Ruby version management and Rails
RVM (Ruby Version Manager) allows you to install multiple rubies (versions) as well as multiple gemsets on your machine.
```
  $ gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
```
followed by
```
  $ \curl -sSL https://get.rvm.io | bash -s stable --ruby --rails
```
Once install is complete, run `ruby -v` to check Ruby install and version, and `rails -v` to check Rails.


## Install Bundler
Bundler manages your Ruby gems for each applicaiton you create. We'll want to install it globally, so you have access to it with each new application.
```
  $ rvm gemset list
```
You should be on `(default)`. If not, run `rvm gemset use default` to switch back. Now run:
```
  $ rvm @global do gem install bundler
```

## A quick note on RVM
If you haven't used RVM before, it can be a little tricky. A good rule of thumb is to keep each application's gems in a separate gemset so things don't get mixed up between versions or applications.

To view your current gemset:
```
  $ rvm gemset list
```
To create a new gemset:
```
  $ rvm gemset create your_gemset_name
```
To switch gemsets:
```
  $ rvm gemset use other_gemset_name
```

## Final Words
Ruby on Rails is an amazingly powerful framework. It's fantastic to learn, and fun to use. Once you get the initial configurations complete, you'll be able to create applications in no time!
