---
layout: post
section-type: post
title: Getting started with AOSP
category: tech
tags: [ 'android' ]
---

#### **Hardware requirements**

Minimal requirements needed for your workstation.

1. 64-bit environment
2. At least 100GB of disk space. If you conduct multiple builds you will need more space.
3. At least 4GB of RAM

#### **Software requirements**

You can build Android on Linux or Mac. But I prefer Linux specifically Ubuntu LTS(Long Term Support), because AOSP master branch is developed and tested on Ubuntu LTS. So software requirements are,

1. OS - Ubuntu 12.04/14.04
2. Java Development Kit(JDK) - OpenJDK 7
3. Python 2.7
4. GNU Make 3.82
5. Git 1.7 or newer

### Setting up build environment
#### **Installing JDK**

Since we are using Linux, I will give instructions to install the OpenJDK. Install OpenJDK using below commands.
<pre><code data-trim class="bash">
$ sudo apt-get update
$ sudo apt-get install openjdk-7-jdk
</code></pre>

Update the default Java version by running
<pre><code data-trim class="bash">
$ sudo update-alternatives --config java
$ sudo update-alternatives --config javac
</code></pre>

<br/>
#### **Install required packages**
<pre><code data-trim class="bash">
$ sudo apt-get -y install git-core gnupg flex bison gperf build-essential zip curl zlib1g-dev gcc-multilib g++-multilib 
$ sudo apt-get -y install libc6-dev-i386 lib32ncurses5-dev x11proto-core-dev libx11-dev lib32z-dev ccache libgl1-mesa-dev libxml2-utils 
$ sudo apt-get -y install xsltproc unzip mtd-utils u-boot-tools lzop liblzo2-2 liblzo2-dev zlib1g-dev liblz-dev uuid uuid-dev
</code></pre>

<br/>
### Downloading the source
#### **Installing Repo**
Repo is a tool which is used to handle a number of Git repositories. Repo is developed for handling the Android Development. 

To install Repo,
<ol>
<li>Make a bin directory in your home directory</li>
<pre><code data-trim class="bash">
$ mkdir ~/bin
$ PATH=~/bin:$PATH
</code></pre>

<li>Download Repo tool and give needed permissions</li>
<pre><code data-trim class="bash">
$ curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
$ chmod a+x ~/bin/repo
</code></pre>

<li>Initialize repo client</li>
<pre><code data-trim class="bash">
$ mkdir ~/working_directory
$ cd ~/working_directory/
$ mkdir my_android
$ cd my_android/ 
$ repo init -u https://android.googlesource.com/platform/manifest -b  android-6.0.1_r3
$ repo sync
</code></pre>
</ol>

repo sync will take some time to execute, because it is downloading whole source of Android OS. It will be nearly 50GB - 60GB. So you can have a tea break. Once the repo sync is executed then you have the Android Source. 
