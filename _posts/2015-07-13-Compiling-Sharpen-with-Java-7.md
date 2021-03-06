---
layout: post
description: Using multiple versions of Java JDK for development to allow use of Sharpen Java to C# conversion
title: Compiling Sharpen with Java 7
---


![Java logo so pretty](https://davidraleigh.github.io/assets/jenv/Java_logo_and_wordmark.svg)

Sharpen is a conversion utility for taking code from Java to C#. I've been playing around with converting ESRI's geometry library to C#, but there are a number of for loops that have multiple declarations in the initialization section and that's causing me all sorts of trouble. If there was more documentation for Sharpen I might figure this out easily. Instead I'm going to jump into the source code and see what's going on.

**UPDATE:**

Originally I had uninstalled Java 8 from my Mac and installed Java 7. But this most recent OS I decided to use jenv instead. Following the instructions in [this stackoverflow post](http://stackoverflow.com/questions/26252591/mac-os-x-and-multiple-java-versions/29195815#29195815) allowed me to install Java 8 and 7 with [homebrew](http://brew.sh/).


```bash
$ brew tap caskroom/versions
$ brew cask search java
$ brew cask install java7
$ brew cask install java
```

After that it was easy enough to follow the instructions on the [jenv site](http://www.jenv.be/).

Installing is a breeze:
```bash
$ brew install jenv
```

Updating the `~/.bash_profile` means adding two lines:

```bash
export PATH="$HOME/.jenv/bin:$PATH"
eval "$(jenv init -)"
```

Then you add in all your jenv jdks:
```bash
$ jenv add /Library/Java/JavaVirtualMachines/jdk1.8.0_74.jdk/Contents/Home
oracle64-1.8.0.74 added
1.8.0.74 added
1.8 added
$ jenv add /Library/Java/JavaVirtualMachines/jdk1.7.0_80.jdk/Contents/Home
oracle64-1.7.0.80 added
1.7.0.80 added
1.7 added
```

For my case I needed a local version of Java 7 for use with sharpen. The following commands set up Java 7 for the directory I am running Sharpend from:

```bash
$ jenv local oracle64-1.7.0.80
$ java -version
java version "1.7.0_80"
Java(TM) SE Runtime Environment (build 1.7.0_80-b15)
Java HotSpot(TM) 64-Bit Server VM (build 24.80-b11, mixed mode)
```

Perfect. Now whenever I run Java from that directory it will be Java 7. If you need to remove the local setting just call `jenv local --unset`.

**ORIGINAL POST (This post shows how to remove other version of Java from system so that Java7 is the only one installed):**

In order to do that I must remove my Java 8 and install Java 7. Maybe I'm supposed to save Java 8 to the side and download java 7 after I've done that and do some linking, but it seems easier to follow a few tutorials instead. My favorite is [this gist](https://gist.github.com/johan/10590467).

Right now I have Java 8 installed:
```bash
$ java -version
java version "1.8.0_45"
Java(TM) SE Runtime Environment (build 1.8.0_45-b14)
Java HotSpot(TM) 64-Bit Server VM (build 25.45-b02, mixed mode)
```

Instead of stashing it and messing with linking I'm just going to dump Java 8:
``` bash
$ sudo rm -rf /Library/Java/JavaVirtualMachines/jdk1.8.0_25.jdk
$ sudo rm -rf /Library/PreferencePanes/JavaControlPanel.prefPane
$ sudo rm -rf /Library/Internet\ Plug-Ins/JavaAppletPlugin.plugin
```

Now when I check the version I'm stuck with the system install Java 6:
```bash
$ java -version
java version "1.6.0_65"
Java(TM) SE Runtime Environment (build 1.6.0_65-b14-466.1-11M4716)
Java HotSpot(TM) 64-Bit Server VM (build 20.65-b04-466.1, mixed mode)
```

After downloading and installing the java 7 JRE and the Java 7 JDK (those are really important to this whole process) I can see that I'm getting closer to running a system that uses Java 7:
```bash
$ /Library/Internet\ Plug-Ins/JavaAppletPlugin.plugin//Contents/Home/bin/java -version
java version "1.7.0_80"
Java(TM) SE Runtime Environment (build 1.7.0_80-b15)
Java HotSpot(TM) 64-Bit Server VM (build 24.80-b11, mixed mode)
```

I stash Java 6 and then link my newly installed Java 7 to the /usr/bin/java and all things are good in the world:
```bash
$ sudo mv /usr/bin/java /usr/bin/java-1.6
$ sudo ln -s /Library/Internet\ Plug-Ins/JavaAppletPlugin.plugin/Contents/Home/bin/java /usr/bin/java
$ java -version
java version "1.7.0_80"
Java(TM) SE Runtime Environment (build 1.7.0_80-b15)
Java HotSpot(TM) 64-Bit Server VM (build 24.80-b11, mixed mode)
```

Now to install Maven:
```bash
$ brew update
$ brew install maven
```

Holy crap. Now to try and build Sharpen? I'm not sure what I'm doing now. I've downloaded the [https://github.com/imazen/sharpen](https://github.com/imazen/sharpen) fork of repository as all it's tests are passing(the mono/sharpen repo has some inconclusive test results). Now to run tests and run the build tools
```bash
$ mvn clean test
$ mvn install
```

I've successfully built a sharpen jar file: ```sharpencore-0.0.1-SNAPSHOT-jar-with-dependencies.jar```. Using that file I've done a few runs of the conversion utility and I'm getting used to some of the options available. In the below example I'm trying to convert the geometry while referencing the various jar files necessary:

```bash
$ java -jar sharpencore-0.0.1-SNAPSHOT-jar-with-dependencies.jar ~/Downloads/geometry-api-java-master/src/ -cp ~/Downloads/geometry-api-java-master/DepFiles/public/jackson-core-asl-1.9.11.jar -cp ~/Downloads/geometry-api-java-master/DepFiles/unittest/junit-4.8.2.jar -cp ~/Downloads/geometry-api-java-master/DepFiles/public/java-json.jar -junitConversion
```

Now when you're in IntelliJ 14 you can go to ```File->Project Structure``` and edit the Java SDK to use the 1.7 version.
