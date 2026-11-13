# memex

A monorepo containing the constituent components of a software system that provides text entry and retrieval functionality via a web UI. The system is implemented using Java, SpringBoot, JavaScript, Angular, and MongoDB. The system is built and deployed using bash, Gradle, npm, Docker, and Kubernetes. The system was formerly self hosted on AWS.

## Overview

The repository contains the following components:


- memex-service

- memex-ui

- memex-data-service


Each of the components have a README at the root of their containing directories. The software system uses the underinformed design of program integration across its data access and user authentication functionalities, providing both data endpoints and authentication endpoints via the `memex-service` system component. The `memex-data-service` system component is a partial implementation of a cloud backup utility and is not essential to the functioning of the system. The repository also contains a library named `memex-spring-data`, which is used to bind data types across the `memex-service` and `memex-data-service` components.

The system was used to record personal journal entries and is no longer in use.

## Running Locally

To use the system locally:
 - Install system level dependencies
   - Java 17+ with JAVA_HOME set
   - Docker daemon (any version)
   - npm version 10 (packaged with Node version 22)
   - Mongo DB client, version 4+
 - Start the `memex-service`
   - Open a CLI and change directory to memex-service/docker, then run the [memex-service/docker/docker-compose-up.sh](memex-service/docker/docker-compose-up.sh) script.
   - Provide responses to the input prompts as follows:
     - Username and password: any value, but will be needed later
     - Encryption secrets: any value, including 0 character strings
     - Spring active profile: 'dev', without quotes
 - Start the `memex-ui`
   - Open a CLI and change directory to memex-ui/docker, then run the [memex-ui/docker/docker-compose-up.sh](memex-ui/docker/docker-compose-up.sh) script.
 - Access the app via web browser at http://localhost:4200
   - Click any button to prompt a login
   - Login using the user name and password that were entered when starting the memex-service

If the application starts but authentication fails with a 401 response, a possible cause of failure is the incorrect population of the Mongo userDetails collection. To resolve the issue:
1) Choose a new username and password.
2) Use the getEncryptedPassword endpoint to encrypt the password.
3) Enter a new entry in to the memex.userDetails Mongo collection where the entry object has a key "password" with a value that is the encrypted password and a key "_id" with a value that is the username of choice.
4) Reattempt login using the chosen username and the password.

## Using the app

To use the app:
   - Select the plus sign to create a new entry.
   - Type in the dark grey text box to populate the entry. Please allow 5 to 10 seconds for the entry to automatically persist (see the 'timeInterval' variable in the EntryScreenComponent class of the memex-ui program). Select the plus sign to create a new entry or close the browser after the persistence interval has passed.
   - Search for previous entries from the 'Retrieval' tab. An empty search string returns all entries. Non-empty search strings are matched exactly with up to 1 character typo (see the 'fuzziness' variable in the RawTextESRestTemplate class of the memex-spring-data library). Selecting a search result from the 'Retrieval' tab will navigate to the 'Entry' tab.
   - Delete entries by selecting the entry from the 'Retrieval' tab and then deleting the content of the 'Entry' tab text box. Proper delete functionality was not implemented.

