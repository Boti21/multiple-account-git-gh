# How to use multiple GitHub accounts on the same computer using SSH


### Why would anyone want to this?
For example if you use the same computer for your personal projects and therefore, account, as well as, for work projects, where you have to use an account associated with an organization, having the ability to easily toggle between accounts can be a great time-saver. At the very least, that was my motivation.

---

## Setting up SSH for authorization
This is like logging in, just in a slightly more complicated way.
To start you should make a config folder for `ssh`.
- Linux or MacOS

```
    mkdir ~/.ssh
```
- Windows

```
    mkdir ~/AppData/.ssh
```

And the to navigate to the folder:
- Linux or MacOs

```
    cd ~/.ssh
```
- Windows

```
    cd ~/AppData/.ssh
```

---

### Generate keys
Some keys need to be generated in order to verify user identity.

This is done with the commands:

```
ssh-keygen -t rsa -C "email_github_personal@mail.com" -f "personal"

ssh-keygen -t rsa -C "email_github_work@mail.com" -f "work"
```

Here `personal` and `work` can be named anything to distinguish them and will be the name of the files holding the keys.

After the keys are generated the directory will look like this:
- `personal` and `personal.pub`
- `work` and `work.pub`

---

### Add keys to SSH Agent

After this the keys need to be added to the SSH agent using the commands:

```
ssh-add -K ~/.ssh/personal
ssh-add -K ~/.ssh/work
```

The public key needs to be added to GitHub, so that it will recognize the user. To do this use this link (https://github.com/settings/keys) and give it a title.

---

### Making SSH config file

Now make an `ssh` config file:

```
touch ~/.ssh/config
```

Into it, the profiles should be put:

```
# Personal
Host personal
    HostName github.com
    User git
    IdentityFile ~/.ssh/personal

# Work
Host work
    HostName github.com
    User git
    IdentityFile ~/.ssh/work
```

---

### Using git to clone repositories
The last step is to clone a repository from GitHub:
```
git clone git@personal:GitHub_user/repo.git
git clone git@work:GitHub_user/repo.git
```


Every time you want to push using `git` to repositories, the file `./.git/config` has to be configured with the following commands:
```
git config --local user.name "personal_github_username" && git config --local user.email "personal_github_user_email"
git config --local user.name "work_github_username" && git config --local user.email "work_github_user_email"
```

This is to have commits that are attributed your account, therefore, each line should be run in the repositories that your respective GitHub accounts have access to. These commands can be made into an `alias` for convenience, my personal `alias` is `gcp` (git config personal) for example.


### If there are any problems with this tutorial please alert me to them!
