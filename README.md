### Installing NPM Modules Without Sudo ###

OK, so the whole installing `npm` modules with `sudo` is beginning to screw me over, and that is without even factoring in the safety concerns regarding using root to install them. I have found some resources on the Internet that helped me to remedy this.

#### Removing Installed Global Modules ####

In essence what you need to do is reset the global module directory to somewhere in your home directory. It is currently pointing to `/usr`, which is owned by root, and hence you need to use `sudo` to install modules (whether locally or globally). But before you do, you’ll need to remove any global modules you’ve installed to date. This will be of the form:

```sh
sudo npm rm -g learnyounode bower karma-cli selenium-standalone grunt-cli mocha
```

don’t forget to add any others you may have installed outside the course. You can check which ones are still installed with `npm ls -g` (you are only concerned with the packages in the highest level (i.e. furthest left); in other words, only remove the packages you recognise). **DO NOT REMOVE NPM!**

Running npm as root will also have locked some of the directories in your `~/.npm` directory, making it impossible to install or remove modules without `sudo` in the future. The best way to resolve this is to remove this directory (we’ll be installing the modules again in a few moments):

```sh
sudo rm -r ~/.npm
```

**WARNING! Dangerous command! Watch your spelling!**

#### Moving the Global Modules Directory ####

Next use:

```sh
npm config set prefix ~/.local/share/npm-modules
```

(or, if you prefer):

```sh
npm config set prefix ~/.npm-modules
```

(or anything in `$HOME`) to set your local global module storage directory (yes, I know that’s an oxymoron… :-Þ).

#### Reinstalling Previously Installed Global Modules ####

Now you can reinstall your global packages to the new directory (**DO NOT USE SUDO!**):

```sh
npm install -g learnyounode bower karma-cli selenium-standalone@latest grunt-cli mocha
```

#### Reclaiming Local Module Configuration ####

For each project you’ve used `npm` in, navigate to the project’s root directory and run:

```sh
sudo rm -r node_modules
```
**WARNING! Dangerous command! Watch your PWD when doing this!**

Then you need to grant yourself access to the `package.json` file that root kindly locked for you:

```sh
sudo chown mark:mark package.json
```

(change `mark:mark` to your login name). Finally run `npm install` to restore your local modules!

#### Sources ####

[StackOverflow – NPM Modules Won’t Install Globally Without Sudo.](http://stackoverflow.com/a/21712034 "StackOverflow – NPM Modules Won’t Install Globally Without Sudo.")
