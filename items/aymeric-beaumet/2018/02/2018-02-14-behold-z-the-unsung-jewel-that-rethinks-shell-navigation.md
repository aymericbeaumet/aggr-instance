---
title: Behold z, the unsung jewel that rethinks shell navigation
link: https://aymericbeaumet.com/behold-z-the-unsung-jewel-that-rethinks-shell-navigation/
source: aymeric-beaumet
published: 2018-02-14T00:00:00Z
updated: 2018-02-14T00:00:00Z
first_seen: 2026-09-02T20:26:23.728357669Z
summary: 'I spend a fair amount of time wandering around in the terminal. The chances are that you are too since you dared to open this story. My daily activities led me to contribute to various kinds of projects (professional, personal and open-source). For my sanity, I tend to follow strict rules to organize my workspace, which helps me keep things tidied up and easily reachable. Or so I thought. I decided to arrange my projects by following the Go philosophy: make the arborescence mirrors the urls where they are reachable. e.g., ~/Workspace/src/github.com/aymericbeaumet/dotfiles would contain the project dotfiles owned by @aymericbeaumet and accessible at github.com. That’s great, but I am now facing a complex directory structure tedious to navigate quickly. Making it, in the end, harder to access the resources I need.'
content: feed
html: 2018-02-14-behold-z-the-unsung-jewel-that-rethinks-shell-navigation.html
---

I spend a fair amount of time wandering around in the terminal. The chances are that you are too since you dared to open this story. My daily activities led me to contribute to various kinds of projects (professional, personal and open-source). For my sanity, I tend to follow strict rules to organize my workspace, which helps me keep things tidied up and easily reachable. Or so I thought.

I decided to arrange my projects by following the [Go philosophy](https://golang.org/doc/code.html#Workspaces): make the arborescence mirrors the urls where they are reachable. e.g., `~/Workspace/src/github.com/aymericbeaumet/dotfiles` would contain the project [dotfiles](https://github.com/aymericbeaumet/dotfiles) owned by [@aymericbeaumet](https://github.com/aymericbeaumet) and accessible at [github.com](https://github.com). That’s great, but I am now facing a complex directory structure tedious to navigate quickly. Making it, in the end, harder to access the resources I need.

```
$ pwd/Users/aymericbeaumet/Workspace/src/github.com/brigad/back-end
```

*Nobody wants to manually deal with this.*

Remembering and typing the location of each project (even while leveraging autocomplete) is time-consuming. Creating shell aliases is not scalable. Instead, I would like to rely on the pseudo-unicity of each project name to find it among all the directories on my computer.

## [](https://aymericbeaumet.com/behold-z-the-unsung-jewel-that-rethinks-shell-navigation/#z)z?

Several tools exist to tackle this problem. My favorite is [*z*](https://github.com/rupa/z). It follows the Unix philosophy by being small, focused and interoperable. *z* allows jumping around swiftly, automatically learning from your habits. The entries are ranked based on the [frecency](https://en.wikipedia.org/wiki/Frecency) at which you access the directories.

Let’s walk through the basics of this tool. Below is how I advise to install it for Zsh on macOS (the install process should be similar for other shells and operating systems):

```
$ brew install z$ echo "source '$(brew --prefix z)/etc/profile.d/z.sh'" >> ~/.zshenv
```

*Install z for Zsh on macOS.*

Note how installing *z* is not enough to start using it. You have to source `z.sh` at the beginning of each shell session to activate it. The `~/.zshenv` configuration file is appropriate as Zsh [loads it in most circumstances](http://zsh.sourceforge.net/Intro/intro_3.html), making the tool accessible in a wide-variety of contexts (terminals, editors, IDEs, scripts, etc.).

## [](https://aymericbeaumet.com/behold-z-the-unsung-jewel-that-rethinks-shell-navigation/#lets-play)Let’s play

Once the install and setup are complete: start a new shell session, `cd` through some folders and execute `z` to confirm it has been accurately tracking your activity:

```
$ z | sort -rn | head -35331.72    /Users/aymericbeaumet/Workspace/src/github.com/brigad/back-end637.255    /Users/aymericbeaumet/Workspace/src/github.com/brigad/ops386.198 /Users/aymericbeaumet/.config/dotfiles
```

*Top 3 directories based on frecency.*

As shown above, I spend quite some time navigating through [Brigad](https://github.com/brigad) projects, but it seems I also often access my [dotfiles](https://github.com/aymericbeaumet/dotfiles) to tweak some configuration. The ranking score is volatile, and one could observe significant changes in a brief amount of time.

Now that *z* has accumulated some knowledge try it by navigating to one of the tracked folders. Navigating is as simple as executing the `z` command followed by the directory name:

```
$ z dotfiles$ pwd/Users/aymericbeaumet/.config/dotfiles
```

*Navigating by matching with a whole directory name.*

As a matter of facts, I do prefer to leverage partial matching (imagine `**` surrounding your search pattern) to type fewer characters. Hence, `z dot` is valid and should produce a similar behavior as `z dotfiles` (modulo possible name collisions, but you get the idea). This works because *z* relies on regular expressions. For example, the snippet below will navigate to the directory ending with the letters “tmp” (with the highest score):

```
$ z tmp$$ pwd/private/tmp
```

*Navigating by matching with a regular expression.*

Neat right? I guess you start seeing the possibilities of this tool. And we have only scratched the surface of its capabilities. I suggest you dive in its [documentation](https://github.com/rupa/z/blob/master/README) to get an idea of its full potential. Features such as autocomplete, subdirectories matching, consecutive matching, etc. are a game changer.

## [](https://aymericbeaumet.com/behold-z-the-unsung-jewel-that-rethinks-shell-navigation/#conclusion)Conclusion

*z* is tremendous. This tool saves me a non-trivial amount of time every single day. When one starts relying on it, there is no going back. And I hope it’s going to fit your workflow as well as mine.

I would be grateful to hear any of your time-saving tricks. Feel free to share your experiences!
