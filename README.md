# precommittest

This is a reference guide on how to create a repository, install pre-commit on your local macbook
and to run pre-commit for some and/or all files in your repository.


### Table of Contents
**[Creating a Repository](#creating-a-repository)**<br>
**[Syncing your Repository to your Macbook](#syncing-your-repository-to-your-macbook)**<br>
**[Installing pre-commit](#installing-pre-commit)**<br>
**[Running pre-commit](#running-pre-commit)**<br>


## Creating a Repository

### 1. First log into GitHub using your credentials or if you do not have an account, register.
### 2. Once you are logged in you could click on the plus icon near your profile picture to add a repository.
* There are four options:
 	1. New repository
	2. Import repository
	3. New codespace
	4. New Gist
* Click on the option that says New Repository to create an empty one.

### 3. Once you are in the Create a new repository window it will ask you for the following:
*  A repository name,
*  An optional description,
*  Whether you would like it to be private or public,
*  The options to add a README file, .gitignore and a license if applicable

### 4. After you select these options you can select Create repository.

## Syncing your Repository to your Macbook

Congratulations, you just created your first repository on GitHub but you would like have it synced to your local Macbook. Luckily
after creating you could do that easily. First create a new directory of the same name as your remote repository.
After you could run the following set of commands:

```
echo "#repos" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:user/repos.git
git push -u origin main

```

What this does is copy the README that you created from your remote repository, initialize the git repository on your machine,
stage and commit your changes for the first time, create a main branch, add the remote origin and push it to main while
setting it as the upstream branch.


Now you can check your GitHub repository where your changes should be synced in.


## Installing pre-commit

### 1. If you have the homebrew package type in the following command to install pre-commit on your terminal

```
brew install pre-commit
```

Once installed use ```pre-commit --version``` to check if you have the correct version installed.

### 2.Add a pre-commit configuration through a yaml file

The pre-commit package does not come with built in configurations, so in order to use the scripts we
must add these configurations ourselves.

To start create a file titled .pre-commit-config.yaml.

The contents in the file should look like the following:

```
repos:
-   repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v2.3.0
    hooks:
      -   id: check-yaml
      -   id: end-of-file-fixer
      -   id: trailing-whitespace
-   repo: https://github.com/psf/black
    	  rev: 22.10.0
     hooks:
-   id: black
```

What these configuration details entail is that it is fetching the scripts from the repositories being called
to check proper yaml syntax, find the end of the file and check for whitespaces. The second repository serves as the
auto-formatter.


### 3. Install the git hook scripts
Run the ```pre-commit install``` command to set up the git hook scripts on your repository.

This will also allow ```pre-commit``` to run everytime you commit changes.


##  Running pre-commit

To run pre-commit to check all files type in

```$ pre-commit-run --all-files```

The output may look like the following:

```
$ pre-commit run --all-files
[INFO] Initializing environment for https://github.com/pre-commit/pre-commit-hooks.
[INFO] Initializing environment for https://github.com/psf/black.
[INFO] Installing environment for https://github.com/pre-commit/pre-commit-hooks.
[INFO] Once installed this environment will be reused.
[INFO] This may take a few minutes...
[INFO] Installing environment for https://github.com/psf/black.
[INFO] Once installed this environment will be reused.
[INFO] This may take a few minutes...
Check Yaml...............................................................Passed
Fix End of Files.........................................................Passed
Trim Trailing Whitespace.................................................Failed
- hook id: trailing-whitespace
- exit code: 1

Files were modified by this hook. Additional output:

Fixing sample.py

black....................................................................Passed
```

What this will do is check for syntax errors in any python or yaml file in the directory and if there are any it returns a
failed message denoting the line that the syntax error exists in.

If not it simply gives a passed notfication and modifies the formatting of the files to ensure that they are ready to be
committed and easy for other developers to read.
