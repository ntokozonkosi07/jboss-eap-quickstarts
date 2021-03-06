# tasks: Test JPA with Arquillian

Author: Oliver Kiss, Lukas Fryc  
Level: Intermediate  
Technologies: JPA, Arquillian  
Summary: The `tasks` quickstart includes a persistence unit and sample persistence code to demonstrate how to use JPA for database access in JBoss EAP.  
Target Product: JBoss EAP  
Source: <https://github.com/jbossas/eap-quickstarts/>  


## What is it?

The `tasks` quickstart demonstrates how to use JPA in Red Hat JBoss Enterprise Application Platform.

It includes a persistence unit and some sample persistence code to demonstrate database access in enterprise Java.

It does not contain a user interface layer. The purpose of the project is to show you how to test JPA with Arquillian.

_Note: This quickstart uses the H2 database included with Red Hat JBoss Enterprise Application Platform 7.1. It is a lightweight, relational example datasource that is used for examples only. It is not robust or scalable, is not supported, and should NOT be used in a production environment!_

_Note: This quickstart uses a `*-ds.xml` datasource configuration file for convenience and ease of database configuration. These files are deprecated in JBoss EAP and should not be used in a production environment. Instead, you should configure the datasource using the Management CLI or Management Console. Datasource configuration is documented in the [Configuration Guide](https://access.redhat.com/documentation/en/red-hat-jboss-enterprise-application-platform/) for Red Hat JBoss Enterprise Application Platform._


## System Requirements

The application this project produces is designed to be run on Red Hat JBoss Enterprise Application Platform 7.1 or later.

All you need to build this project is Java 8.0 (Java SDK 1.8) or later and Maven 3.3.1 or later. See [Configure Maven for JBoss EAP 7.1](https://github.com/jboss-developer/jboss-developer-shared-resources/blob/master/guides/CONFIGURE_MAVEN_JBOSS_EAP7.md#configure-maven-to-build-and-deploy-the-quickstarts) to make sure you are configured correctly for testing the quickstarts.


## Use of EAP7_HOME

In the following instructions, replace `EAP7_HOME` with the actual path to your JBoss EAP installation. The installation path is described in detail here: [Use of EAP7_HOME and JBOSS_HOME Variables](https://github.com/jboss-developer/jboss-developer-shared-resources/blob/master/guides/USE_OF_EAP7_HOME.md#use-of-eap_home-and-jboss_home-variables).


## Start the Server

1. Open a command prompt and navigate to the root of the JBoss EAP directory.
2. The following shows the command line to start the server:

        For Linux:   EAP7_HOME/bin/standalone.sh
        For Windows: EAP7_HOME\bin\standalone.bat


## Run the Arquillian Tests

This quickstart provides Arquillian tests. By default, these tests are configured to be skipped as Arquillian tests require the use of a container.

1. Make sure you have started the JBoss EAP server as described above.
2. Open a command prompt and navigate to the root directory of this quickstart.
3. Type the following command to run the test goal with the following profile activated:

        mvn clean verify -Parq-remote

You can also let Arquillian manage the JBoss EAP server by using the `arq-managed` profile. For more information about how to run the Arquillian tests, see [Run the Arquillian Tests](https://github.com/jboss-developer/jboss-developer-shared-resources/blob/master/guides/RUN_ARQUILLIAN_TESTS.md#run-the-arquillian-tests).


## Investigate the Console Output

Maven prints summary of the 8 performed tests to the console.

    -------------------------------------------------------
     T E S T S
    -------------------------------------------------------
    Running org.jboss.as.quickstarts.tasks.UserDaoTest
    Tests run: 3, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 1.084 sec
    Running org.jboss.as.quickstarts.tasks.TaskDaoTest
    Tests run: 5, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 2.31 sec

    Results :

    Tests run: 8, Failures: 0, Errors: 0, Skipped: 0


### Server log

SQL statements generated by Hibernate are written into the server log.

#### Server Log: Expected warnings and errors

_Note:_ You will see the following warnings in the server log. You can ignore these warnings.

    WFLYJCA0091: -ds.xml file deployments are deprecated. Support may be removed in a future version.

    HHH000431: Unable to determine H2 database version, certain features may not work


#### Example Output from the tests

Creating the database schema:

    INFO  [stdout] (ServerService Thread Pool -- 104) Hibernate: drop table Task if exists
    INFO  [stdout] (ServerService Thread Pool -- 104) Hibernate: drop table User if exists
    INFO  [stdout] (ServerService Thread Pool -- 104) Hibernate: create table Task (id bigint generated by default as identity, title varchar(255), owner_id bigint, primary key (id))
    INFO  [stdout] (ServerService Thread Pool -- 104) Hibernate: create table User (id bigint generated by default as identity, username varchar(255), primary key (id))
    INFO  [stdout] (ServerService Thread Pool -- 104) Hibernate: alter table Task add constraint FK46hwvn8nayomwr7k7h13l0uxe foreign key (owner_id) references User
    INFO  [stdout] (ServerService Thread Pool -- 104) Hibernate: alter table User add constraint UK_jreodf78a7pl5qidfh43axdfb unique (username)

Generating ID for a new entity and inserting the entity into the database:

    INFO  [stdout] (default task-83) Hibernate: select user0_.id as id1_1_, user0_.username as username2_1_ from User user0_ where user0_.username=?
    INFO  [stdout] (default task-88) Hibernate: select user0_.id as id1_1_, user0_.username as username2_1_ from User user0_ where user0_.username=?
    INFO  [stdout] (default task-90) Hibernate: insert into User (id, username) values (null, ?)
    INFO  [stdout] (default task-90) Hibernate: select user0_.id as id1_1_, user0_.username as username2_1_ from User user0_ where user0_.username=?


## Run the Quickstart in Red Hat JBoss Developer Studio or Eclipse

You can also start the server and deploy the quickstarts or run the Arquillian tests from Eclipse using JBoss tools. For general information about how to import a quickstart, add a JBoss EAP server, and build and deploy a quickstart, see [Use JBoss Developer Studio or Eclipse to Run the Quickstarts](https://github.com/jboss-developer/jboss-developer-shared-resources/blob/master/guides/USE_JBDS.md#use-jboss-developer-studio-or-eclipse-to-run-the-quickstarts).


## Debug the Application

If you want to debug the source code of any library in the project, run the following command to pull the source into your local repository. The IDE should then detect it.

        mvn dependency:sources
