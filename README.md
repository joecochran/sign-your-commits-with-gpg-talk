# Sign Your Commits, with GPG!

Today we're going to set up commit signing with GPG. You're welcome to follow along, and this repo will include all the links and commands run so that you can do it later if you choose!

## Install GPG Tools

First, we're going to install the GPG Suite from https://gpgtools.org/. Click the big red button to download it, open the dmg, and click install.

Note: the installer will include GPG Mail, a paid product. You do not have to pay for the rest of the suite, nor do you need to set up the trial that is included with the GPG suite.

## Create a New Key Pair

You'll get a popup that says "Installer would like to controll GPG Keychain." Tell this prompt yes and it will launch GPG keychain. Let's create a key pair now!

Aside from ensuring the email address is correct, you can safely trust the defaults. Things to note: this key will be set to expire in 5 years. If you'd prefer to create new keys more often, customize the expiration date in the Advanced section.

Set a password and click create key!

## Hook It Up To Github

Github has [fantastic documentation](https://docs.github.com/en/github/authenticating-to-github/managing-commit-signature-verification/adding-a-new-gpg-key-to-your-github-account) and honestly anyone here could easily follow along. For the sake of the workshop though, we'll step through it.

Jump into your command line. To confirm everything is working correctly run:

```
gpg --list-keys
```

You should see the key pair you just generated using GPG suite.

Now we need to get the id of our key. Run the following:

```
gpg --list-secret-keys --keyid-format=long
```

You should see something that looks like:

```
sec   4096R/3AA5C34371567BD2 2016-03-10 [expires: 2017-03-10]
uid                          Foo 
ssb   4096R/42B317FD4BA89E7A 2016-03-10
```

The string we want in this example is `3AA5C34371567BD2` (the first one).

With that id, we can output what we need to enter in github with

```
gpg --armor --export 668680C4FFA3F169
```

Protip for mac users, to put the output of that in your clipboard, use pbcopy!

```
gpg --armor --export 668680C4FFA3F169 | pbcopy
```

With that string in your clipboard, go to [account settings](https://github.com/settings/) page in github and navigate to the [SSH and GPG](https://github.com/settings/keys) keys page. Scroll down and click the New GPG Key button. Paste the exported key into the textarea and click Add GPG Key. You now have a gpg key associated with your github account.

### Vigilant Mode

You can enable Vigilant Mode in github to flag all unsigned commits in the future with the *Unverified* tag. This will include your past commits.

## Set Up Git

Let's head to the codebase and set up git to sign our commits! Then grab the ID we used earlier with:

```
gpg --list-secret-keys --keyid-format=long

```
Which outputs:
```
sec   4096R/3AA5C34371567BD2 2016-03-10 [expires: 2017-03-10]
uid                          Foo 
ssb   4096R/42B317FD4BA89E7A 2016-03-10
```

We'll use the first one again, so in this case `3AA5C34371567BD2`. 

```
git config --global user.signingkey 3AA5C34371567BD2
```

Now git knows who you are! Next time you're about to commit, add `-S` to the command!

```
git commit -S -m "this is a commit message"
```
