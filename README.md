# Flatiron School Environment Setup Resources

## Introduction

The purpose of this document is to provide Flatiron School instructors and
coaches with resources that will help them assist students with setting up their
local environment and troubleshoot any issues that arise.

NOTE: environment setup and troubleshooting issues change constantly. Please
help us keep this document current and useful. If you encounter an issue or
resource that isn't captured here, or if you find a new solution to an existing
issue, **please submit an issue or pull request to this repo**!

## Environment Setup Troubleshooting

### Installing GMP

Command: `brew install gmp`

Error: `fatal: Could not resolve HEAD to a revision...`

Fix: `git -C $(brew --repository homebrew/core) checkout master`

See: https://stackoverflow.com/questions/65605282/trying-to-install-hugo-via-homebrew-could-not-resolve-head-to-a-revision

---

Command: `brew install gmp`

Error: `No similarly named formulae found...`

Fix: `brew update-reset` may work

OR (from [this github thread](https://github.com/Homebrew/discussions/discussions/1160#discussioncomment-547009)):

```bash
rm -fr $(brew --repo homebrew/core)
brew tap homebrew/core
```

---

Command: `brew install gmp`

Error: `cannot install in homebrew on arm processor in intel default prefix`

Fix: `export PATH=/opt/homebrew/bin:$PATH`

### Installing Ruby on Mac

Command: `rvm install 2.7.4 --default`

Error: `No binary rubies available...Error running ‘__rvm_make -j16’`

Fix: Solution 2 from [this article](https://medium.com/flawless-app-stories/gyp-no-xcode-or-clt-version-detected-macos-catalina-anansewaa-38b536389e8d)

### Installing Ruby on WSL

Command: `rvm install 2.7.4 --default`

Error: `Error running '__rvm_make -j4' ubuntu 22.04`

Cause: In short, OpenSSL 3 is the only version of OpenSSL available to Ubuntu 22.04.
Ruby 3.1.0 was the first version shipped with OpenSSL 3 so older versions like Ruby
2.7.4 are incompatible.

Fix: 

- downgrade ubuntu to something like 20.04
- compile a custom OpenSSL 1.1.1 and compile Ruby against this custom OpenSSL (talked about in the link above)
- install ruby 3.1.0

See [this Stack Overflow thread](https://stackoverflow.com/questions/72179373/cant-install-ruby-via-rvm-error-running-rvm-make-j4-on-ubuntu-22-04)

### Installing ZSH

Error: Frozen terminal after installing ZSH (often with a `[process completed]` message)

Fix: with Terminal open navigate to Terminal > Preferences. Select "General" tab and  
click "Command" radio button under the "Shells open with:" section. 
Ensure `/bin/zsh` is being used there.

Cause: usually they already had ZSH installed at `/bin/zsh` but the
setup guide walks them through installing it at `/usr/local/bin/zsh` so the
terminal cannot find the right path when it starts up

---

Error: `Compinit insecure directories`

Fix: `compaudit | xargs chmod g-w`

### Cloning Labs

Command: `git clone <repo name>`

Error:  

(Mac) `ssh: connect to host github.com port 22: Operation timed out`

(Ubuntu) `Connection reset by 140.82.112.4 port22`

Fix:

- Create a `~/.ssh/config` file if they don’t already have one
- Add the following three lines, save the file, and try to clone again:

```bash
Host github.com
Hostname ssh.github.com
Port 443
```

### Running npm install

Command: `npm install`

Error: `command not found: npm`

Possible Fix:

- Run `nvm list` to ensure that a default version of node it set
- It might show `default -> lts/* (-> N/A)`, but because `lts/*` is also not set
  to a specific version then no version is currently set as default
- Run `nvm alias default <version-number>` (use one of the 
  versions listed out from the `nvm list` command)
- Quit the Terminal application and open a new one
- Then, either navigate back to the specific lab to test `npm install` again, or
  just run `npm` in any directory to check that the "command not found error"
  does not appear again

### Running bundle
Command: `bundle`

Error: `An error occurred while installing racc (1.6.0), and Bundler cannot continue`

Fix: `xcode-select install`

### Setting up Git/GitHub

Issue: Need to remove SSH passphrase

Fix: See [this resource](https://www.simplified.guide/ssh/set-remove-passphrase)

### Misc

Error (when running tests): `TypeError: Cannot set property 'isExternal' of undefined`

Fix: Delete local repo, re-clone, don’t run ‘npm audit fix’

### Installing Ubuntu

Error in Ubuntu app: `Error 0x80080005 Server execution failed`

Fix:

- Press `Windows key`+ R to open "Run" dialog. type: `optionalfeatures.exe` and hit `Enter`
- Scroll to the bottom and uncheck "Windows Subsystem for Linux". Click "OK".
- Repeat the above steps to re-enable Windows Subsystem for Linux.
- Then have them uninstall Ubuntu, (by searching for it in the Search bar or
  finding it in the Start menu, right clicking, and selecting “Uninstall”)
- Then, reinstall Ubuntu and have them continue with the steps they were working
  on
  
Explanation: Really just toggling the WSL setting off and on and reinstalling
Ubuntu

### Opening Ubuntu

When starting up Ubuntu for the first time after restart that is required from
enabling WSL settings:

Error: `WslRegisterDistribution failed with error: 0x80370102`

Fix: Try one of the solutions in [this article](https://appuals.com/wsl-register-distribution-error-0x80370102-on-windows-10/).
“Solution 1” worked for the one and only student that has encountered this error

---

Upon opening Ubuntu terminal:

Error: `404:: command not found`

Fix: open .bashrc file (`code ~/.bashrc`) and scroll to bottom or search the
file for that error text and just delete the text. Save the file. Close out of Ubuntu
and reopen to confirm that text doesn’t appear again.

### Ubuntu: Installing NVM:

Command: `curl -o- curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash`

Error: `0curl: (6) Could not resolve host: curl`

Fix: Navigate to current internet settings via Control Panel (Wifi, Ethernet, etc) and disable IPv6
[Step by Step Guide](https://networking.grok.lsu.edu/article.aspx?articleid=17573)

> Note: Googling around brings up [the fact](https://docs.microsoft.com/en-US/troubleshoot/windows-server/networking/configure-ipv6-in-windows) that permanently disabling these settings can cause other issues, 
> so maybe have them try to re-check that setting once things are installed - if this fix helped to get past the error of course.
> I have had other students disable this specific setting before to solve other errors, and never re-enabled it, so I think they should
> be fine as far as working through our curriculum with that disabled. But making sure the student remains aware of this disabled setting
> could be helpful for future issues they *might* encounter.

### Ubuntu: Trying to Push a Lab

Command: `git push ...`

Error:

```bash
Connection to github.com closed by remote host
Send-pack: unexpected disconnect while reading sideband packet
Fatal: the remote end hung up unexpectedly
```

Fix: have them switch to WSL 1 in place of WSL 2. Open Command Prompt and run:

```bash
wsl --set-default-version 1
wsl --set-version Ubuntu 1
```

### JS Lessons: Embedded Replit Editors Not Displaying in Lesson Page(s)

Make sure cookies are enabled!

## Flatiron School Local Environment Setup Lessons

### Initial Setup (Prework)

MacOS:

- [MacOS Environment Setup Introduction](https://github.com/learn-co-curriculum/phase-0-macos-env-introduction)
- [MacOS System Setup](https://github.com/learn-co-curriculum/phase-0-macos-env-system/blob/main/README.md)
- [MacOS NodeJS Install](https://github.com/learn-co-curriculum/phase-0-macos-env-nodejs)
- [Installing Ruby on MacOS](https://github.com/learn-co-curriculum/phase-0-macos-env-ruby) (Ruby/Rails curriculum)
- [Installing Python on MacOS](https://github.com/learn-co-curriculum/phase-0-macos-env-python) (Python/Flask curriculum)
- [MacOS Git and GitHub Configuration](https://github.com/learn-co-curriculum/phase-0-macos-env-git-github)
- [Configure the Flatiron Student Portal](https://github.com/learn-co-curriculum/phase-0-configuring-the-flatiron-student-portal) (deprecated starting with 2023 cohorts)
- [MacOS Troubleshooting and Verification](https://github.com/learn-co-curriculum/phase-0-macos-env-verification)

Windows:

- [Windows Environment Setup Introduction](https://github.com/learn-co-curriculum/phase-0-wsl2-env-introduction)
- [Updating Windows System and Installing Applications](https://github.com/learn-co-curriculum/phase-0-wsl2-env-system)
- [WSL2 Setup](https://github.com/learn-co-curriculum/phase-0-wsl2-env-windows-subsystem-linux)
- [Windows NodeJS Install](https://github.com/learn-co-curriculum/phase-0-wsl2-env-nodejs)
- [Windows Ruby Install](https://github.com/learn-co-curriculum/phase-0-wsl2-env-ruby) (Ruby/Rails curriculum)
- [Windows Python Install](https://github.com/learn-co-curriculum/phase-0-wsl2-env-python) (Python curriculum)
- [Windows Git and GitHub Configuration](https://github.com/learn-co-curriculum/phase-0-wsl2-env-git-github)
- [Configure the Flatiron Student Portal](https://github.com/learn-co-curriculum/phase-0-configuring-the-flatiron-student-portal) (deprecated starting with 2023 cohorts)
- [Windows Troubleshooting and Verification](https://github.com/learn-co-curriculum/phase-0-wsl2-env-verification)

### Render Setup (Phase 4)

- [Deploying to Render](https://github.com/learn-co-curriculum/phase-4-deploying-rails-api-to-render) (see the Environment Setup section)
