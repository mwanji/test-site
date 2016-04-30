---
title: "Using Java 8 with Eclipse and m2e"
layout: post
og_description: "How to get Java 8 and Maven to work in Eclipse 4.3 and 4.4"
og_image: "/img/2014/eclipse-m2e-java-8/luna.png"
---

Eclipse is as confusing as ever. And the splash screens aren't getting any better...

![Kepler splash screen]({{site.baseurl}}/img/2014/eclipse-m2e-java-8/kepler.png "Kepler splash screen")![Luna splash screen]({{site.baseurl}}/img/2014/eclipse-m2e-java-8/luna.png "Luna splash screen")

With the official release of Java 8, Eclipse [announced support for Java 8 in Kepler and Luna](https://wiki.eclipse.org/JDT/Eclipse_Java_8_Support_For_Kepler). In reality, that support is not provided out-of-the-box, especially if you want to use Maven.

**Kepler**

In Eclipse Kepler SR2 (4.3.2) (the SR2 part is important), you need to get a JDT patch from the [4.3 update site](http://download.eclipse.org/eclipse/updates/4.3-P-builds/). There are instuctions in the [announcement](https://wiki.eclipse.org/JDT/Eclipse_Java_8_Support_For_Kepler). Then, add an Installed JRE and an execution environment:

1. Window > Preferences > Java > Installed JREs
2. Add...
3. Choose JDK 8's home directory
4. Click OK and go to Window > Preferences > Java > Installed JREs > Execution Environment (you have to close the preferences pane because the list of execution environments isn't updated when you add the Java 8 JDK)
5. Select JavaSE-1.8 and check jdk1.8.0

**Luna**

I initially downloaded Luna Milestone 6 and was surprised not to see Java 8 support. By reading the announcement very carefully, it turns out that the earliest Luna build with Java 8 support is from March 18, 2014 and Milestone 6 was released on March 6. So, you can either:

1. download the latest [4.4 milestone](http://download.eclipse.org/eclipse/downloads/) ([4.4M6](http://download.eclipse.org/eclipse/downloads/drops4/S-4.4M6-201403061200/) at the time of writing) and add the integration builds [update site](http://download.eclipse.org/eclipse/updates/4.4-I-builds), or
2. [download I20140325-0830](http://download.eclipse.org/eclipse/downloads/drops4/I20140325-0830/).

In both cases, choose a download from the "Eclipse SDK" section. I took the first option, because of all the big red X's in the Status column of the integration build.

Luna is still rough. It seems to work, but there are a lot of weird, dev-related items in menus.

**Maven**

When using Maven your work is still not done: setting the maven-compiler-plugin to 1.8 will cause Eclipse to set the project's compiler level to 1.4.

You need the latest from the [m2e](http://eclipse.org/m2e) 1.5 milestones [update site](http://download.eclipse.org/technology/m2e/milestones/1.5).

And there you have it: Java 8 and Maven in Eclipse, in 657 easy steps!
