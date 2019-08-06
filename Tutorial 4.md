


### Clone the repository

Depending on whether you have set up an ssh public/private key pair on turing (and given GitLab your public key), you can either use the ssh or the https URL

either

```
git clone git@gitlab.une.edu.au:cosc220-2018/classproject.git
```

or

```
setproxy
git clone https://gitlab.une.edu.au/cosc220-2018/classproject.git
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
   
   If it doesn't we'll need to set your `JAVA_HOME` environment variable to `/opt/jdk1.8.0_161/`
   
   You can do this by creating a file `setjavaopts` containing
   
   ```
   export JAVA_HOME=/opt/jdk1.8.0_161/
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

   `/usr/share/gradle`
   
   `/opt/jdk1.8.0_161`
   
   
* Open the gradle pane on the right hand side. Try running the client from there.

* Open the `Run` menu and `Edit configurations...`

   Set up a configuration for `gradle runClient` in the client module
   
   Try running the client; and try debugging the client (and using a breakpoint)

   ## Project notes

Welcome to joining a work in progress. In the project code so far, I've given you:

* A core project which contains some of the elementary classes
* A basicrooms project which gradually will contain some simple implementations, but not the crazy stuff like rooms where suddenly gravity is upside down. It does also include a RoomBuilder, so we can create rooms from scripts.
* A little networking library (Kryonet, usually used in games), also because it's one of the simplest to use, and a protocol package that ensures the classes are registered in the right order
* A client that shows a RoomView, and is ready to set up some timeres... but on the day of release all it does is ping the server.

There is relatively extensive use of Java 8 lambdas.

A few rules:

* This year there should be one (and only one) `runServer` and `runClient` task. We're going to try not to fragment into different executables, though we could have many different screens

* Each team's feature should interact with other teams' features. 

* Each client should interact with the server in a meaningful way.

* Remember to configure your email address in your commits (using your myune.edu.au address)
 
* Remember that other students can and will edit your code (but communicate)

* Remember to run the tests before pushing changes to GitLab on master 
