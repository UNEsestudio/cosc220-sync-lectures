## Tutorial 3 -- the project base is up

This week, the class project has been released, and the focus of the tutorial is on getting your group ready to start development.

I would like you to create a ticket on GitLab for your group's first feature as soon as possible. By Tuesday! It doesn't need to be polished or complete -- develop it in public rather than planning it in private.

(Otherwise you might do all your planning and then discover that another group is planning the same thing.)

### Ensure you are in a team

* Check the [Group formation page](https://gitlab.une.edu.au/cosc220-2016/sense-o-matic/wikis/group-formation) on the GitLab Wiki to see that you are in the right group. If not, tell the unit coordinator!

### Ensure you have access to the class repository

* Check that you have access to the cosc220-2016 group on GitLab

  You should be able to see this by visiting https://gitlab.une.edu.au/cosc220-2016/
  
  If you do not yet have access, let the unit coordinator know (preferably by pressing the "Request access" button)
  

### Clone the repository

Depending on whether you have set up an ssh public/private key pair on turing (and given GitLab your public key), you can either use the ssh or the https URL

either

```
git clone git@gitlab.une.edu.au:cosc220-2016/sense-o-matic.git
```

or

```
setproxy
git clone https://gitlab.une.edu.au/cosc220-2016/sense-o-matic.git
```

You can also clone the repository (on your home machine) using GitHub for Windows or Atlassian SourceTree. 

### Create a group branch, and push it

cd into the repository (the directory that git clone just created)

* Create a branch for your group, called group_*groupName*

* Push the branch to origin

  ```
  git push -u origin group_groupName // not forgetting to replace "groupName" with your group name...
  ```

You now have your own group branch. Generally in this course, we want your code to be merged into master. But having a group branch means that you can share code with your group even if it is not working. (We don't want broken code merged to master, but how you manage your group branch is less of a concern)

### Remember

When we want to share code with our class on master, there are some steps we need to take to 

1. `gradle build` and check it runs successfully (including passing the tests)

2. `git commit` to save a snapshot of our changes

3. `git pull` to get changes our colleagues may already have pushed

4. Fix any merge conflicts that result, test and commit again if necessary. (And then another `git pull` in case more changes arrived in the meantime)

5. `gradle build` again to check the merged code works and passes all the tests. If not, fix it.

6. `git push` to share our changes

### Note that you need to use the Oracle JDK

* Run

   ```
   java -version
   ```
   
   And check that the Java runtime reports itself as "HotSpot" rather than "OpenJDK".
   
   If it doesn't we'll need to set your `JAVA_HOME` environment variable to `/opt/jdk1.8.0_31/`
   
   You can do this by creating a file `setjavaopts` containing
   
   ```
   export JAVA_HOME=/opt/jdk1.8.0_66/
   ```
   
   and then running
   
   ```
   source setjavaopts
   ```
   
   (The version of OpenJDK on turing doesn't include JavaFX, and we need it)

### Make sure gradle can get through the HTTP proxy

(Instructions are in last week's tutorial)

### Run the code

At the moment, the code doesn't do very much at all. But there is something you can run, and it will make a connection from the client to the server.

* To run the server, go into the server directory of the project and run 

   ```
   gradle runServer
   ```
   
   Very quickly, you should notice your first problem -- the ports are hardcoded, and if there's more than one group on turing you'll start to get errors!
   
   In the tutorial, we'll make the port a command-line argument.
   
   On turing, you can get a unique port number for your account by running 
   
   ```
   my_tomcat_ports
   ```
   
   Any of the ports it lists should be fine for you to use.
   
* To run the client, go into the client directory of the project and run

   ```
   gradle runClient
   ```
   
   Hmm, we need to set a parameter here for the port number too...
   

### Open the code in IntelliJ

* You can open IntelliJ on Turing by `cd`ing into the project directory and running `idea.sh .`

* Opening the project, you should get a message detecting that it is a Gradle project. Click to import it in the message

* You may need to set the location of Gradle and which Java SDK to use

   `/opt/gradle`
   
   `/opt/jdk1.8.0_66`
   
   
* Open the gradle pane on the right hand side. Try running the client from there.

* Open the `Run` menu and `Edit configurations...`

   Set up a configuration for `gradle runClient` in the client module
   
   Try running the client; and try debugging the client (and using a breakpoint)
   
### Create a ticket on GitLab

On the GitHub repository, go to `Issues` and create a new issue. Roughly describe what you are planning to do, and tag the feature with your group name.

https://gitlab.une.edu.au/cosc220-2016/sense-o-matic/issues

**Don't wait** until you've finished planning your task. Put the ticket up as soon as you have even the vaguest idea of what you'd like to do. You can refine it later, but putting your ticket up earmarks what you would like your team to develop (so we don't get multiple teams trying to do exactly the same feature).

## The project task...

(Sorry, I haven't prepared a funny intro video this time)

Data science is all the rage at the moment -- at UNE, we're even doing predictive analytics on sheep. But there's a couple of different kinds: batch systems (where everything gets run overnight and you see the results in the morning) and real-time systems (where as data comes in everything updates live and instantaneously). And computer-supported-cooperative-work is fairly fun. Be it Google Docs, or playing games over the network. And visualising live data is always rather fun too. 

At their heart, all of these are about event based systems. So we're going to build something small that does quite a lot of that.

In the project code so far, I've given you:

* Some server-side code for tasks to be done by different "actors" (just think of them as people who post messages to each other). This uses Akka, which has horribly complicated-sounding documentation, but is actually much easier to use.
* A little networking library (Kryonet, usually used in games), also because it's one of the simplest to use.
* A little "monitoring" client that doesn't do much more than connect at the moment. That uses JavaFX, which is the modern UI toolkit for Java.

And on top of this platform, we're going to build all sorts of things. Things that produce data (eg, being able to play games across the network), things that bring data into the platform (eg, you could hook it up to the GitLab API), things that visualise data... it's up to the teams to pick what they want to build.

We could even build Google Wave!

A few rules:

* Although there will be more than one client app, there should only be one server executable. Lots of actors doing different things, and lots of modules making up that server, but one `runServer` task.

* Each team's feature should interact with other teams' features. 

* Each client should interact with the server in a meaningful way.

* Remember to configure your email address in your commits (using your myune.edu.au address)
* Remember that other students can and will edit your code (but communicate)
* Remember to run the tests before pushing changes to GitLab on master 

