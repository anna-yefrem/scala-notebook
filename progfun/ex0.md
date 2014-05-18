Guides to Exercises
==================

### Installing Required Tools

1. JDK (Java Development kit)
  * Linux:
    * Ubuntu, Debian: execute `sudo apt-get install openjdk-7-jdk`
    * Fedora, Oracle, Red Hat, CentOS: `su -c "yum install java-1.7.0-openjdk-devel"`
    * Manual:
      1. download the `.tar.gz` archive from http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html
      2. unpack by `tar zxf jdk1.7.0-VERSION.tar.gz`
      3. add the `bin/` directory of the extracted JDK to the `PATH` environment variable
      4. add `export PATH=/PATH/TO/YOUR/jdk1.7.0-VERSION/bin:$PATH` to `~/.bashrc` in `vi/vim`
  * Mac OS X: 
  * Windows:
    1. download JDK installer from http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html
    2. run the installer
    3. add the `bin/` directory to `PATH` environment variables
    4. verify installation by `java -version`
2. sbt (simple build tool for Scala and Java projects)
  * Linux:
    1. download sbt from http://scalasbt.artifactoryonline.com/scalasbt/sbt-native-packages/org/scala-sbt/sbt/0.12.4/sbt.tgz
    2. unpack by `tar zxf `
    3. add the `bin/` directory to the `PATH` environment variable
    4. add the `export PATH=/PATH/TO/YOUR/sbt/bin:$PATH` to `~/.bashrc` in `vi/vim`
    5. verify the installation by opening a new terminal and execute `sbt -h`
  * Mac OS X:
    1. `brew update`
    2. `brew install sbt`
  * Windows:
    1. download the sbt installer from http://scalasbt.artifactoryonline.com/scalasbt/sbt-native-packages/org/scala-sbt/sbt/0.12.4/sbt.msi
    2. run the installer
3. Scala IDE for Eclipse
  * Linux, Mac OS X, Windows: 
    1. download the Scala IDE for eclipse with the Scala Worksheet pre-installed from `http://typesafe.com/stack/scala_ide_download`
    2. unpack it and start eclipse

### Submitting Solutions

1. Obtain the Project Files
2. Using the Scala REPL
3. Opening the Project in Eclipse
4. Running your Code
  * Using the Scala REPL: typing `console` in the sbt copsole
  * Using a Main Object:
    1. In eclipse, right-click on the package example in `src/main/scala` and select `"New" - "Scala Object"`
    2. Use `Main` as the object name (any other name would also work)
    3. Confirm by clicking `"Finish"`
    4. Edit the `Main` object, for examples
    ```
    object Main extends App {
    println(Lists.max(List(1,3,2)))
        }
    ```
    5. Execute by right-click on the file `Main.scala` and select `"Run As" - "Scala Application"`
    6. Or run the `Main` object in sbt by `run`
5. Write Tests
  * Writing tests in `src/test/scala` source folder
6. Submitting Solution
  * Typing `submit your.email@domain.com submissionPassword` in the sbt console
