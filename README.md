# How to deploy on tomcat server using maven?  ![](https://img.shields.io/badge/DevOps-Maven-brightgreen)

Automated deployment of WAR files to Tomcat with Maven is a surprisingly straightforward task. If you have Maven and Tomcat installed, use a Maven project object model (POM) to describe the structure of your web application and connect to Maven Central -- where you download the Maven-Tomcat plugin, a Maven Tomcat deploy is a straightforward affair.  With access to the Maven-Tomcat plugin, a WAR file deployment simply involves a few XML file edits, followed by a Maven build command.


## Maven Tomcat deploys example prerequisites 

* You must have JDK 1.8 or newer installed with JAVA_HOME path (Anyway it is included in this tutorial) ✔

## Table of Contents
* Java installation and configuration ---> [starts here](#java-setup)
* Maven Server configuration ---> [starts here](#Maven-setup)  
* Tomcat Server configuration ---> [starts here](#Tomcat-setup)
* deploy on tomcat server using maven ---> [starts here](#deploying)

## Java setup 
⚠⚠⚠ **Must be done in both machines** ⚠⚠⚠


+ First install **JDK 1.8** or newer and define **JAVA_HOME** path

```bash
# yum install java-1.8* -y
```

+ Get the java path

```bash
# find /usr/lib/jvm/java-1.8* | head -n 3

/usr/lib/jvm/java-1.8.0
/usr/lib/jvm/java-1.8.0-openjdk
/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.302.b08-0.amzn2.0.1.x86_64
```
copy  **/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.302.b08-0.amzn2.0.1.x86_64**


+ Add path to  .bash_profile

```bash
# vi ~/.bash_profile
```


```python
############ Edit this file #############

# .bash_profile

# Get the aliases and functions
if [ -f ~/.bashrc ]; then
        . ~/.bashrc
fi

##### Assign JAVA_HOME below ######
JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.302.b08-0.amzn2.0.1.x86_64

# User specific environment and startup programs

##### Edit PATH below ######
PATH=$PATH:$HOME/bin:$JAVA_HOME

export PATH
~
~
~
~
~
"~/.bash_profile" 18L, 341C
```
* Restart **.bash_profile**
```bash
source ~/.bash_profile
```
<------------------**OR**------------------>

```bash
. ~/.bash_profile
```


* check the updated path 
```bash
echo $PATH
/sbin:/bin:/usr/sbin:/usr/bin:/root/bin:/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.302.b08-0.amzn2.0.1.x86_64
```

# Maven Setup
* Download and extract **Maven** 
```bash
# Download apache-maven-3.8 from here
wget https://dlcdn.apache.org/maven/maven-3/3.8.2/binaries/apache-maven-3.8.2-bin.tar.gz

# Check the downloaded file
ls 
apache-maven-3.8.2-bin.tar.gz  file1 file2
dir1   dir2

# Extract the apache-maven-3.8.2-bin.tar.gz  file
tar -xzvf apache-maven-3.8.2-bin.tar.gz 
apache-maven-3.8.2/README.txt
apache-maven-3.8.2/LICENSE
apache-maven-3.8.2/NOTICE
apache-maven-3.8.2/lib/
apache-maven-3.8.2/lib/cdi-api.license
apache-maven-3.8.2/lib/commons-cli.license
apache-maven-3.8.2/lib/commons-io.license
apache-maven-3.8.2/lib/commons-lang3.license
apache-maven-3.8.2/lib/guava.license
apache-maven-3.8.2/lib/guice.license
apache-maven-3.8.2/lib/jansi.license
apache-maven-3.8.2/lib/javax.inject.license
apache-maven-3.8.2/lib/jcl-over-slf4j.license
apache-maven-3.8.2/lib/jsoup.license
apache-maven-3.8.2/lib/jsr250-api.license
apache-maven-3.8.2/lib/org.eclipse.sisu.inject.license
apache-maven-3.8.2/lib/org.eclipse.sisu.plexus.license
apache-maven-3.8.2/lib/plexus-cipher.license
apache-maven-3.8.2/lib/plexus-component-annotations.license
apache-maven-3.8.2/lib/plexus-interpolation.license
apache-maven-3.8.2/lib/plexus-sec-dispatcher.license
apache-maven-3.8.2/lib/plexus-utils.license
........
# check the extracted folder name
ls 
apache-maven-3.8.2  apache-maven-3.8.2-bin.tar.gz  file1 file2
dir1   dir2 
```
* Move inside to apache-maven-3.8.2 directory and **copy the path**
```bash
cd apache-maven-3.8.2
# find the current working directory 
pwd
/tmp/apache-maven-3.8.2/    ---------->it may change in your case
```
+ Add path to  .bash_profile

```bash
# vi ~/.bash_profile
```


```bash
############ Edit this file #############

# .bash_profile

# Get the aliases and functions
if [ -f ~/.bashrc ]; then
        . ~/.bashrc
fi

##### Assign JAVA_HOME below ######
JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.302.b08-0.amzn2.0.1.x86_64

M2_HOME=/tmp/apache-maven-3.8.2
M2=/tmp/apache-maven-3.8.2/bin

# User specific environment and startup programs

##### Edit PATH below ######
PATH=$PATH:$HOME/bin:$JAVA_HOME:$M2_HOME:$M2

export PATH
~
~
~
~
~
"~/.bash_profile" 18L, 341C
```
* Restart **.bash_profile**
```bash
source ~/.bash_profile
```
<------------------**OR**------------------>

```bash
. ~/.bash_profile
``` 




# Tomcat Setup
**[JAVA required]**

* Installing Tomcat
```bash
sudo yum install tomcat-webapps tomcat-admin-webapps tomcat-docs-webapp tomcat-javadoc
```
* Restarting and enabling Tomcat
```bash
sudo systemctl start tomcat
sudo systemctl enable tomcat
```

# Checking all pre-configurations 
- [x] Open port 8080 on both machines
- [x] Java installed in both machines
- [x] JAVA_HOME path added to .bash_profile
- [x] Mavn installed
     - [x] Add M2_HOME and M2 path in .bash_profile
     - [ ] ```Creating first Maven project```
     - [ ] ```Run deploy command```

- [x] Installing Apache-Tomcat
     - [ ] ```Add a new user with deployment rights to Tomcat```

# Creating first Maven project
* first download an archetype from maven
```bash
mvn archetype:generate | grep maven-archetype-webapp
576: remote -> com.haoxuer.maven.archetype:maven-archetype-webapp (a simple maven archetype)
712: remote -> com.lodsve:lodsve-maven-archetype-webapp (Lodsve Maven Archetype Webapp)
1830: remote -> org.apache.maven.archetypes:maven-archetype-webapp (An archetype which contains a sample Maven Webapp project.)
2083: remote -> org.bytesizebook.com.guide.boot:maven-archetype-webapp (An archetype to create the starting code for the first three chapters of Guide to Web Development with Java, 2nd edition.)
```
> Note the number of web app which is built by org.apache.archetype from the list

> For us simply maven webapp is the perfect number: **1830**

> Now rerun the command without grep and fill the details 
```bash
mvn archetype:generate
.
.
.
.
.
2985: remote -> xyz.luan.generator:xyz-gae-generator (-)
2986: remote -> xyz.luan.generator:xyz-generator (-)
2987: remote -> za.co.absa.hyperdrive:component-archetype (-)
2988: remote -> za.co.absa.hyperdrive:component-archetype_2.11 (-)
2989: remote -> za.co.absa.hyperdrive:component-archetype_2.12 (-)
Choose a number or apply filter (format: [groupId:]artifactId, case sensitive contains): 1825: 1830
Choose org.apache.maven.archetypes:maven-archetype-webapp version:
1: 1.0-alpha-1
2: 1.0-alpha-2
3: 1.0-alpha-3
4: 1.0-alpha-4
5: 1.0
6: 1.3
7: 1.4
Choose a number: 7: 7
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/archetypes/maven-archetype-webapp/1.4/maven-archetype-webapp-1.4.pom
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/archetypes/maven-archetype-webapp/1.4/maven-archetype-webapp-1.4.pom (1.4 kB at 20 kB/s)
Downloading from central: https://repo.maven.apache.org/maven2/org/apache/maven/archetypes/maven-archetype-webapp/1.4/maven-archetype-webapp-1.4.jar
Downloaded from central: https://repo.maven.apache.org/maven2/org/apache/maven/archetypes/maven-archetype-webapp/1.4/maven-archetype-webapp-1.4.jar (6.8 kB at 57 kB/s)
Define value for property 'groupId': com.democompany.repo
Define value for property 'artifactId': first-demo-project
Define value for property 'version' 1.0-SNAPSHOT: :
Define value for property 'package' com.democompany.repo: :
Confirm properties configuration:
groupId: com.democompany.repo
artifactId: first-demo-project
version: 1.0-SNAPSHOT
package: com.democompany.repo
 Y: : y
[INFO] ----------------------------------------------------------------------------
[INFO] Using following parameters for creating project from Archetype: maven-archetype-webapp:1.4
[INFO] ----------------------------------------------------------------------------
[INFO] Parameter: groupId, Value: com.democompany.repo
[INFO] Parameter: artifactId, Value: first-demo-project
[INFO] Parameter: version, Value: 1.0-SNAPSHOT
[INFO] Parameter: package, Value: com.democompany.repo
[INFO] Parameter: packageInPathFormat, Value: com/democompany/repo
[INFO] Parameter: package, Value: com.democompany.repo
[INFO] Parameter: version, Value: 1.0-SNAPSHOT
[INFO] Parameter: groupId, Value: com.democompany.repo
[INFO] Parameter: artifactId, Value: first-demo-project
[INFO] Project created from Archetype in dir: /home/ec2-user/first-demo-project
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  03:31 min
[INFO] Finished at: 2021-09-30T09:25:12Z
[INFO] ------------------------------------------------------------------------
```
* check for files in the current folder
```bash
ls
first-demo-project
cd first-demo-project/
ls
pom.xml  src
```



 


# Deploying
## 1. In Tomcat Server
* **Step1:** Add a new user with deployment rights to Tomcat
> To perform a Maven Tomcat deploy of a WAR file you must first set up a user in Tomcat with the appropriate rights. You can do this with an edit of the tomcat-users.xml file
```bash
sudo vi /usr/share/tomcat/conf/tomcat-users.xml
```
Edit the following in tomcat-users.xml 
```xml
	<?xml version='1.0' encoding='utf-8'?>
<!--
  Licensed to the Apache Software Foundation (ASF) under one or more
  contributor license agreements.  See the NOTICE file distributed with
  this work for additional information regarding copyright ownership.
  The ASF licenses this file to You under the Apache License, Version 2.0
  (the "License"); you may not use this file except in compliance with
  the License.  You may obtain a copy of the License at

      http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License.
-->
<tomcat-users>
<!--
  NOTE:  By default, no user is included in the "manager-gui" role required
  to operate the "/manager/html" web application.  If you wish to use this app,
  you must define such a user - the username and password are arbitrary. It is
  strongly recommended that you do NOT use one of the users in the commented out
  section below since they are intended for use with the examples web
  application.
-->
<!--
  NOTE:  The sample user and role entries below are intended for use with the
  examples web application. They are wrapped in a comment and thus are ignored
  when reading this file. If you wish to configure these users for use with the
  examples web application, do not forget to remove the <!.. ..> that surrounds
  them. You will also need to set the passwords to something appropriate.
-->
<!--
  <role rolename="tomcat"/>
  <role rolename="role1"/>
  <user username="tomcat" password="<must-be-changed>" roles="tomcat"/>
  <user username="both" password="<must-be-changed>" roles="tomcat,role1"/>
  <user username="role1" password="<must-be-changed>" roles="role1"/>
-->

<!-- <role rolename="admin"/>-->
<!-- <role rolename="admin-gui"/> -->
<!-- <role rolename="admin-script"/> -->
<!-- <role rolename="manager"/> -->
<!-- <role rolename="manager-gui"/> -->
<!-- <role rolename="manager-script"/> -->
<!--<role rolename="manager-jmx"/>-->
<!-- <role rolename="manager-status"/> -->

<!-- <user username="admin" password="admin" roles="admin, manager-gui, manager-script, manager-jmx" /> -->

############ Add This #############
<role rolename="manager-gui"/>
<role rolename="manager-script"/>
<role rolename="manager-jmx"/>
<role rolename="manager-status"/>
<user username="admin" password="admin" roles="manager-script, manager-jmx, manager-status"/>
<user username="deployer" password="deployer" roles="manager-script"/>
<user username="tomcat" password="s3cret" roles="manager-gui"/>
############ Add This #############


</tomcat-users>
```
* Save and exit the file. Restart the Tomcat service for the changes to take effect:
```bash
sudo systemctl restart tomcat
```


## 2. In Maven server
* **Step2:** Tell Maven about the Tomcat deploy user
> Create **settings.xml** for credentials in  ~/.m2 or 
```bash
 vi ~/.m2/settings.xml
```
Edit the file
```xml
	<?xml version="1.0" encoding="UTF-8"?>

	 <settings>
 		<servers>

 			<server>
 				<id>TomcatServer</id>
 				<username>admin</username>   
 				<password>password</password>
			 <!-- must match with  username & password,we created with script roles--->
 			</server>
	
 		</servers>
	 </settings>
```

* **Step3:**  Update **pom.xml** code to add maven plugin to copy artefacts onto tomcat this should be under <build> section
> Go to the place where we download the **first-demo-project** 
```bash
cd /home/user/first-demo-project/
ls
pom.xml  src
```
>Edit the file **pom.xml**
```bash
vi pom.xml
```
>find </plugin> ----------> end of any other plugin and add the code for our plugin

  ```  
          <plugin>
                        <groupId>org.apache.tomcat.maven</groupId>
                        <artifactId>tomcat7-maven-plugin</artifactId>
                        <version>2.2</version>
                        <configuration>
                                <url>http://localhost:8080/manager/text</url>
                                <server>TomcatServer</server>
                                <path>/helloworld-webapp</path>
                        </configuration>
         </plugin>
```

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.democompany.repo</groupId>
  <artifactId>first-demo-project</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>war</packaging>

  <name>first-demo-project Maven Webapp</name>
  <!-- FIXME change it to the project's website -->
  <url>http://www.example.com</url>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.7</maven.compiler.source>
    <maven.compiler.target>1.7</maven.compiler.target>
  </properties>

  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>
  </dependencies>

  <build>
    <finalName>first-demo-project</finalName>
    <pluginManagement><!-- lock down plugins versions to avoid using Maven defaults (may be moved to parent pom) -->
      <plugins>
        <plugin>
          <artifactId>maven-clean-plugin</artifactId>
          <version>3.1.0</version>
        </plugin>

############################ ADD This by changing Localhost and path(normally used as projectname)################# 
         
          <plugin>
                        <groupId>org.apache.tomcat.maven</groupId>
                        <artifactId>tomcat7-maven-plugin</artifactId>
                        <version>2.2</version>
                        <configuration>
                                <url>http://localhost:8080/manager/text</url>
                                <server>TomcatServer</server>
                                <path>/helloworld-webapp</path>
                        </configuration>
         </plugin>



##################################################################################################################


        <!-- see http://maven.apache.org/ref/current/maven-core/default-bindings.html#Plugin_bindings_for_war_packaging -->
        <plugin>
          <artifactId>maven-resources-plugin</artifactId>
          <version>3.0.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.8.0</version>
        </plugin>
        <plugin>
          <artifactId>maven-surefire-plugin</artifactId>
          <version>2.22.1</version>
        </plugin>
        <plugin>
          <artifactId>maven-war-plugin</artifactId>
          <version>3.2.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-install-plugin</artifactId>
          <version>2.5.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-deploy-plugin</artifactId>
          <version>2.8.2</version>
        </plugin>
      </plugins>
    </pluginManagement>
  </build>
</project>
```
* Step4:  Build and the project on tomcat
> with in the same directory (pom.xml)
```bash
mvn tomcat7:deploy
```
**OR**
```bash
mvn tomcat7:redeploy
```

```bash
[INFO] Scanning for projects...
[INFO]
[INFO] --------------< com.democompany.repo:first-demo-project >---------------
[INFO] Building first-demo-project Maven Webapp 1.0-SNAPSHOT
[INFO] --------------------------------[ war ]---------------------------------
[INFO]
[INFO] >>> tomcat7-maven-plugin:2.2:deploy (default-cli) > package @ first-demo-project >>>
[INFO]
[INFO] --- maven-resources-plugin:3.0.2:resources (default-resources) @ first-demo-project ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory /home/ec2-user/first-demo-project/src/main/resources
[INFO]
[INFO] --- maven-compiler-plugin:3.8.0:compile (default-compile) @ first-demo-project ---
[INFO] No sources to compile
[INFO]
[INFO] --- maven-resources-plugin:3.0.2:testResources (default-testResources) @ first-demo-project ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory /home/ec2-user/first-demo-project/src/test/resources
[INFO]
[INFO] --- maven-compiler-plugin:3.8.0:testCompile (default-testCompile) @ first-demo-project ---
[INFO] No sources to compile
[INFO]
[INFO] --- maven-surefire-plugin:2.22.1:test (default-test) @ first-demo-project ---
[INFO] No tests to run.
[INFO]
[INFO] --- maven-war-plugin:3.2.2:war (default-war) @ first-demo-project ---
[INFO] Packaging webapp
[INFO] Assembling webapp [first-demo-project] in [/home/ec2-user/first-demo-project/target/first-demo-project]
[INFO] Processing war project
[INFO] Copying webapp resources [/home/ec2-user/first-demo-project/src/main/webapp]
[INFO] Webapp assembled in [43 msecs]
[INFO] Building war: /home/ec2-user/first-demo-project/target/first-demo-project.war
[INFO]
[INFO] <<< tomcat7-maven-plugin:2.2:deploy (default-cli) < package @ first-demo-project <<<
[INFO]
[INFO]
[INFO] --- tomcat7-maven-plugin:2.2:deploy (default-cli) @ first-demo-project ---
[INFO] Deploying war to http://18.188.255.27:8080/helloworld-webapp
Uploading: http://18.188.255.27:8080/manager/text/deploy?path=%2Fhelloworld-webapp
Uploaded: http://18.188.255.27:8080/manager/text/deploy?path=%2Fhelloworld-webapp (3 KB at 2237.3 KB/sec)

[INFO] tomcatManager status code:200, ReasonPhrase:OK
[INFO] FAIL - Application already exists at path /helloworld-webapp
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  4.960 s
[INFO] Finished at: 2021-09-30T10:12:19Z
[INFO] ------------------------------------------------------------------------
```
# Check your Tomcat server
* Go to ip-v4:8080
* Clik on Manager APP
* > It will ask for username and password give **username=tomcat** and **password=s3cret**


![alt text](<https://github.com/pranavkp306/maven-tomcat/blob/main/Screenshot%202021-09-30%20155913.png>)







