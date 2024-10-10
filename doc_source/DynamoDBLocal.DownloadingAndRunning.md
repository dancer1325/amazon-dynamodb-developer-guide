# Deploying DynamoDB locally on your computer<a name="DynamoDBLocal.DownloadingAndRunning"></a>

* ways
  * [Download locally](#-download-locally-)
  * [Docker](#-docker-)
  * [Apache Maven](#-apache-maven-)
------
#### [ Download locally ]

* == executable `.jar`
  * runs |
    * Windows,
    * Linux,
    * macOS,
      * | M1 -> use DynamoDB local v1.20+ 
    * other platforms / support Java
* requirements
  * JRE v8+
* steps
  * [download it / find links | AWS documentation website](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DynamoDBLocal.DownloadingAndRunning.html)
    * if you use Eclipse -> use [AWS Toolkit for Eclipse](https://aws.amazon.com/eclipse/)
  * extract the contents
  * run
    * `java -Djava.library.path=./DynamoDBLocal_lib -jar DynamoDBLocal.jar -sharedDb`
    * | Windows PowerShell -- `java -D"java.library.path=./DynamoDBLocal_lib" -jar DynamoDBLocal.jar`
  * 808 as default port
    * if it's unavailable -> previous command throws an exception 
  * `Ctrl\+C`
    * stop DynamoDB
      * meantime, DynamoDB processes incoming requests flow
* `java -Djava.library.path=./DynamoDBLocal_lib -jar DynamoDBLocal.jar -help`
  * complete list of DynamoDB runtime options
* steps -- to access -- DynamoDB programmatically or through AWS Command Line Interface \(AWS CLI\)
  * configure your credentials
    * -- via -- `aws configure`
      * [Using the AWS CLI](Tools.CLI.md)
    * _Example:_
    
       ```
       AWS Access Key ID: "fakeMyKeyId"
       AWS Secret Access Key: "fakeSecretAccessKey"
       ```
  * use the `--endpoint-url` parameter
    * _Example:_

     ```
     aws dynamodb list-tables --endpoint-url http://localhost:8000
     ```

------
#### [ Docker ]

* [dynamodb\-local Docker Hub](https://hub.docker.com/r/amazon/dynamodb-local)
  * _Example:_ [use DynamoDB local as part of a REST application / built | AWS Serverless Application Model \(AWS SAM\)](https://github.com/aws-samples/aws-sam-java-rest) 
* if you want to run a multi\-container / one is the DynamoDB local container -> use Docker Compose
  * `docker-compose.yml`
    * 

     ```
     version: '3.8'
     services:
       dynamodb-local:
         command: "-jar DynamoDBLocal.jar -sharedDb -dbPath ./data"
         image: "amazon/dynamodb-local:latest"
         container_name: dynamodb-local
         ports:
           - "8000:8000"
         volumes:
           - "./docker/dynamodb:/home/dynamodblocal/data"
         working_dir: /home/dynamodblocal
     ```

    * if you want multi-container / your application | container1 + DynamoDB local | container2

     ```
     version: '3.8'
     services:
       dynamodb-local:
         command: "-jar DynamoDBLocal.jar -sharedDb -dbPath ./data"
         image: "amazon/dynamodb-local:latest"
         container_name: dynamodb-local
         ports:
           - "8000:8000"
         volumes:
           - "./docker/dynamodb:/home/dynamodblocal/data"
         working_dir: /home/dynamodblocal
       app-node:
         depends_on:
           - dynamodb-local
         image: amazon/aws-cli
         container_name: app-node
         ports:
           - "8080:8080"
         environment:
            # AWS_ACCESS_KEY_ID & AWS_SECRET_ACCESS_KEY are required, BUT NOT necessarily valid ones | DynamoDB local
           AWS_ACCESS_KEY_ID: 'DUMMYIDEXAMPLE'
           AWS_SECRET_ACCESS_KEY: 'DUMMYEXAMPLEKEY'
         command:
           dynamodb describe-limits --endpoint-url http://dynamodb-local:8000 --region us-west-2
     ```

    * if you want multi-container / your own application image | container1 + DynamoDB local | container2

     ```
     version: '3.8'
     services:
       dynamodb-local:
         command: "-jar DynamoDBLocal.jar -sharedDb -dbPath ./data"
         image: "amazon/dynamodb-local:latest"
         container_name: dynamodb-local
         ports:
           - "8000:8000"
         volumes:
           - "./docker/dynamodb:/home/dynamodblocal/data"
         working_dir: /home/dynamodblocal
       app-node:
         image: location-of-your-dynamodb-demo-app:latest
         container_name: app-node
         ports:
           - "8080:8080"
         depends_on:
           - "dynamodb-local"
         links:
           - "dynamodb-local"
         environment:
            # AWS_ACCESS_KEY_ID & AWS_SECRET_ACCESS_KEY are required, BUT NOT necessarily valid ones | DynamoDB local
           AWS_ACCESS_KEY_ID: 'DUMMYIDEXAMPLE'
           AWS_SECRET_ACCESS_KEY: 'DUMMYEXAMPLEKEY'
           REGION: 'eu-west-1'
     ```
  * `docker-compose up`

------
#### [ Apache Maven ]

* Amazon DynamoDB Local | your application -- as a -- dependency
* goal 
  * 👁️deploy DynamoDB -- via -- Apache Maven dependency 👁️
* steps
  * add | your "pom.xml"

     ```
     <!--Dependency:-->
     <dependencies>
         <dependency>
            <groupId>com.amazonaws</groupId>
            <artifactId>DynamoDBLocal</artifactId>
            <version>[1.12,2.0)</version>
         </dependency>
     </dependencies>
     ```
  * check [AWS documentation website](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DynamoDBLocal.DownloadingAndRunning.html)

------