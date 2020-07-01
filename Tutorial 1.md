## Tutorial 1

## First, try git out git in the browser.

[Learn Git Branching](https://learngitbranching.js.org) is an in-browser tutorial. 

For the first tutorial, let's take a look at the **Introduction Sequence** steps:

1. Introduction to Git Commits
2. Branching in Git
3. Merging in Git

We won't worry about rebase in step 4. The later tutorials (*Ramping Up...* etc) are also more complex than we need at the moment.

## Now the exercise

First, we need to get you set up with access to GitLab. 

### Set up an SSH key 

There are two ways that git can communicate with the source code repository: over `HTTP` and over `SSH`. HTTP bumps into the problem that you have to keep typing in your password, which can get annoying, so if you are working on **turing** or on a Unix-like environment (e.g. a Mac) I usually recommend using SSH because you can set up an *ssh key* that will do the authentication for you.

If you are working locally on a home Windows computer, the set-up is a little more complex, so I recommend starting off working on turing (using x2go) and moving to Windows when you feel more confident.

SSH (Secure Shell) keys are way that you can authenticate a connection to another server without using a password. If you are working on turing:

1. You might already have an ssh-key generated, in which case you can re-use it. To see if you do, 

   ```bash
   cat ~/.ssh/id_rsa.pub
   ```

   If it's there, that should print out the contents of the *public key* (the part you can safely share). If it isn't you'll get "No such file or directory"

1. If you don't already have an ssh key, and need to generate one: Open a shell on turing, and generate an ssh key. The default options will be fine, and don't worry about setting a passphrase.

   ```bash
   ssh-keygen
   ```
   
   This should also work for you if you are on a Mac (in Terminal) or Windows Subsystem for Linux. If your are working just in Windows, it should also work in the `Git-Bash` program that
   comes with git and provides a little Unix-like shell environment.

2. Open the file `~/.ssh/id_rsa.pub` (the "public key") and copy the contents to the clipboard

   ```bash
   cat ~/.ssh/id_rsa.pub
   ```
   
   (and remember that in bash, Ctrl-C is cancel/break, not copy!)

### Log into GitLab, and give it your public key

1. Log into GitLab at http://gitlab.une.edu.au 

2. From your user menu (in the top right corner), go to Settings, and then in the menu on the left hand side choose "SSH Keys".

   Paste the contents of `id_rsa.pub` into the Key field, and "me on turing" into the title field. Then click "Add Key".
   
Now git can use your SSH key to authenticate you to GitLab if you run git commands from turing.

You can create and save keys from other computers (eg, your laptop) if you want to be able to work from there too.

### Request access to the cosc220-2020 group

We'll need to get you into the cosc220-2020 group on GitLab. If you log in and go to the group page: https://gitlab.une.edu.au/cosc220-2020  then you should see a "request access" button.

### Someone was followed...

1. Clone the repository for this part of the tutorial

   ```
   git clone git@gitlab.une.edu.au:cosc220-2020/tut-1-someone-was-followed.git
   ```

   Now if you do an `ls`, you'll see you have a new directory called `tut-1-someone-was-followed`, containing the git repository. `cd` into the directory. 
   
   ```bash
   cd tut-1-someone-was-followed
   ```      
   
   Now we can start working with git on this repository
   
2. Set your username and email address in the git config

   ```
   git config user.name "Your Name"
   git config user.email "your@email.address"
   ```   

In each revision of this tutorial repository, a name has been added or removed from `names.txt`. But in one of the revisions, the name came with a spy. The spy immediately left, but of course they are still present in the version control history and they left their password behind... 

Q: Who was followed, and what is the spy's password? (And why don't we ever check passwords into version control!)
   
### Join the meeting...

Let's practice branching, committing, merging, and pushing.

1. Create a new branch, named `s{your_student_number}`

   e.g., `git branch s1234`

2. Change to this new branch

   e.g., `git checkout s1234`

2. Add your name to names.txt, keeping it in alphabetical order

   (Use your favourite text-editor)

3. Commit your change. Don't forget, you'll need to use `git add` first to tell git which changes you intend to commit.

    ```sh
    git add names.txt
    git commit -m "Added Suzie Student"
    ```  

    **Beware**: This is a common place to hit your first bump. If you miss the `-m "Added Suzie Student"` and just say `git commit`,
    git will open up the *default editor* so you can type a comment. The default editor on some systems is `vim`, which works a little
    differently than you might expect and doesn't show its commands on the screen.

    By default, vim opens up in command mode. (You won't be able to type.) To go into `insert` mode, press `i`. To exit insert mode, press
    `escape`. To save and exit (complete the commit), type `:wq` from command mode (`:` to start a command, `w` for write, `q` for quit, and hit enter). 
    To abort the commit, type `:q!` from command mode (`:` to start a command, `q` for quit, and `!` to mean "yes, really without saving my changes").

7. If you haven't already (you should have), request access to 
   [http://gitlab.une.edu.au/groups/cosc220-2020](http://gitlab.une.edu.au/groups/cosc220-2020) from the GitLab web UI (so you can push your changes)

4. Push your new branch to GitHub with `git push -u origin {your_branch_name}`. e.g.,

   ```sh
   git push -u origin s1234
   ```

4. Change to the master branch. 

   ```sh
   git checkout master
   ```

   Is your name present in the file now?


5. Merge your branch into master.

   ```sh
   git merge s1234
   ```

6. Before you can *push* your changes, you'll need to *pull* any changes that others have already pushed.

    ```
    git pull
    ```

6. Fix any merge conflicts you might get!

   You might get a "merge conflict" if someone else has edited the same part of the file while you've been working.

   If, so the rule of thumb is:

   * Open the file in a text editor.
   * Edit the conflicting file so it looks how you want it (there'll be some <<<, ===, and >>> lines marking what was different in each version. Delete those and make the file look how you think it should)
   * `git add` the files
   * `git commit` the changes

7. Push the change to origin with `git push origin master`

### A spy story...

The last part of the exercise has a little spy story. But its real aim is just to give you a reason to look through the revision history of the repository.

The file `names.txt` has a (smallish) number of revisions at the start of this tutorial, as new agents have been added. But one of these agents was "followed" into the textfile by a spy. The spy left immediately, but of course you can still find them in the version history!

What you are looking for is a revision where the commit comment says that one person joined, but actually if you look at the names in the files, two were added. And in the next revision, the comment says one more name was added, but the change to the file shows the spy has been removed.

The easiest way to find it is from GitLab's web UI. Browse to the project on GitLab and click the History button.

https://gitlab.une.edu.au/cosc220-2020/tut-1-someone-was-followed