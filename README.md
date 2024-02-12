# Alacritty Auto Theme Switcher

## About
[Alacritty](https://alacritty.org/) is a "fast, cross-platform, OpenGL terminal emulator" with [many themes available](https://github.com/alacritty/alacritty-theme), this project allows Alacritty theme to automatically change to a light/dark theme based on the current system theme. This only works for systems running Gnome and systemd (happy to receive contributions for other systems). 

This is not affliated/supported by the Alacritty team, please report issues here and not to the Alacritty project.

### Background
Alacritty provides the ability for an user to define themes (or override parts of a theme). All of this is achieved via the `alacritty.toml` configuration file. With the switch to TOML, Alacritty allows the import of other `.toml` files that have themes defined, so it's easy to keep configuration separate from the current theme. Also if any of the configuration files are updated, the terminal windows will auto-reload (if `live_config_reload = true` is set). This functionality mkakes it possible to import a `theme.toml` file and update the contents of the file and have Alacritty change themes live. Unfortunately, Alacritty does not automatically follow the current system theme preference. 

## Installation

### Normal
There are many ways to [install Alacritty](https://github.com/alacritty/alacritty/blob/master/INSTALL.md) and they also have [instructions for installing themes](https://github.com/alacritty/alacritty-theme?tab=readme-ov-file#installation) that are maintained by the Alacritty team. Once Alacritty is installed and working with your chosen theme: 
``` sh
cd ~/.config/alacritty/
git clone https://github.com/shombando/alacritty-auto-theme.git
```

Now you can jump to the [Configuration](#Configuration) section.

### Git submodule
If you version control your Alacritty configuration as part of your dot files:
``` sh
cd ~/.config/alacritty/
git submodule add https://github.com/shombando/alacritty-auto-theme.git
git submodule update
git submodule init
```
Consider making a fork of this repo so I ever change the default light/dark themes my changes don't overwrite your selection. I could not make those files part of the repo but wanted to make it easier to just install and try with minimal config.

## Configuration
Edit both the `light_theme.toml` and `dark-theme.toml` file and set it to one of the options from the themes directory (or paste in your color scheme directly in light/dark file) and include the following in your main `alacritty.toml` file:
```toml
import = [
    "~/.config/alacritty/alacritty-auto-theme/theme.toml"
]
```
>NOTE: Make sure that is the only time you're importing a theme. If you followed the directions for the alacritty-theme installation make sure you don't have that import still in your `alacritty.toml` file

Then we'll install the systemd service, by opening a terminal in this current folder and running the following:
``` sh
cd ~/.config/alacritty/alacritty-auto-theme
cp ./AlacrittyAutoTheme.service ~/.config/systemd/user/
systemctl --user enable AlacrittyAutoTheme.service
systemctl --user start AlacrittyAutoTheme.service
```
That's it, now when you switch your system theme, all Alacritty windows will also switch the respective light/dark themes you picked.

## Alternate Usage
Since this script is dependent on Gnome and systemd if you want to use it "manually" you can create an alias for switching to your preferred theme by calling `alacritty-light` or `alacritty-dark`: 
``` sh
alias alacritty-light="echo \"import = [ '.~/.config/alacritty/alacritty-auto-theme/light_theme.toml' ]\" > ~/.config/alacritty/alacritty-auto-theme/theme.toml"
alias alacritty-dark="echo \"import = [ '.~/.config/alacritty/alacritty-auto-theme/dark_theme.toml' ]\" > ~/.config/alacritty/alacritty-auto-theme/theme.toml"
```

