# go-demo-ledger-rest

This sample app is a REST API that reads and writes messages to the Hedera distrubuted ledger.  It is written in Go and utilizes the Hedera Go SDK. The function of this demo app is to log vehicle maintenance/repairs onto a public, immutable ledger, in this case the Hedera network.  App users could use it to prove they did maintenance on their vehicle, which would be usefull for resale evalution purposes.  Orgazizations, like police departments and repair shops could also use it to record incidents like accident damage.

## Setup

This sample app assumes you have already installed the GO distribution.  If not, you can find instructions [here](https://golang.org/doc/install)

Adiitionally, you will need a Hedera Portal profile. To create your Hedera Portal profile register [here](https://portal.hedera.com/register).  Once registered, you'll need to note your Account ID and your Private Key.  These credential will be used by the the app to access any Hedera network services uned in the demo.

Before starting the project, update the .env file with your Hedera Account ID and your Private Key.

## Setup
### Set Hedera Credentials

> .env
>
> ACCOUNT_ID= (set account id)
>
> PRIVATE_KEY= (set private key)
>
> TOPIC_ID= (set later)


This project writes messages to a Hedera pub/sub topic, so you will need to create a topic by executing the following command from the project root directory.

> go run hederaTopicCreation.go

This will create a Hedera pub/sub topic and will return the Topic Id.
Edit the .env again and set the TOPIC_ID

> .env
>
> ACCOUNT_ID=
>
> PRIVATE_KEY=
>
> TOPIC_ID= (set topic id)

Finally, execute the project.

> go run server.go

This will start a local webserver that serves the REST API used to create and read Hedera Topic messages.
THe default URL will be http://localhost:8082/ledgerMessages

Update server.go file to change the port, if desired.

## End Points

GET /ledgerMessages - return all messages for the configured topic

GET /ledgerMessages/<vin> - return messages for the configured topic filtered by VIN

POST /ledgerMessages/ - save message

Expected JSON request format for POST
>
>      {
>
>        "vin": "GA94234351",
>  
>        "workdescription": "Oil Change & Tune Up",
>
>        "servicer": "Smith Auto Repair",
>
>        "technician": "Joe Smith",
>
>        "selectedfile": "receipt.jpg"
>
>      }


The expected "selectedfile" is an uploaded image for the receipt or work summary.  Eventually, this project will be expanded with functionality to upload this file to a distrubuted storage layer, like [IPFS](https://ipfs.io/). The code for the UI used to interact with this REST API is in this [repository](https://github.com/droatl2000/node-demo-ledger-ui)
