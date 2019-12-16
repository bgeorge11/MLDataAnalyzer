# MLDataAnalyzer
A javascript and REST based data analyzer for MarkLogic. 

# Pre-requisites 
1. Java 1.8 or above (for the installer) 
2. A content database to analyze in MarkLogic 9.X or above. Note that the installation procedure does not create database resources.
3. A analyzer configuration document in the content database. Refer instructions below on how to create it. 

# Installation
1. Unzip the project
2. Make a copy of gradle.example.properties and edit for your environment
3. Edit the run/run-config.json for your environment. Only below attributes will need to be changed
    - configUri ==> The URI of the configuration document created in Step 3 of Pre-requisites
    - collections ==> The comma separated list of collections in the database to analyze. The initial values persons,people are for the test data. 
4. Run <i>gradlew mlLoadModules</i> to install the Analyzer. 

# Run the analyzer as Gradle task
Run <i>gradlew invokeAnalyzer -Pmode=collection_level_paths</i> OR <i> gradlew invokeAnalyzer -Pmode=collection_level_tree</i> to run the analyzer 

# Run the analyzer as REST 
The analyzer can be run as a REST service also. After Steps 1-4 of the Installation section, the analyzer can be invoked as a POST service 
http(s)://<host>:<port>/LATEST/resources/InvokeAnalyzer 
with below as the body of POST request
  <pre>
    {
      "config": {
       "outputType" : <"collection_level_paths" OR "collection_level_tree">,
          "collections" : <comma separated collections to be analyzed>,
          "configUri": <The URI of the configuration document created in Step 3 of Pre-requisites> ,
          "analyzerModule": "/analyzer/mlDatabaseAnalyzer.sjs",
          "contentDb": <Name of the database to be analyzed>,
          "modulesDb": <The Modules database where the analyzer is installed>,
          "user": "analyzer"
      }}
  </pre>
# Testing the installation
The analyzer provides some test data to test the installation if required. After the installation steps 1-4 of Installation section
- Run <i>gradlew ingestAllTestData </i>. This ingests 3200 documents in the content database as below 

| Collection | Count | DocumentType
| ------ | ------ | ------ | 
| people | 1100 | xml - 1000 documents having one schema, 100 documents having another schema |
| persons | 2100 | json - 1000 documents having one schema, 1000 documents having second schema and 100 documents having third schema |

- Run <i>gradlew invokeAnalyzer -Pmode=collection_level_paths</i> OR <i> gradlew invokeAnalyzer -Pmode=collection_level_tree</i>. For the test data, the analyzer output should identify total 5 scehmas from 2 collections - persons and people. 

- The analyzer cane be run as REST service to verify the installation.

# How to use the analyzer
Refer the Architecture_And_Usage.pptx. 

# Reading Analyzer Output 
To-be-completed
