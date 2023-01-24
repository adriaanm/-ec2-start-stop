## 📖

A Jenkins plugin used on https://scala-ci.typesafe.com to start and stop EC2 instances on demand.

Differs from the [ec2-plugin](https://plugins.jenkins.io/ec2/), which creates and deletes instances based on AMIs.

## 🚒

After updating all (other) Jenkins plugins (Jan 23, 2023), this plugin stopped working, the Jenkins node log showed "Socket not created by this factory".

A google search brought me to [this StackOverflow](https://stackoverflow.com/questions/48908610/aws-socket-not-created-by-this-factory), suggesting to try updating dependencies. After attempting to update individual dependencies first, I finally updated everything, which luckily worked.

## 🚧

To build the plugin
  - Switch to Java 11
  - [Allow external HTTP repos](https://stackoverflow.com/questions/67001968/how-to-disable-maven-blocking-external-http-repositories) in maven by creating a file `~/.m2/settings.xml` with the following content:
      ```
      <settings xmlns="http://maven.apache.org/SETTINGS/1.2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.2.0 http://maven.apache.org/xsd/settings-1.2.0.xsd">
          <mirrors>
                <mirror>
                    <id>maven-default-http-blocker</id>
                    <mirrorOf>dummy</mirrorOf>
                    <name>Dummy mirror to override default blocking mirror that blocks http</name>
                    <url>http://0.0.0.0/</url>
              </mirror>
          </mirrors>
      </settings>
      ```
  - Build the plugin by running `mvn install -Dspotbugs.skip=true`
  - Upload the `target/ec2-start-stop.hpi` file to a [GitHub release](https://github.com/lightbend/ec2-start-stop/releases)
  - Copy the `hpi` file URL and use it in https://scala-ci.typesafe.com/manage/pluginManager/advanced to install ("deploy") the new version, restart Jenkins

## 🤨

  - Not sure where credentials are picked up.
  - Didn't figure out how to enable verbose logging for the AWS client library - that would be useful when things don't work.
