---
author: Chris Nicola
date: '2011-07-15 23:04:53'
layout: post
slug: vim-from-start-to-finish
status: publish
title: Vim from start to finish
comments: true
wordpress_id: '593'
categories:
- blogging
- VIm
---

I think I've finally hit a bit of a sweet spot with VIm, it's grown from
something that I wanted to conquer almost partially out of my frustration with
it to a tool I use for almost every editing and writing task I have. I code
with it (I do still use VS for .NET work but I have VIEmu to smooth that out).
I edit ordinary documents, letters, etc. with it. I have plugins in Firefox and
Chrome that emulate it. [An intervention may be needed before something like this happens][1].

Today, I've managed to even get blog editing working with it using a plugin
called [vim-repressed][2]. So, since I'm riding a bit of a VI high today, I
figured I would do my best to put together some tips, tricks and suggested
starter plugins for anyone foolish enough to follow me into this madness.

<!--more-->

## 1. Pathogen + Github == Awesome Sauce!

The biggest impediment to getting started with VIm, is that it is not exactly
known for being plug-and-play. It's default settings are pretty worthless and
thus it is left as an exercise for the user to get it all set up how they like.
Getting VIm setup exactly how you want can be a bit tedious even for VIm
veterans, so for a newbie it is downright daunting and usually the reason most
people shy away. But don't fret, there is help and it's name is Github.  


Because of how much work goes into getting all your settings and plugins just
right VIm users need a convenient way way to keep track of what they did.
Keeping all your plugins and settings under version control is arguably the
best way to do this. Dropbox is also an option, but I'm going to show you why I
think using Github, pathogen and submodules is even better.  


Pathogen is a plugin that allows you to keep all of your VIm plugins in
separate folders (one of VIm's early mistakes was to put them all in root
folders often spreading a plugin in ways that made it hard to find a remove
later. Pathogen solves this and automatically loads plugins from any folder it
finds under `/bundle`. The way I like to use this is to setup an empty `.vim`
folder, install pathogen under `/autoload` and then use Github and `git
submodule add` to install each plugin directly into `/bundle` from Github.

The basic structure looks like this:
    
    .vim
    |
    |`vimrc           # shared vim settings
    |`autoload        # plugins automatically loaded by VIm
    | |
    |  `pathogen.vim
     `bundle          # plugins automatically loaded by pathogen
      |
      |`plugin1
      | |
      | |`plugin      # The standard VIm plugin folders for each plugin
      | |`syntax      # go under the plugin name instead of at the root.
      | |`indent      # Each plugin can be cloned from Github as a submodule
      |  `... 
      |`plugin2
       `-...
    

Each plugin is included as a git submodule from it's git repository. From what
I can tell almost every VIm plugin worth looking at is up on Github, so
installing a plugin is as easy as:
    
    git submodule add [url to submodule] bundle/[submodule name]
    
After that pathogen does all the work. Obviously you don't have to use
submodules, but this makes it easy to update your plugins with new revisions
from Github, as well as revert them if a revision happens to be broken .

Many git users have already got a decent dotvim folder up on Github that you
can use theirs as a starting place. Please feel free to [fork mine][3] as a
starting point. I have a special branch for Win32 as I've occasionally found
plugins (jslint is one of them) that did not work out-of-the-box and required
me to modify them slightly.

## 2. My favorite things

Once again, you can check my dotvim on Github to see which plugins I'm using
but to give a quick TL;DR overview of some of the most useful here is what I'm
using:

  * NERDTree: Provides a filetree navigation and manipulation similar to the solution explorer (but better)
  * NERDCommenter: Provides quick comment/uncomment functionality in many languages
  * Command-T: File searching inspired by TextMate
  * supertab: Beefs up the tab key to do auto completion similarly to VS and other IDEs (can be both a blessing and a curse)
  * vim-PairTools: Pairs up quotes, braces, etc. auto-magically
  * snipmate: Has similar functionality to TextMates snippets
  * jslint, jsbeutify, vim-javascript: Cause everyone uses javascript
  * endwise: Closes code blocks in multiple languages like Ruby, VimScript, etc.
  * vim-surround: Similar to PairTools but allows more flexibility to surround with just about anything
  * vim-git: Plugins for editing git files
  * fugitive: Git integration for Vim (I've never got this working all that well)
  * vim-ragtag: HTML/XML tag support
  * vim-haml, vim-rails, vim-rake: Cause Ruby on Rails yo...
  * gist-vim: I have not tried posting a gist from VIm, but it seemed like a cool idea

I have a couple more but these are easily the most important ones for anyone
doing development (especially web development) with VIm.

## 3. Actually learning to use VIm

There is no substitute for just making yourself use the thing. However this can
be a bit of a challenge to start on during work hours, so I recommend picking
up an easy pet project (or 7 languages in 7 weeks) and learning it while doing
that. If you are a .NET developer I highly recommend (no insist) that you
choose something to work on that is NOT.NET(tm). VS is still basically a pretty
great editing experience until you gain full proficiency as a keyboard jockey
(and arguably a pretty good one even after the fact) and you will find the lack
of intellisense when working in something like C# a bit annoying at first.
However if you are working in something that typically has no intellisense
anyways you will see clearly the power of this **fully functional text
manipulation station** _oh I'm afraid the regular expresion navigation will be
quite operational when you're friends arrive_... *ahem*.

I personally find it an invaluable tool with HTML and Javascript editing as
there are not many editors that do a good all-around job in this area (though
it should be said that R#6 has vastly improved VS' ability to edit javascript).

Here are some excellent resources:

  * [VimCasts][4] - You can't complain about free
  * [VIEmu Cheatsheet][5] - Not just for VIEmu
  * [Vim Recipes][5]
  * [VimTips (Twitter)][6]

VIm has been around for over 20 years, so there are many, many more resources
if you Google around for them.

### My personal tips for starting out:

  1. First, before you yank out your hair, learn to yank text: `y` (yank) and `p` (paste) are your new copy/paste.
    * `yy` will yank the current line without first selecting anything.
    * `P` will paste before the cursor position instead of after it like `p` does.
    * Switch to visual mode `v` or visual line mode `V` to make text selections
  2. Start by learning to get used to `h,j,k,l` for navigation but don't sweat it. Many people fall back to arrow keys from time to time and character-by-character navigation is not VIm's [real ultimate power][7].
  3. Try out navigating forwards and backwards by entire words rather than characters using `w, b, e`
  4. Master the power that is search based navigation: 
    * `f,F,t,T` allow you to quickly navigate to any character in a line (`f` - find, `t` - until)
    * `*` navigates to other occurrences of the word you are at
    * `/,?` let you search the entire document for an expression (supports regex)
  5. Learn the many ways to enter insert mode
    * `cw` will delete the rest of the word from your cursor and enter insert mode (good for replace a word)
    * `C` will delete the rest of the line from your cursor and enter insert mode
    * `A` will take you to the end of the line and insert insert mode
    * `I` will take you to the beginning of the line 
    * This list goes on for a while...
  6. Start using visual `v`, visual line `V` and visual block modes `ctrl-v` to select and manipulate text.
  7. Practice search and replace with `:s/<search>/<replace>/g` check [here][8] for more options.
  8. Recognize that this is but the smallest tip of the iceberg, sit down, have a good cry and then start Googling for new things to learn.

It is really impossible to sum up VIm's capabilities in a blog post so I won't try. I hope I've at least highlighted some of the best beginner features to whet your appetite to learn more though.

## 4. I like pretty colors

I've finally settled on a color scheme I like for just about everything. It's
called [Solarized][9], it is extremely easy on the eyes, provides good all
around contrast and has both light and dark variants. I've even created my own
VS themes based on the scheme and put them up on [studiostyl.es](http://studiostyl.es),
here is the [light][10] and the [dark][11] variants. The author's Github also
has VS themes someone contributed, but I like my variant a little better.

You can get gVIm, VIm and MacVIm setup with Solarized following the
instructions on the authors site. Or you can just see how I've got it set up in
my [dotvim][3] as always. If you are using Solarized with VIm from a bash
console the author recommends setting up your console colors to use the scheme
too (and so do I).

Yes I know it's a bit weird for a partly colorblind man to be so obsessed with
the colors on his terminal.

Well that's it folks, I hope this is helps a few more people discover the
goodness that is VIm editing, and saves them from the dark path to hell that is
Emacs (I kid, I kid).

Have fun!

   [1]: http://symbolsystem.com/2010/12/15/this-is-your-brain-on-vim
   [2]: http://www.vim.org/scripts/script.php?script_id=3510
   [3]: http://github.com/lucisferre/dotvim
   [4]: http://vimcasts.org/
   [5]: http://www.viemu.com/a_vi_vim_graphical_cheat_sheet_tutorial.html
   [6]: https://twitter.com/#!/vimtips
   [7]: http://www.realultimatepower.net
   [8]: http://vim.wikia.com/wiki/Search_and_replace
   [9]: http://ethanschoonover.com/solarized
   [10]: http://studiostyl.es/schemes/nsolarized-light
   [11]: http://studiostyl.es/schemes/nsolarized

