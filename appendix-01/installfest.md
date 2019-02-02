# Installfest

During this WDI course, we will be using a variety of different tools to develop web applications. However it is important that we all start off using similar technologies so that we can focus on learning concepts and code.

### Pre-install

1. Identify which version of your mac's Operating System you're using - ideally you should have Sierra or higher (10.12.x)
2. Ensure you've got [Xcode installed](https://itunes.apple.com/us/app/xcode/id497799835?ls=1&mt=12)
3. Ensure that you've uninstalled any antivirus software you may have, as it can prevent some of the tools from installing properly

<br>

# Section 1: Code Editor and Linter setup

#### Atom

During this course we'll be using Atom as our text editor of choice. It's similar in many ways to Sublime Text, but it has a nice user interface and a lot of 3rd-party plugins and packages that we can use to help us code more efficiently, and hopefully make fewer typos along the way.

#### Eslint

Eslint is a package designed to check for bugs in our code **before** we run it. Atom has a package called atom-linter that we can configure to work with eslint to check our code _as we type_. We'll set up this next.

## Atom

1. Download [Atom](https://atom.io/).
2. Copy it into your `Applications` folder, and add it to your dock.
4. Open Atom and click on the `Atom` menu then `Install Shell Commands`
5. Un-check the box on the left hand panel `Show Welcome Guide when opening Atom`
6. Type <kbd>cmd</kbd> + <kbd>,</kbd> to open the preferences. Click on the _Themes_ tab and set the UI theme **and** the syntax theme to _One Light_
6. In the _Core_ panel, make the following change:
    - **Exclude VCS Ignored Paths** - _uncheck_
6. In the _Editor_ panel, make the following changes:
    - **Show Indent Guide** – _check_
    - **Show Invisibles** - _check_
    - **Tab Type** > _soft_
7. Copy and paste the following command into the terminal:
  ```
  apm install auto-indent atom-ternjs file-icons language-ejs language-babel linter linter-erb linter-csslint linter-eslint linter-js-yaml linter-ruby open-in-browser ruby-block pigments sublime-style-column-selection
  ```
8. Now quit Atom.


<br>

## Create an `.eslintrc` file

We're going to define a set of rules for `eslint` to use. From the terminal, run the following command:

```sh
atom ~/.eslintrc
```

You might come across some blue notifications on the right hand side, just say `yes` to all of the installations

Cut and paste the following text into the file you just opened in Atom:

```json
{
  "env": {
    "browser": true,
    "commonjs": true,
    "es6": true,
    "node": true
  },
  "plugins": [
    "react"
  ],
  "extends": ["eslint:recommended", "plugin:react/recommended"],
  "globals": {
    "angular": true,
    "$": true
  },
  "parser": "babel-eslint",
  "parserOptions": {
    "sourceType": "module",
    "ecmaVersion": 7,
    "ecmaFeatures": {
      "jsx": true
    }
  },
  "rules": {
    "brace-style": "error",
    "camelcase": ["error", { "properties": "never" }],
    "comma-dangle": ["error", "never"],
    "func-call-spacing": ["error", "never"],
    "eqeqeq": "warn",
    "indent": [ "error", 2, { "SwitchCase": 1 } ],
    "key-spacing": [ "error", { "beforeColon": false } ],
    "no-console": "off",
    "no-fallthrough": "warn",
    "prefer-const": "error",
    "quotes": [ "error", "single" ],
    "semi": [ "error", "never" ],
    "react/prop-types": [ 0 ]
  }
}
```

And now **save the file** in Atom with <kbd>cmd</kbd> + <kbd>S</kbd>.

## Auto Indent

Keeping your code indented is so important and because of this we are going to give you a shortcut to automaticly indent all code in the file you are viewing.

Whilst on atom:

- click on "Atom" in the topbar and select `Keymap...`
- Add this to the bottom of the file

```yml
'atom-text-editor':
  'cmd-shift-r': 'editor:auto-indent'
```
- Save the file

Now <kbd>cmd</kbd> + <kbd>shift</kbd> + <kbd>R</kbd> will be the shortcut to auto indent!

<br>

## Linter config

Back in Atom's preferences

- Click on the packages tab and search for the `linter-eslint` and click on "Settings" for this package.
- Find where it says `.eslintrc Path`, set it to `~/.eslintrc`
- Make sure the box for `Disable when no ESLint config is found` is **unchecked**
- Set `Global Node installation Path` to `/Users/username/.config/yarn/global`, replacing `username` with your username. If you are not sure what that is ask your instructor for help.
- Check the `Use global ESLint installation` checkbox

**Now quit Atom**

<br>

# Section 2: The Terminal

The Terminal is a Command Line Interface that allows you to interact directly with your laptop's operating system. You can use it to run programmes, manage fils and folders and install software. Because we'll be using it so much on the course, we'll start by upgrading the default terminal with some extra commands and features.

## Command Line Tools

Xcode is a large suite of software development tools and libraries from Apple. The Xcode Command Line Tools are part of XCode. Installation of many common Unix-based tools requires the [GCC compiler](https://en.wikipedia.org/wiki/GNU_Compiler_Collection). The Xcode Command Line Tools include a GCC compiler.

Open the Terminal by hitting <kbd>cmd</kbd> + <kbd>space</kbd>, then type Terminal and hit <kbd>enter</kbd>

1. At the terminal prompt type: `xcode-select --install`
2. Choose `Install` from the prompt then `agree` to the license agreement
3. If you receive a message saying `Can't install the software because it is not currently available from the Software Update server` it's probably because the command line tools are already installed. If so type `gcc` in the terminal. If you see `gcc: command not found` inform your instructor, otherwise, move on to the next step.
4. Type `sudo xcodebuild -license` and type in your password at the prompt.
5. Press enter, then `q`
6. Then on the next prompt, type `agree`

<br>

## Homebew Package Manager

Homebrew is a package manager for OS X.

#### What are packages?

Packages are bundles of source code distributed by developers of software, which can be compiled and installed on your machine.

#### Install

1. Copy and paste the following command into the terminal, then hit enter
  ```
  /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
  ```
2. Press `enter` at the prompt, and enter your password.
3. Once installation has finished type `brew doctor`. It should say `Your system is ready to brew.` If not inform one of your instructors.
4. Update Homebrew: `brew update`

<br>

## Zsh

#### What is a shell?

We will go into a bit more detail about the shell later on in the course but a shell is a very basic user interface for accessing an operating system's services.

#### bash vs zsh

Macs come shipped with a shell called 'bash' by default. Bash stands for **'Bourne-again shell'**, referring to its objective as a free replacement for the Bourne shell which was developed by [Steven Bourne](https://en.wikipedia.org/wiki/Stephen_R._Bourne).

We are going to use another shell called `zsh` because it has some extra features to make our web-development easier.

The American English pronunciation of Z is "zee", so Z shell rhymes with C shell, which sounds like seashell. `zsh` was also the login of the original developer Paul Falstad's Yale professor Zhong Shao.

#### Install

1. Type `brew install zsh`.
2. Type `zsh`. You will see a list of options. Type `0` to create a `~/.zshrc` file.
3. You should see a `%` instead of a `$` at the terminal prompt. Type `exit` and you should see the `$` again.
4. Type `which zsh` to determine where your new shell has installed. It should say `/usr/local/bin/zsh`
5. Type `sudo su` and enter your password. You should see a `#` instead of the `$` at the terminal prompt
6. Type `echo '/usr/local/bin/zsh' >> /etc/shells` to add `zsh` into the list of allowed shells, then type `exit`. You should see the `$` sign at the prompt again.
6. Type `chsh -s /usr/local/bin/zsh`, and enter your password.
7. Close and reopen your terminal application. This will enable `zsh` by default. You should see the `%` symbol at the prompt.
7. Type `echo $SHELL`. This should return `/usr/local/bin/zsh`.

<br>

## Oh-My-Zsh

Oh My Zsh is an open source, community-driven framework for managing your zsh configuration. Here is the link to the [Github](https://github.com/robbyrussell/oh-my-zsh).

The `PATH` environment variable is a colon-delimited list of directories that your shell searches through when you enter a command.

1. Type `sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"`.
2. Open the `.zshrc` file by typing `atom ~/.zshrc`
3. Find the line `# export PATH=$HOME/bin:/usr/local/bin:$PATH` It's right at the top of the file.
4. Remove the `#` and the space at the start.
5. Save the file and quit Atom.
6. Close your terminal and open a new one.

<br>

## Install `wget`

We will often download files from the internet in the command line. We can install a tool to help us do that:

``` sh
brew install wget
```

<br>

## Speed up your cursor

During the course, we will be doing a lot of navigating using our keyboards. By default, the speed of the cursor on a Mac is a little too slow. Let's increase the speed of the cursor by going to:

```
System Preferences > Keyboard
```

Set Key Repeat to `Fast` and Delay Util Repeat to `Short`, then close the Preferences panel.

<br>

# Section 3: Git and Github setup

## Install `git`

Git is the version control system that we will use throughout the course. It is one of the most powerful tools you will use as a developer.

1. This ensures we can upgrade Git more easily:
`brew install git`
1. Quit terminal and reopen.
1. Ensure you're not using "Apple Git". Type `which git` it should say `/usr/local/bin/git`
1. Configure your name and email address for commits (be sure to use the email address you have registered with Github):
``` sh
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```
1. Configure git to track case changes in file names:
```sh
git config --global core.ignorecase false
```

<br>

## Global `.gitignore`

There are a few files that we don't want Git to track. We can specifically ignore them by adding the files to a global `.gitignore` file.

Create a file in your home directory called `.gitignore_global` and configure git to use it for all repos, like so:

``` sh
touch ~/.gitignore_global
git config --global core.excludesfile ~/.gitignore_global
```

#### `.DS_Store` files

`.DS_Store` files are used by Mac OS X to store folder specific metadata information.  They are different for every mac, it means that they often cause conflicts in version controlled folders.

#### `node_modules`, `bower_components` & `.sass-cache`

In the same way, we want to never track the contents of our node\_modules, bower\_components or .sass-cache folder as these are not part of our source code and do not need to be committed.

Open the `.gitignore_global` file in atom:

``` sh
atom ~/.gitignore_global
```

And add the following lines:

```
.DS_Store
node_modules
.sass-cache
.env
*.log
```

**Save** and close the file.

<br>

## Configure SSH access to Github

GitHub is a web-based Git repository hosting service. It allows us to keep a remote version of our version-controlled projects. When we push and pull from Git, we don't want to always have to login to verify who we are. Therefore, what we can do is generate and use something called an SSH key. SSH keys are a way to identify trusted computers, without involving passwords.

1. First, we need to check for existing SSH keys on your computer. Open up your Terminal and type: `ls -al ~/.ssh`. If you get `No such file or directory` go to
the next step **Generate a new SSH key**, otherwise, go to step 5 **Add your SSH Ket to Github**.
1. Generate a new SSH key by typing `ssh-keygen -t rsa -C "your_email@example.com"`
1. You'll be prompted for a file to save the key, and a passphrase. Press enter for both steps leaving both options blank (default name, and no passphrase).
1. Add your new key to the ssh-agent:
`ssh-add ~/.ssh/id_rsa`
1. Add your SSH key to GitHub by logging into Github, visiting **Account settings** and clicking **SSH keys**. Click **Add SSH key**
1. Copy your key to the clipboard with the terminal command:
`pbcopy < ~/.ssh/id_rsa.pub`
1. In the **Title** field put something that identifies your machine, like _Mike's MacBook Air_
1. In the **Key** field just hit <kbd>cmd</kbd> + <kbd>V</kbd> to paste the key that you created earlier - *do not add or remove and characters or whitespace to the key*
1. Click **Add key** and check everything works in the terminal by typing:
  `ssh -T git@github.com` <br>

  You should see the following message:
  ```
  Hi mickyginger! You've successfully authenticated, but GitHub does not provide shell access.
  ```

<br>

# Section 4: Node & Python

## Install Node

Node is a JavaScript engine that works on the server side. We can use it to create full stack web applications, command line tools and various other tools and packages.

Although we'll not be using it directly until week 4, we'll install it now, so that we can use it to install some linter packages written with node.

We'll install node with `nvm` which allows us to easily manage and upgrade our node version. Then we'll install a node package manager called `yarn`, then finally the `eslint` packages we need.

## Install `nvm`

>**Note:** if you have installed NodeJS previously, you will have to uninstall it first. Ask an instructor for help with this.

Open a terminal window and paste the following command:
```
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.2/install.sh | bash
```

Once `nvm` is installed, list the available versions by typing:

``` sh
nvm ls-remote
```

And you will be shown a list of all the available versions of node.js. Install the latest version. At the time of writing that was `v9.4.0`.

``` sh
nvm install v9.4.0
```

If you type:

``` sh
node --version
```

You should see the last version number that you've installed.

<br>

## Install `yarn`

Yarn is a new package manager for node which has some nice improvements on the native one. Install it with `brew`:

```sh
brew install yarn
```

Check its working by typing:

```sh
yarn --version
```

You should see a version number

## Install `eslint` packages

Install the following eslint packages globally with yarn:

```sh
yarn global add eslint eslint-plugin-react babel-eslint
```

## Install Python 3

Python is a server-side language that we'll be using towards the end of the course. Your MacBook comes with Python 2.7 installed already, but we need to upgrade to Python 3.

We can install the latest version using Homebrew:

```sh
brew install python
```

This will install the latest version of python as `python3` and the package manager as `pip3`. We can remove the 3 from the end by making an alias in the `.zshrc` file. Open the file with atom:

```sh
atom ~/.zshrc
```

Then add the following lines at the bottom of the file:

```sh
alias python=python3
alias pip=pip3
```

<br>

# Section 5: Databases

We'll be using two different types of database on the course. One is a traditional SQL database called `Postgres`, the other a JavaScript based implementation known as `MongoDB`.

## Install Postgres

Install Postgres with Homebrew:

```sh
brew install postgresql
```

You can now start, stop and restart Postgres using the following commands:

```sh
brew services start postgresql
brew services stop postgresql
brew services restart postgresql
```

When you connect to Postgres for the first time, it will attempt to connect to a database that matches your MacBook username, so we will need to create that first:

```sh
createdb $(whoami)
```

Start Postgres, and connect to your newly created database using `psql`:

```sh
brew services start postgresql
psql
```

If everything has worked correctly you should see the following:

```sh
psql (11.1)
Type "help" for help.

mickyginger=#
```

Where `mickyginger` is _your_ username.

You can now exit `psql` by typing `\q` then enter, then stop Postgres running by typing:

```sh
brew services stop postgresql
```

## Install mongoDB

We'll be using mongo with Node. It's a non-relational (noSQL) database. Install it with `homebrew`:

``` sh
brew install mongodb
```

Run the following command to ensure you have a place for mongo to store the data for your databases:

``` sh
sudo mkdir -p /data/db && sudo chown -R $(whoami) /data/db
```
To check everything is working type `mongod` in the terminal. It will print a lot of stuff on the screen. After a few moments you should see this line at the bottom:

```
I NETWORK [thread1] waiting for connections on port 27017
```

Hit <kbd>ctrl</kbd> + <kbd>C</kbd> to stop the process running and quit the terminal.


<br>


# Section 6: GUI applications

Finally we need to install some more traditional Graphical User Interface applications to help us throughout the course.

## Google Chrome

We primarily use Chrome on this course. If you use other browsers, you can continue to do so in your spare time. However, on this course all lessons will expect you to be using Chrome.

1. Go to [https://google.com/chrome](https://google.com/chrome)
2. Click on `Download Chrome`
3. Go to the Downloads folder and run the `googlechrome.dmg` package
4. Drag the icon into the Applications folder
5. Add Chrome to your dock and open it
6. Select `set Chrome as my default browser`

<br>

## Slack

Slack has a native app available on the App store. If you haven't already, [sign up to Slack](https://www.slack.com/) online, and install the OS app.

Once it has downloaded you'll find it your the Applications folder. Drag it into the dock and launch the app.

<br>

## Insomnia

Insmonia is an app that will allow us to make requests to web servers, and visualise the responses that they make.

Download the app from [https://insomnia.rest/](https://insomnia.rest/), copy it into your Applications folder and add it to your dock.

<br>

## Closure

Don't worry, you won't have to remember how to do all of this. If you need to setup a new machine in the future, we can send you over these installation instructions!
