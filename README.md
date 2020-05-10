
# Writing a Melan extension

The aim of this repository is to make it easy for anyone to create a [Melan](https://github.com/jhadida/melan) extension.

Melan extensions are Python packages, which define and expose commands that can be loaded into a `.mel` document.

If this is your first time, please follow the steps below. Happy coding! :) 

---

## Step-by-step guide

### Step 1: give it a name :hatching_chick:

First and foremost, choose a name for your extension:

- **Clearer is better**, don't choose a catchy name if it obscures what your extension does.
- Add a `melan_` prefix to it (e.g. `melan_html`). All extensions **MUST** have this prefix; don't worry, users won't have to type it when loading your extension.
- Check that the name is available [on PyPI](https://pypi.org/) (the Python Package Index). If it is taken, you might want to contribute to the existing package instead, or simply rename yours (e.g. by appending your name `melan_html_jsmith`).

### Step 2: create your repo :octocat:

In order to create your own package, you first need to create a `git` repository for it.
If you don't know how to do that, take a moment to read the [Github help pages](https://help.github.com/en/github/getting-started-with-github/create-a-repo).
It's also good idea to [setup SSH keys](https://help.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh) for authentication (safer and more convenient than typing your username/password all the time).
Once your repo is created on Github, write down its URL somewhere (should look like `git@github.com:<Username>/<ExtensionName>.git`). 

Next, you'll need to decide _where_ the folder will live on your machine (e.g. `$HOME/repo`). Open a terminal and type:
```
cd "The folder where your repo will live"
ExtensionName="Your extension's name"
ExtensionRepo="Your extension's URL"
```
Obviously, you should replace the quoted contents as needed (**note:** there should be _no spaces_ around the `=` signs!).

Finally, you'll need to copy the contents of this template extension to the folder `$ExtensionName` on your machine; link that folder to your own repo; and sync them together. To do just that, type the following:
```
git clone https://github.com/jhadida/melan_extension "$ExtensionName"
cd "$ExtensionName"
git remote set-url origin "$ExtensionRepo"
git push -u origin master
```

If you refresh the page of your repo on Github, you should now see the same files as above.

### Step 3: setup your extension :sunrise_over_mountains:

> **Tip:** we recommend using [VSCode](https://code.visualstudio.com/) to work with Melan.<br>
> Make sure you also install the [language extension](https://marketplace.visualstudio.com/items?itemName=jhadida.melan), for syntax highlighting and more!

Now that you have a copy of this template extension on your machine, and that your folder is setup to sync with Github, let's get down to business!

1. Edit the file `package.json`, and provide a value for each field. The `name` should correspond to the name of your repo (no spaces); and if you don't want to provide an e-mail address, you may set it to `<YourName>@unknown.invalid` (cf. [TLD](https://en.wikipedia.org/wiki/.invalid)).

2. Edit the file `LICENSE.txt` and replace `<YourNameHere>` with.. your name :)

3. Rename `README.md` as `_.md` (`git` is [setup](https://github.com/jhadida/melan_extension/blob/3a720b4e42eda0870e4a8f7fcab998e5893440fa/.gitignore#L31) to ignore this file), and create a new readme file to describe your extension. You don't have to write everything now, but a good structure would look like this:
````
# Extension name

Short description.

## Quick install

```
python3 -m pip install --user --upgrade <ExtensionName>
```

Warn about external dependencies (e.g. `ffmpeg`).

## Documentation

Either list your commands here, or point to the documentation pages (can also be a wiki).

## Known Issues

Gotchas, experimental stuff, compatibility issues, etc.

## Release Notes

Mention bug fixes and exciting new features.
````

### Step 4: code it! :art: :musical_score:

It's time to actually implement your extension! See the [Melan documentation](https://jhadida.github.io/melan/#/create/intro) for guidance.

Note that, if you want your Python module to live in a subfolder `src/` (as opposed to the root of the repo), you will need to set the following options in the file `setup.py`:
```
packages=setuptools.find_packages('src'),
package_dir={'': 'src'}
```

Similarly, if your extension requires other PyPI packages, add them to the list `install_requires`.

### Step 5: test it! :boom: :scream:

There is nothing more frustrating for a user than to install a package that looks cool, only to see it fail at the first use, or not do what the doc says.

Writing documentation and keeping it up-to-date takes time, but it is necessary. Nothing fancy, but at the very least, list the commands provided by your extension, and explain what they do. Your users will be very grateful for your efforts, and your future self will thank you for being such a bright star :star2:.

Writing proper tests can also be very time-consuming. A good way to start without too much pain is to provide simple examples to showcase the use of your commands. For example, these could be `.mel` files saved in a subfolder `examples`. Make sure the compiled results are what you'd expect!

> **Tip:** use the built-in command `\pypath` in your example files so the compiler can load the latest version of your extension (for example `\pypath(before){../src}`).

### Step 6: publish on PyPI :trophy: :rocket:

Congratulations, you have now coded and tested your very own Melan extension! Take a moment to celebrate your awesomeness :100: :tada:

Before you share your hard work with the world, tidy up your house! And by your house, I mean your repo, of course:

- Make sure `README.md` is up-to-date (in particular, update the release notes).
- Make sure `CHANGELOG.md` is up-to-date.

Once everything is in order, commit and push all changes to Github. If you are not sure what that means, stop here, and take the time you need to [learn about git](https://help.github.com/en/github/managing-files-in-a-repository/managing-files-using-the-command-line). This is important, and nobody else can do this for you; learning takes time.

With everything commited and pushed, your extension is now ready to be published on PyPI!
Create an account on [PyPI.org](https://pypi.org/), and then from a terminal, simply run: 
```
./publish.sh 0.0.1 -m "First version"
```
This will build a distribution of your Python package to be uploaded; prompt you for the username/password of your PyPI account; and publish your package there.
Done!

### Step 7: future versions :rainbow: :european_castle:

You probably have brilliant ideas about how your extension could be improved; and as users start using your commands, it is likely they'll come up with questions, too.
It's completely fine if your first version is not up to scratch, this is why versions are a thing.

As you develop your extension, you'll be able to publish future versions simply by calling:
```
./publish.sh <Version> -m <Message>
```

A couple of important things to note:

- Version numbers are formatted as `major.minor.patch`. Each of these numbers means something, and you should take the time to understand that before you plan your next version: https://semver.org/
- Before calling `./publish.sh`, always make sure all your changes are pushed to Github, and that your repo is clean. You don't want to be publishing 5 patches in 10 minutes because you kept missing little things. Call `./publish.sh` with no input to show a quick checklist in your terminal.

Finally, while using your username/password for your account on PyPI is fine, it's a good idea to setup an [API token](http://pyfound.blogspot.com/2019/07/pypi-now-supports-uploading-via-api.html) instead (see [the docs](https://pypi.org/help/#apitoken)). You can find information about how to do this by typing `./token.sh` in your terminal.

--- 

## Contributions

This is a _template_ extension, so obviously it is not a functional package.

However, if you think something is wrong, or missing, or could be better; feel free to [open an issue](https://github.com/jhadida/melan_extension/issues) and/or submit a PR!
