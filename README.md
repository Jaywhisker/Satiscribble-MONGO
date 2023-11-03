# Satiscribble-DB
This is the repository for Satiscribble DB. This mongoDB has 2 collections:
`minutes`: document store of all agenda, meeting details and minutes (segmented by topics)
`chatHistory`: document store of all chat history, split by document and web QnA

# Setup
This github is hosted on Docker. To set up your docker image, please follow the following steps after you git clone.
```
cd build
docker compose up
```
Note: There is no volume mount for /build and /test folders as there shouldn't be any further edits required. If you do edit the files, please down the container before rebuilding it.

Once you have the container set up, we need to give our admin user root. <br/>
Enter the docker shell / terminal and run the following:
```
mongosh (your terminal should change from # to test> )
use admin (your terminal should change from test> to admin> )
db.createUser(
   {
     user: 'admin',
     pwd: 'password',
     roles: [ { role: 'root', db: 'admin' } ]
  }
)
```
Please copy the entire db.createUser function and send it all at once. You should see a `{ok:1}` as a response.

Once you have given admin root access, exit mongosh db by executing `exit`. Your terminal should return to a #. <br/>
Upon success, run the final command to setup your mongoDB schema:
```
mongosh --username admin --authenticationDatabase admin --password password < satiscribble-db/build/create_document_table.mql

```

# Test
To confirm that your MongoDB has set up properly, you may run
```
mongosh --username admin --authenticationDatabase admin --password password < satiscribble-db/test/test.mql
```

# Querying information 
After launching the docker program, login in with the user
```
mongosh --username admin --authenticationDatabase admin --password password
```

Then go to the correct document
```
use document_db
```

Run away
To get the minutes database
```
db.minutes.find({})
```

To get the chatHistory database
```
db.chatHistory.find({})
```


# File structure
```
├── build                          <- folder containing all docker container related files
│   ├── create_document_table.mql  <- schema for mongoDB
│   ├── Dockerfile
│   └── docker-compose.yml (volume mount important files)
│
└── tests                          <- folder containing all test files required for each microservice
    └── test.mql                   <- mql file containing test code to test that mongoDB schema is up and running
```
