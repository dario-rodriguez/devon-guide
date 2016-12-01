:toc: macro
toc::[]

# Compatibility guide for JAVA and TOMCAT

== What this guide contains?

As we have migrated to eclipse neon , which mandatorily uses _Java8_ ,if any project or users want to use _JAVA7_, this guide helps them in doing so.
We also have integrated _TOMCAT8_ with the devon distribution and oasp4j IDE. So it also describes steps to use _Tomcat7_ with the distributions.

== Using JAVA7

=== Download JAVA7.
One can download java7 from http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html[here].
Once java7 is downloaded,run .exe and follow instructions.

=== Change "installed jre" in eclipse

When you open eclipse, follow steps mentioned below to change installed jre in preferences :

===== 1.Go to Preferences

image::images/compatibility-guide-for-java7/compatibility-guide-for-java7-01.png["Go to Preferences",width="450",link="images/compatibility-guide-for-java7/compatibility-guide-for-java7-01.png"]


==== 2.Go to Installed Jres

image::images/compatibility-guide-for-java7/compatibility-guide-for-java7-02.png["Go to Installed Jres",width="450",link="images/compatibility-guide-for-java7/compatibility-guide-for-java7-02.png"]


==== 3.Browse for java7
image::images/compatibility-guide-for-java7/compatibility-guide-for-java7-03.png["Browse for java7",width="450",link="images/compatibility-guide-for-java7/compatibility-guide-for-java7-03.png"]


==== 4. Apply changes
image::images/compatibility-guide-for-java7/compatibility-guide-for-java7-04.png["Apply changes",width="450",link="images/compatibility-guide-for-java7/compatibility-guide-for-java7-04.png"]


After following above instructions, you can import projects or create new ones, and build using java7.

== Using java8
One can use distribution as is, and there is no extra configuration needed for java8.

== Using Tomcat8

As mentioned earlier in the guide, distribution comes with _Tomcat8_ by default, so no changes are required to run applications with _tomcat8_.

== Using Tomcat7 for deploying.

You can download tomcat externally and deploy war on it.
For more information on this ,please visit this https://github.com/devonfw/devon-guide/wiki/getting-started-deployment-on-tomcat#deploy-on-tomcat-7[link].

== Linux and Windows Compatibility

So the above mentioned steps on _java7 and tomcat7_ compatibility apply to devonfw distributions of Windows OS as well as  Linux .

Linux and Windows distribution works by default on *JAVA8* and *TOMCAT8*.

















