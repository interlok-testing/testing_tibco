# Tibco Testing

[![license](https://img.shields.io/github/license/interlok-testing/testing_tibco.svg)](https://github.com/interlok-testing/testing_tibco/blob/develop/LICENSE)
[![Actions Status](https://github.com/interlok-testing/testing_tibco/actions/workflows/gradle-build.yml/badge.svg)](https://github.com/interlok-testing/testing_tibco/actions/workflows/gradle-build.yml)

Project tests interlok-tibco features

## What it does

This project contains a single instance of Interlok that will attempt to connect to a locally running instance of TibcoEMS upon start-up.  Once started a single polling trigger will produce a message every 10 seconds and send to a queue on the broker.  
A second workflow will consume the message and move to a second queue.
A third workflow will again consume the message and send to a topic, where a final workflow is subscribed to that topic.

![tibco diagram](/tibco.png "tibco diagram")
 

## Getting started

Please note this project uses a licensed component; therefore you will need an Interlok license.
Once obtained create a file "src/main/interlok/config/license.properties" that contains your license key.

### Configuring Tibco EMS

We'll need to create the topic endpoint to produce and consume from.  To do this we'll use the Tibco admin CLI tool which can be found in your Tibco EMS installations "bin" directory.  We'll use two scripts which can be found in the root of this project;
 - tibco-script.txt
 - tibco-show-script.txt

Now simply use your command line tool and execute each script (replacing the path to the script) like so;
```
> tibemsadmin.exe -server tcp://localhost:7222 -script C:\tibco-testing\tibco-script.txt
```
The output of the script may look something like this;
```
TIBCO Enterprise Message Service Administration Tool.
Copyright 2003-2019 by TIBCO Software Inc.
All rights reserved.

Version 8.5.1 V4 9/12/2019

Connected to: tcp://localhost:7222
Command: create topic Sample.T1
Topic 'Sample.T1' has been created
Warning: command script did not commit changes before exiting
Trying to commit now...
Configuration has been saved
```

The second script is optional and only to verify the required JMS endpoint has been created;

```
> tibemsadmin.exe -server tcp://localhost:7222 -script C:\xa-testing-activemq-tibco\tibco-show-script.txt

TIBCO Enterprise Message Service Administration Tool.
Copyright 2003-2019 by TIBCO Software Inc.
All rights reserved.

Version 8.5.1 V4 9/12/2019

Connected to: tcp://localhost:7222
Command: show topics
                                                               All Msgs            Persistent Msgs
  Topic Name                        SNFGEIBCTM  Subs  Durs     Msgs    Size        Msgs    Size
  Sample.T1                         ----------     0     0        0     0.0 Kb        0     0.0 Kb

```

### Launching Interlok

* `./gradlew clean build`
* `(cd ./build/distribution && java -jar lib/interlok-boot.jar)`
