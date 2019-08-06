class: center, middle

# The story so far...

---

## Recap

* The code is files on a disk
  - Some are Java
  - Some are "build files" and config
* I've thrown a lot of tools at you very quickly. 
* They each have responsibility for *part* of the process
* Put together, they form a toolchain

---

## Git

* Handles how files change over time
* We use it to get our code, and share our changes
* We have different "branches" so we can manage when we get which changes

---

## JUnit

* Tests our code for us
  - We write some "setup" code in `@Before` and `@BeforeClass` methods
  - We write the test procedure in some `@Test` methods
  - It checks anything we call assertion methods for (eg, `assertTrue("This should be true", true)`)


---

## Gradle 

* Runs tasks for us. The tasks are configured in `build.gradle`
* It knows some very useful tasks
  - fetching libraries from the Internet
  - calling javac with the right classpath settings to compile
  - compiling our test code
  - running our tests
* It'll do all of these as part of a `gradle build`

---

## IntelliJ

* Helps us edit our code
  - We could just use a text editor (the code is all just files on disk)
  - But IntelliJ can do things like autocomplete method names for us
  - It also happens to understand gradle and JUnit, so we can run gradle from within IntelliJ if we want (or even right-click a test and run it)

  
---

## GitLab

* Hosts the current state of our project
  - it has the full history of our code so far, and is where we share our code to
  - its issue tracker tells us what needs to be worked on, and by whom
  - its wiki holds all sorts of documentation about the project we all might need to see
  
---

## And...

* Getting your computer set up is perhaps the most annoying, frustrating and downright time-sucking part of it
  - professional developers curse that part too...
  - ... and in UNE we also have the joy of a web proxy


  
---

class: center, middle

### Typical workflow for a bit of editing on your feature

---

### getting 

*  First I need to get the latest code  
   *git clone or pull from GitLab*
   
* Then I need to check out whichever branch I'm working on  
   *git checkout mybranch*
   
* Then I run the build to check everything's ok before I start  
   *gradle build*
      
---

### editing and pushing      
      
* Then I edit the code and tests  
   *probably using IntelliJ*
   
* Then I run the tests before committing  
   *gradle build*
   
* Then I commit the changes and push them on my branch to GitLab  
   *git commit and git push -u origin mybranch*
   

---

class: center, middle

## Typical workflow for sharing your changes

---

### getting

* First I need to get the latest code   
   *git clone or pull from GitLab*
   
* Then I need to check out whichever branch I'm working on  
   *git checkout myfeature*
   
* Then I merge in the latest changes from master  
   *git merge master*
  
---

### fixing anything that's drifted  
   
* Then I fix any prbolems the code and tests  
   *gradle build, and perhaps some editing using IntelliJ*
   
* Then I run the tests before committing  
   *gradle build*
   
* Then I commit the changes on my branch    
   *git commit and git push -u origin mybranch*
   
---

### merging into master   
   
* Then I merge the changes into master  
   *git checkout master and git merge mybranch*
   
* Then I run the tests *again* just in case  
   *gradle build*
   
* Then I push the changes to GitLab  
   *git push origin master*
   
   
---

class: center, middle

## How the project is organised

(demo)
 
---

### Where your team's code might fit in

* If you are just on the client, you might just have a client

--

* If you are just on the server, you'll probably define your own module, and make the server depend on it  
  
-- 

* If you have anything that needs to go between the server and client, put it in a module, and add dependencies where you need them. eg, kryonet to register the classes, plus your server module, plus your client

---

### You are not alone

- Ask questions on the forums and on slack. Feel free to @mention me, but often someone smart in the class will have solved your problem first.

- Look at the other teams code. There will very soon be examples of almost everything.

- Comment on each others' tickets. (The marks scheme asks you to)


