---
layout: post
title: "ant problem"
description: ""
category: JAVA
tags: [Ant, Java]
---
{% include JB/setup %}
 I follow the operations in the book but I failed to start ant.

 

Operations:

1) Write a simple java program named MyDate as follows:

import java.util.Date;
public class MyDate{
	public static void main(String [] args){
		Date now = new Date();
		System.out.println(now.toString());
	}
} 

 

2) Create a XML file and write follows words in it:

<?xml version="1.0" encoding="UTF-8" ?>
<project name="MyDate" default="run" basedir=".">
	<property name="src" value="src"/>
	<property name="dest" value="build/classes"/>
	<property name="lib" value="build/lib"/>
	<propery name="date_jar" value="build/lib/date.jar"/>
	<target name="init">
		<mkdir dir="${dest}"/>
		<mkdir dir="${lib}"/>
	</target>
	<target name="compile" depends="init">
		<javac srcdir="${src}" destdir="${dest}"/>
	</target>
	<target name="build">
		<jar destfile="date_jar" basedir="${dest}" depends="compile">
			<manifest>
				<attribute name="Main-Class" value="MyDate"/>
			</manifest>
		</jar>
	</target>
	<target name="run" depends="build">
		<java classname="MyDate" classpath="${date_jar}"/>
	</target>
	<target name="clean">
		<delete file="${date_jar}"/>
		<delete file="${dest}"/>
		<delete file="${lib}"/>
	</target>
</project> 

3) Open the cmd window and type the command as follows:



Now the directory of the project is as follows:



The MyDate.java is under the folder src

4) Start use ant



 

Tips: I don't know why would it happen and I've serched many materials but still can't find the solution.

 

I've got the solutions!!!!! 8.18

modify the build.xml as follows:



 <project name="MyDate" default="run" basedir=".">
    <description>
        simple example build file
    </description>
  <!-- set global properties for this build -->
  <property name="src" location="src"/>
  <property name="dest" location="build/classes"/>
  <property name="lib" location="build/lib"/>

  <target name="init">
    <!-- Create the time stamp -->
    <!-- tstamp/ -->
    <!-- Create the build directory structure used by compile -->
    <mkdir dir="${dest}"/>
    <mkdir dir="${lib}"/>
  </target>

  <target name="compile" depends="init"
        description="compile the source " >
    <!-- Compile the java code from ${src} into ${build} -->
    <javac srcdir="${src}" destdir="${dest}"/>
  </target>

  <target name="build" depends="compile"
        description="generate the distribution" >

    <!-- Put everything in ${build} into the MyProject-${DSTAMP}.jar file -->
    <jar jarfile="${lib}/MyDate.jar" basedir="${dest}">
    	<manifest>
    		<attribute name="Main-Class" value="MyDate"/>
    	</manifest>
    </jar>
  </target>
  <target name="run" depends="build">
	<java classname="MyDate" classpath="${lib}/MyDate.jar"/>
  </target>

  <target name="clean"
        description="clean up" >
    <!-- Delete the ${build} and ${dist} directory trees -->
    <delete dir="${dest}"/>
    <delete dir="${lib}"/>
  </target>
</project>


The lesson I've got here is that: Don't always believe the book. Just go to the offical site to get the solution.

I got a example from the site and modified it and then I can run ant! 

So cool!
