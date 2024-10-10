![This picture here](testmd.png "the title of this pic")



# How to use multiple GitHub accounts on the same computer using SSH


### Why would anyone want to this?
For example if you use the same computer for your personal projects and therefore, account, as well as, for work projects, where you have to use an account associated with an organization, having the ability to easily toggle between accounts can be a great time-saver. At the very least, that was my motivation.



## Setting up SSH for authorization
This is like logging in, just in a slightly more complicated way.
To start you should make a config folder for `ssh`.
- Linux or MacOS

    `mkdir ~/.ssh`
- Windows

    `mkdir ~/AppData/.ssh`

And the to navigate to the folder:
- Linux or MacOs

    `cd ~/.ssh`
- Windows

    `cd ~/AppData/.ssh`

### Generate keys
Some keys need to be generated in order to verify user identity.

This is done with the command:

`ssh-keygen -t rsa -C "email_github_personal@mail.com" -f "personal"`

`ssh-keygen -t rsa -C "email_github_work@mail.com" -f "work"`

Here `personal` and `work` can be named anything to distinguish them and will be the name of the files holding the keys.

After the keys are generated the directory will look like this:
- `personal` and `personal.pub`
- `work` and `work.pub`

Now make an `ssh` config file:

`touch ~/.ssh/config`

Into it, the profiles should put:

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


