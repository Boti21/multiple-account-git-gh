# How to use multiple GitHub accounts on the same computer using SSH


### Why would anyone want to this?
For example if you use the same computer for your personal projects and therefore, account, as well as, for work projects, where you have to use an account associated with an organization, having the ability to easily toggle between accounts can be a great time-saver. At the very least, that was my motivation.

---

## Setting up SSH for authorization
This is like logging in, just in a slightly more complicated way.
To start you should make a config folder for `ssh`.

```
    mkdir ~/.ssh
```

And the to navigate to the folder:

```
    cd ~/.ssh
```

---

### Generate keys
Some keys need to be generated in order to verify user identity.

This is done with the commands:

```
ssh-keygen -t rsa -C "<personal email used for GH>@mail.com" -f "<personal account name or an alias>"

ssh-keygen -t rsa -C "<work email used for GH>@mail.com" -f "<work account name or an alias>"
```

This command will prompt you for a passphrase but this can be ignored by pressing Enter 2 times.


After the keys are generated the directory will look like this:
- `<personal account name or an alias>` and `<personal account name or an alias>.pub`
- `<work account name or an alias>` and `<work account name or an alias>.pub`

From this point on I will refer to `<personal account name or an alias>` as `personal` and to 
`<work account name or an alias>` as `work`.
---

### Add keys to SSH Agent

After this the keys need to be added to the SSH agent using the commands:

```
ssh-add -K ~/.ssh/personal
ssh-add -K ~/.ssh/work
```

Naturally Windows might need some extra attention. A problem I am aware of is that OpenSSH (the SSH client on your computer) might not be running and has to be started.

The public key needs to be added to GitHub, so that it will recognize the user. To do this use this link (https://github.com/settings/keys) and give it a title.

---

### Making SSH config file

Now make an `ssh` config file:

Linux and MacOS:
```
touch ~/.ssh/config
```
Windows (Powershell):
```
New-Item -Path "~\.ssh\config" -ItemType File 
```

Into it, the profiles should be put:

Linux and MacOS:
```
# Personal (this line is a comment)
Host personal
    HostName github.com
    User git
    IdentityFile ~/.ssh/personal

# Work (this line is also a comment)
Host work
    HostName github.com
    User git
    IdentityFile ~/.ssh/work
```

Windows:
```
# Personal (this line is a comment)
Host personal
    HostName github.com
    User git
    IdentityFile ~\.ssh\personal

# Work (this line is also a comment)
Host work
    HostName github.com
    User git
    IdentityFile ~\.ssh\work
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


