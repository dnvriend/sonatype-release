# Releasing to Sonatype
1. Follow the steps in the [Sonatype OSS Maven Repository Usage Guide](https://docs.sonatype.org/display/Repository/Sonatype+OSS+Maven+Repository+Usage+Guide)
2. Once you have created an account, and a ticket, and the ticket has been handled (you will get an email for that)
3. Read the scala-sbt [Deploying to Sonatype](http://www.scala-sbt.org/0.13/docs/Using-Sonatype.html) manual

# Launching sbt or activator
I use the activator script, but basically it is SBT. The following commands are handy:

* reload: Any settings or additions made to build.sbt and/or the ~/.sbt directory eg. adding files, will be reloaded.
* clean: removes any artifacts from the target directory
* compile: compiles the project
* test: runs tests
* package: package artifacts to $PROJECT_DIR/target/<scala-version> eg. $PROJECT_DIR/target/scala-2.10
* +package: cross builds/packages the artifacts with the configured scala versions
* publishLocal: publish artifacts to the local ~/.ivy directory
* publishM2: publish artifacts to the local ~/.m2 directory

# My additions/notes to the manual
The following are my additions/details to the steps that are already documented

## Add the gpg.sbt file 
Add a new file in the following directory:

	~/.sbt/0.13/plugins/gpg.sbt

And add the following entries:

	addSbtPlugin("com.typesafe.sbt" % "sbt-pgp" % "0.8.3")

	addSbtPlugin("org.xerial.sbt" % "sbt-sonatype" % "0.2.1")

## Create a key pair
* Read the following [pgp usage manual](http://www.scala-sbt.org/sbt-pgp/usage.html)

Type the following:

	> set pgpReadOnly := false

	> pgp-cmd gen-key
	Please enter the name associated with the key: dnvriend/akka-persistence-jdbc
	Please enter the email associated with the key: dnvriend@gmail.com
	Please enter the passphrase for the key: **********
	Please re-enter the passphrase for the key: **********
	[info] Creating a new PGP key, this could take a long time.
	[info] Public key := /Users/dennis/.sbt/gpg/pubring.asc
	[info] Secret key := /Users/dennis/.sbt/gpg/secring.asc
	[info] Please do not share your secret key.   Your public key is free to share.

## Publish the key to the keyserver pool:
Type the following:

	> set pgpReadOnly := false

	> pgp-cmd send-key dnvriend/akka-persistence-jdbc hkp://keyserver.ubuntu.com

## Cross building:
Read the [SBT - Cross Build documentation](http://www.scala-sbt.org/0.12.4/docs/Detailed-Topics/Cross-Build.html)
	
	> +package
	
	> +publish

## Publish to Sonatype Staging
Note the following:
	* This will publish to the staging area of Sonatype!
	* Do not forget to set organization := "com.github.yourname" in build.sbt
	* Do not forget to set the profileName := "com.github.yourname" in build.sbt

Type the following:

	> publishSigned

or

	> +publishSigned
	[info] Setting version to 2.10.4
	[info] Set current project to akka-persistence-jdbc (in build file:/Users/dennis/projects/akka-persistence-jdbc/)
	[info] Updating {file:/Users/dennis/projects/akka-persistence-jdbc/}akka-persistence-jdbc...
	[info] Packaging /Users/dennis/projects/akka-persistence-jdbc/target/scala-2.10/akka-persistence-jdbc_2.10-1.0.0-sources.jar ...
	[info] Done packaging.
	[info] Wrote /Users/dennis/projects/akka-persistence-jdbc/target/scala-2.10/akka-persistence-jdbc_2.10-1.0.0.pom
	[info] Resolving org.fusesource.jansi#jansi;1.4 ...
	[info] Done updating.
	[info] :: delivering :: com.github.dnvriend#akka-persistence-jdbc_2.10;1.0.0 :: 1.0.0 :: release :: Thu Jul 03 20:16:28 CEST 2014
	[info] 	delivering ivy file to /Users/dennis/projects/akka-persistence-jdbc/target/scala-2.10/ivy-1.0.0.xml
	[info] Main Scala API documentation to /Users/dennis/projects/akka-persistence-jdbc/target/scala-2.10/api...
	[info] Packaging /Users/dennis/projects/akka-persistence-jdbc/target/scala-2.10/akka-persistence-jdbc_2.10-1.0.0.jar ...
	[info] Done packaging.
	model contains 48 documentable templates
	[info] Main Scala API documentation successful.
	[info] Packaging /Users/dennis/projects/akka-persistence-jdbc/target/scala-2.10/akka-persistence-jdbc_2.10-1.0.0-javadoc.jar ...
	[info] Done packaging.
	Please enter PGP passphrase (or ENTER to abort): **********
	[info] 	published akka-persistence-jdbc_2.10 to https://oss.sonatype.org/service/local/staging/deploy/maven2/com/github/dnvriend/akka-persistence-jdbc_2.10/1.0.0/akka-persistence-jdbc_2.10-1.0.0.pom
	[info] 	published akka-persistence-jdbc_2.10 to https://oss.sonatype.org/service/local/staging/deploy/maven2/com/github/dnvriend/akka-persistence-jdbc_2.10/1.0.0/akka-persistence-jdbc_2.10-1.0.0-sources.jar.asc
	[info] 	published akka-persistence-jdbc_2.10 to https://oss.sonatype.org/service/local/staging/deploy/maven2/com/github/dnvriend/akka-persistence-jdbc_2.10/1.0.0/akka-persistence-jdbc_2.10-1.0.0-javadoc.jar.asc
	[info] 	published akka-persistence-jdbc_2.10 to https://oss.sonatype.org/service/local/staging/deploy/maven2/com/github/dnvriend/akka-persistence-jdbc_2.10/1.0.0/akka-persistence-jdbc_2.10-1.0.0.jar.asc
	[info] 	published akka-persistence-jdbc_2.10 to https://oss.sonatype.org/service/local/staging/deploy/maven2/com/github/dnvriend/akka-persistence-jdbc_2.10/1.0.0/akka-persistence-jdbc_2.10-1.0.0-sources.jar
	[info] 	published akka-persistence-jdbc_2.10 to https://oss.sonatype.org/service/local/staging/deploy/maven2/com/github/dnvriend/akka-persistence-jdbc_2.10/1.0.0/akka-persistence-jdbc_2.10-1.0.0-javadoc.jar
	[info] 	published akka-persistence-jdbc_2.10 to https://oss.sonatype.org/service/local/staging/deploy/maven2/com/github/dnvriend/akka-persistence-jdbc_2.10/1.0.0/akka-persistence-jdbc_2.10-1.0.0.pom.asc
	[info] 	published akka-persistence-jdbc_2.10 to https://oss.sonatype.org/service/local/staging/deploy/maven2/com/github/dnvriend/akka-persistence-jdbc_2.10/1.0.0/akka-persistence-jdbc_2.10-1.0.0.jar
	[success] Total time: 35 s, completed 3-jul-2014 20:17:02
	[info] Setting version to 2.11.1
	[info] Set current project to akka-persistence-jdbc (in build file:/Users/dennis/projects/akka-persistence-jdbc/)
	[info] Packaging /Users/dennis/projects/akka-persistence-jdbc/target/scala-2.11/akka-persistence-jdbc_2.11-1.0.0-sources.jar ...
	[info] Updating {file:/Users/dennis/projects/akka-persistence-jdbc/}akka-persistence-jdbc...
	[info] Done packaging.
	[info] Wrote /Users/dennis/projects/akka-persistence-jdbc/target/scala-2.11/akka-persistence-jdbc_2.11-1.0.0.pom
	[info] Resolving jline#jline;2.11 ...
	[info] Done updating.
	[info] :: delivering :: com.github.dnvriend#akka-persistence-jdbc_2.11;1.0.0 :: 1.0.0 :: release :: Thu Jul 03 20:17:02 CEST 2014
	[info] 	delivering ivy file to /Users/dennis/projects/akka-persistence-jdbc/target/scala-2.11/ivy-1.0.0.xml
	[info] Main Scala API documentation to /Users/dennis/projects/akka-persistence-jdbc/target/scala-2.11/api...
	[info] Packaging /Users/dennis/projects/akka-persistence-jdbc/target/scala-2.11/akka-persistence-jdbc_2.11-1.0.0.jar ...
	[info] Done packaging.
	model contains 48 documentable templates
	[info] Main Scala API documentation successful.
	[info] Packaging /Users/dennis/projects/akka-persistence-jdbc/target/scala-2.11/akka-persistence-jdbc_2.11-1.0.0-javadoc.jar ...
	[info] Done packaging.
	[info] 	published akka-persistence-jdbc_2.11 to https://oss.sonatype.org/service/local/staging/deploy/maven2/com/github/dnvriend/akka-persistence-jdbc_2.11/1.0.0/akka-persistence-jdbc_2.11-1.0.0.pom
	[info] 	published akka-persistence-jdbc_2.11 to https://oss.sonatype.org/service/local/staging/deploy/maven2/com/github/dnvriend/akka-persistence-jdbc_2.11/1.0.0/akka-persistence-jdbc_2.11-1.0.0-sources.jar.asc
	[info] 	published akka-persistence-jdbc_2.11 to https://oss.sonatype.org/service/local/staging/deploy/maven2/com/github/dnvriend/akka-persistence-jdbc_2.11/1.0.0/akka-persistence-jdbc_2.11-1.0.0-javadoc.jar.asc
	[info] 	published akka-persistence-jdbc_2.11 to https://oss.sonatype.org/service/local/staging/deploy/maven2/com/github/dnvriend/akka-persistence-jdbc_2.11/1.0.0/akka-persistence-jdbc_2.11-1.0.0.jar.asc
	[info] 	published akka-persistence-jdbc_2.11 to https://oss.sonatype.org/service/local/staging/deploy/maven2/com/github/dnvriend/akka-persistence-jdbc_2.11/1.0.0/akka-persistence-jdbc_2.11-1.0.0-sources.jar
	[info] 	published akka-persistence-jdbc_2.11 to https://oss.sonatype.org/service/local/staging/deploy/maven2/com/github/dnvriend/akka-persistence-jdbc_2.11/1.0.0/akka-persistence-jdbc_2.11-1.0.0-javadoc.jar
	[info] 	published akka-persistence-jdbc_2.11 to https://oss.sonatype.org/service/local/staging/deploy/maven2/com/github/dnvriend/akka-persistence-jdbc_2.11/1.0.0/akka-persistence-jdbc_2.11-1.0.0.pom.asc
	[info] 	published akka-persistence-jdbc_2.11 to https://oss.sonatype.org/service/local/staging/deploy/maven2/com/github/dnvriend/akka-persistence-jdbc_2.11/1.0.0/akka-persistence-jdbc_2.11-1.0.0.jar
	[success] Total time: 14 s, completed 3-jul-2014 20:17:16
	[info] Setting version to 2.10.4
	[info] Set current project to akka-persistence-jdbc (in build file:/Users/dennis/projects/akka-persistence-jdbc/)
	
## Show the list of staging repositories.

	> sonatypeList
	[info] Nexus repository URL: https://oss.sonatype.org/service/local
	[info] Reading staging repository profiles...
	[info] Staging repository profiles:
	[info] [comgithubdnvriend-1000] status:open, profile:com.github.dnvriend(5fd6517e0f0a60)
	[success] Total time: 3 s, completed 3-jul-2014 20:19:35

## Show the staging activity logs

	> sonatypeLog
	[info] Nexus repository URL: https://oss.sonatype.org/service/local
	[info] Reading staging repository profiles...
	[info] Checking activity logs of comgithubdnvriend-1000 ...
	[info] Staging activities of [comgithubdnvriend-1000] status:open, profile:com.github.dnvriend(5fd6517e0f0a60):
	[info] Activity open started:2014-07-03T13:16:50.604-05:00, stopped:2014-07-03T13:16:54.277-05:00
	[success] Total time: 4 s, completed 3-jul-2014 20:22:08

## Close and promote a staging repository
Read the [readme.md of the sbt-sonatype plugin](https://github.com/xerial/sbt-sonatype) and [chapter 8: Release it of the Sonatype OSS Maven Repository Usage Guide](https://docs.sonatype.org/display/Repository/Sonatype+OSS+Maven+Repository+Usage+Guide#SonatypeOSSMavenRepositoryUsageGuide-8a.ReleaseIt). 

When you release the artifacts from the staging repository of sonatype, it takes 	

Type the following:

	> sonatypeRelease
	[info] Nexus repository URL: https://oss.sonatype.org/service/local
	[info] Reading staging repository profiles...
	[info] Reading staging profiles...
	[info] Closing staging repository [comgithubdnvriend-1000] status:open, profile:com.github.dnvriend(5fd6517e0f0a60)
	[info] Checking activity logs of comgithubdnvriend-1000 ...
	[info] Close process is in progress ...
	[info] Checking activity logs of comgithubdnvriend-1000 ...
	[info] Close process is in progress ...
	[info] Checking activity logs of comgithubdnvriend-1000 ...
	[info] Close process is in progress ...
	[info] Checking activity logs of comgithubdnvriend-1000 ...
	[info] Closed successfully
	[info] Promoting staging repository [comgithubdnvriend-1000] status:closed, profile:com.github.dnvriend(5fd6517e0f0a60)
	[info] Checking activity logs of comgithubdnvriend-1000 ...
	[info] Release process is in progress ...
	[info] Checking activity logs of comgithubdnvriend-1000 ...
	[info] Release process is in progress ...
	[info] Checking activity logs of comgithubdnvriend-1000 ...
	[info] Release process is in progress ...
	[info] Checking activity logs of comgithubdnvriend-1000 ...
	[info] Promoted successfully
	[info] Dropping staging repository [comgithubdnvriend-1000] status:released, profile:com.github.dnvriend(5fd6517e0f0a60)
	[info] Dropped successfully: comgithubdnvriend-1000
	[success] Total time: 56 s, completed 3-jul-2014 20:26:59

## Comment on the ticket you have created to create the repository
Check the mailbox for the ticket about the repository. Click on the link, login to Jira and add a comment to the ticket, asking to review the artifacts and activate syncing to Maven Central.
After the review, automatic syncing will be enabled and future releases will be automatically.

## Sonatype repository
The artifact should be available in the following sonatype repository, without syncing:

    https://oss.sonatype.org/content/groups/public/com/github/dnvriend/
    
