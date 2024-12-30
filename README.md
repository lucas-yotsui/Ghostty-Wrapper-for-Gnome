# Ghostty Wrapper for Gnome
## The wrapper
This is a very simple script that allows for Ghostty to be used to launch terminal-based applications on Gnome.

When a user clicks on one of the icons in Gnome, apparently it calls something like:
> `<TERMINAL_EMULATOR> -- <PROGRAM_BEING_LAUNCHED>`

To launch Ghostty with a program, the user calls it with the`-e` flag, which makes it so that Ghostty starts by calling `/bin/sh -c <ARGUMENTS PASSED>`. 
Because of this flag, Ghostty's executable can't be used directly as the launcher for these applications, requiring some sort of wrapper that adds the `-e` flag.

However, by simply having the wrapper take all of the arguments passed to it and forward them to `ghostty -e`, these two behaviors are put together and the following call is made:
> `ghostty -e -- <PROGRAM_BEING_LAUNCHED>`

Which starts Ghostty running:
> `/bin/sh -c -- helix`

Aaaaaannddd, this crashes Ghostty 😃

So basically to fix this, all this script does is strip the first argument passed to it (**--**) and forward the rest to `ghostty -e`

## The AUR package
The AUR package does a bit more ["gambiarra"](https://www.linkedin.com/pulse/how-explain-gambiarra-gringo-stefan-feh%C3%A9r-zilenovski) as we say in Brazil...

Having the wrapper ready, I now needed to make it the default way to launch terminal-based applications on Gnome. 
However, since Gnome deprecated the use of the **org.gnome.desktop.default-applications.terminal** setting and didn't bother to provide an alternative, this became quite a hard task.

After scouring the internet looking for the proper way to do this, I've found [this brilliant answer](https://askubuntu.com/questions/111592/how-do-i-set-the-default-terminal-in-gnome/1262935#1262935) in a forum. 
It's not the most elegant way of accomplishing what I needed, but it works!

Well, that's most of what this package does on install (and why it conflicts with gnome-terminal 🤪). Basically, I symlinked this script to the first terminal I'm sure Gnome would try to use: _their own_.
I've also put a desperate attempt at doing things the right (and deprecated) way by keeping the line `gsettings set org.gnome.desktop.default-applications.terminal exec ghostty` in the install script.

Besides that, for those that have the nautilus-open-any-terminal package installed, I've also put in the line `gsettings set com.github.stunkymonkey.nautilus-open-any-terminal terminal ghostty || true` to set Ghostty as the default terminal emulator for that (and the `|| true` part makes it so that this command does not fail the whole install for users who don't have it)

## That's it!
And that's all there is to this project, I honestly believe that no one other than myself will use it and I've only done it to help me setup my fresh Arch installs more easily.
If this helps anyone else in any way I'll be extremely happy, feel free to open issues and pull requests if you find a way to turn this piece of junk into something more decent 😁.

On that same note, since I use Arch _(btw)_, I only made a package for it, but I guess people using other distros would also benefit from this.
So please if you know how to create packages for your distro and would like to help this monstrosity, I'd really appreciate it!

Honestly, in my perfect world, something like this would be done by the Ghostty package itself. 
I'll open a suggestion there, hopefully they agree that this is somewhat useful and take inspiration to make something more elegant to accomplish this same goal.
