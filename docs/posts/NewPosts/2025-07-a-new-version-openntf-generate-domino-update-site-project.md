---
title: "A New version: OpenNTF generate-domino-update-site Project"
slug: new-version-openntf-generate-domino-update-site-project
date: 2025-07-18T15:11:07.777Z
draft: false
tags:
    - dots
    - java
    - open-source
    - openntf
    - plugins
    - docker
categories:
    - OpenNTF
description: "Announcing new features and improvements in the OpenNTF generate-domino-update-site Maven project, including Docker and DOTS support."
---

I have created a new version of [the OpenNTF generate-domino-update-site project](https://github.com/OpenNTF/generate-domino-update-site). This Maven script, originally developed by the brilliant [Jesse Gallagher](https://frostillic.us/), helps plugin developers create p2 repositories for their Domino plugin development environments. The [new version (6.0.0)](https://github.com/OpenNTF/generate-domino-update-site/releases/tag/6.0.0) includes several enhancements and bug fixes that improve its functionality and usability.
<!-- more -->
I actually implemented the changes some time ago, but hadn't had a chance to test all parts of the code. I finally found time to do so, and Jesse updated the project and published a new release.

The project now supports the latest Domino versions, including 14.5, and fixes a few platform issues for Macs. Two new features have been added:

## Docker Support

Since moving my servers to Docker, generating new update sites has been a challenge. I either had to run the script inside the Docker container or copy the Domino installation to my local machine. While working on a testing script for another project, I realized it would be useful to add Docker support to the generate-domino-update-site project.

The generate-domino-update-site project can now connect to a Docker container running Domino and generate the update site directly from there:

```bash
$ mvn org.openntf.p2:generate-domino-update-site:6.0.0:generateUpdateSite \
    -DsrcContainer="domino-container-name" \
    -DdockerDominoDir="/opt/hcl/domino/notes/latest/linux" \
    -Ddest="/Users/someuser/Desktop/UpdateSite"
```

I don't run the Domino server on my Mac, so I also added a second option to extract update sites directly from a Docker image:

```bash
$ mvn org.openntf.p2:generate-domino-update-site:6.0.0:generateUpdateSite \
    -DsrcImage="domino-image-name:latest" \
    -DdockerDominoDir="/opt/hcl/domino/notes/latest/linux" \
    -Ddest="/Users/someuser/Desktop/UpdateSite"
```

This allows you to generate update sites without needing a Domino server running on your local machine. You can simply use a Docker image with Domino installed. In theory, this should also work with a remote Docker server using Docker contexts, but I haven't tested that yet.

## DOTS Support

The second new feature is support for DOTS (Domino OSGi Tasklet Services). The generate-domino-update-site project can now generate update sites for DOTS projects. This is particularly useful if you are developing DOTS plugins and need target platforms for your project.

To generate an update site for a DOTS project, use the following command:

```bash
$ mvn org.openntf.p2:generate-domino-update-site:6.0.0:generateUpdateSite \
    -DsrcImage="domino-image-name:latest" \
    -DdockerDominoDir="/opt/hcl/domino/notes/latest/linux" \
    -Ddest="/Users/someuser/Desktop/UpdateSite" \
    -DonlyDots=true
```

The new parameter `-DonlyDots=true` tells the script to include only DOTS plugins in the generated update site. You can then add the generated update site to your target platform in Eclipse, or use it in your Maven project.

## Using Update Sites in Maven Projects

Here's how to use the generated update sites in your Maven projects, step by step:

1. Create the update site and add the directory to your Maven `settings.xml` file (usually located in `~/.m2/settings.xml`). Here is the minimal `settings.xml` you need:

```xml
<?xml version="1.0"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
    <profiles>
        <profile>
            <id>notes-program</id>
            <properties>
                <notes-client-platform>file:///Path/To/UpdateSites/For/Notes_14.0FP2</notes-client-platform>
                <domino-platform-14>file:///Path/To/UpdateSites/For/Domino_14.0.0</domino-platform-14>
                <domino-platform-12>file:///Path/To/UpdateSites/For/Domino_12.0.2FP2</domino-platform-12>
                <dots-platform-12>file:///Path/To/UpdateSites/For/Domino_12.0.2FP6_DOTS</dots-platform-12>
                <dots-platform-14>file:///Path/To/UpdateSites/For/Domino_14.0FP2_DOTS</dots-platform-14>
            </properties>
        </profile>
    </profiles>
    <activeProfiles>
        <activeProfile>notes-program</activeProfile>
    </activeProfiles>
</settings>
```

As you can see, I set different directories for different platforms and versions. I recommend using properties for the directories so you can easily update multiple projects in one place.

2. Add OpenNTF repositories to your `pom.xml` file:

```xml
<pluginRepositories>
    <!-- The plugin is hosted in OpenNTF's Maven repository -->
    <pluginRepository>
        <id>artifactory.openntf.org</id>
        <name>artifactory.openntf.org</name>
        <url>https://artifactory.openntf.org/openntf</url>
    </pluginRepository>
</pluginRepositories>
```

3. Add Jesse's P2 Layout Resolver plugin to the `build` section of your `pom.xml`. This allows Maven to resolve the P2 repositories and their dependencies correctly.

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.openntf.maven</groupId>
            <artifactId>p2-layout-resolver</artifactId>
            <version>1.9.0</version>
            <extensions>true</extensions>
        </plugin>
    </plugins>
</build>
```

4. Add update sites as P2 repositories in your `pom.xml`. For example, I use plugins from both Domino 12 and DOTS in my [newly-mavenized XLogback project](https://github.com/sbasegmez/XLogback). Here is the relevant part of my `pom.xml`:

```xml
    <!-- Some OpenNTF-based plugins and packages are coming from these repositories -->
    <repositories>
        <repository>
            <id>domino.p2</id>   <!-- defines the groupId to be used below -->
            <url>${domino-platform-12}</url>
            <layout>p2</layout>
        </repository>
        <repository>
            <id>dots.p2</id>   <!-- defines the groupId to be used below -->
            <url>${dots-platform-12}</url>
            <layout>p2</layout>
        </repository>
    </repositories>

    <!-- ... -->

    <dependencies>
        <!-- DOTS and related dependencies -->
        <dependency>
            <groupId>dots.p2</groupId>
            <artifactId>com.ibm.dots</artifactId>
            <version>[2.0.0,)</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>domino.p2</groupId>
            <artifactId>com.ibm.notes.java.api.win32.linux</artifactId>
            <version>[9.0.0,)</version>
            <scope>provided</scope>
        </dependency>
    </dependencies>
```

Now I can use DOTS classes in my Java code. Maven is not the easiest way to develop Domino plugins, but it's a price I pay for [using IntelliJ IDEA as my IDE](2025-06-intellij-idea-domino-jnx-domino-development.md).

I hope you find the new features of the OpenNTF generate-domino-update-site project useful. If you have any questions or suggestions, feel free to open an issue on the [GitHub repository](https://github.com/OpenNTF/generate-domino-update-site).
