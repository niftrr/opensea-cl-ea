# Opensea Chainlink External Adapter in NodeJS.

To retrieve Opensea floor price per given NFT collection.

## Input Params

- `q`: the Opensea API `collection_slug` parameter to query

## Output

```json
{
  "jobRunID": 0, 
  "data": {
    "stats": {
      "one_day_volume": 2059.34782734916,
      "one_day_change": 0.35853963254478943,
      "one_day_sales": 502,
      "one_day_average_price": 4.102286508663665,
      "seven_day_volume": 5352.95893016035, 
      "seven_day_change": 4.096843834444037,
      "seven_day_sales": 1929, 
      "seven_day_average_price": 2.774991669341809,
      "thirty_day_volume": 13467.0743491787,
      "thirty_day_change": 0.7387267158496497,
      "thirty_day_sales": 6998,
      "thirty_day_average_price": 1.9244175977677478,
      "total_volume": 21212.4400966635, 
      "total_sales": 12390,
      "total_supply": 9999,
      "count": 9999,
      "num_owners": 5197,
      "average_price": 1.7120613475918887,
      "num_reports": 2,
      "market_cap": 27747.14170174875,
      "floor_price": 3.63
    },
    "result": 3.63
  },
  "result": 3.63,
  "statusCode": 200
}
```

## Install Locally

Install dependencies:

```bash
yarn
```

### Test

Run the local tests:

```bash
yarn test
```

Natively run the application (defaults to port 8080):

### Run

```bash
yarn start
```

## Call the external adapter/API server

```bash
curl -X POST -H "content-type:application/json" "http://localhost:8080/" --data '{ "id": 0, "data": {"q":"doodles-official"} }'
```

## Docker

If you wish to use Docker to run the adapter, you can build the image by running the following command:

```bash
docker build . -t external-adapter
```

Then run it with:

```bash
docker run -p 8080:8080 -it external-adapter:latest
```

## Serverless hosts

After [installing locally](#install-locally):

### Create the zip

```bash
zip -r external-adapter.zip .
```

### Install to AWS Lambda

- In Lambda Functions, create function
- On the Create function page:
  - Give the function a name
  - Use Node.js 12.x for the runtime
  - Choose an existing role or create a new one
  - Click Create Function
- Under Function code, select "Upload a .zip file" from the Code entry type drop-down
- Click Upload and select the `external-adapter.zip` file
- Handler:
    - index.handler for REST API Gateways
    - index.handlerv2 for HTTP API Gateways
- Add the environment variable (repeat for all environment variables):
  - Key: API_KEY
  - Value: Your_API_key
- Save

#### To Set Up an API Gateway (HTTP API)

If using a HTTP API Gateway, Lambda's built-in Test will fail, but you will be able to externally call the function successfully.

- Click Add Trigger
- Select API Gateway in Trigger configuration
- Under API, click Create an API
- Choose HTTP API
- Select the security for the API
- Click Add

#### To Set Up an API Gateway (REST API)

If using a REST API Gateway, you will need to disable the Lambda proxy integration for Lambda-based adapter to function.

- Click Add Trigger
- Select API Gateway in Trigger configuration
- Under API, click Create an API
- Choose REST API
- Select the security for the API
- Click Add
- Click the API Gateway trigger
- Click the name of the trigger (this is a link, a new window opens)
- Click Integration Request
- Uncheck Use Lamba Proxy integration
- Click OK on the two dialogs
- Return to your function
- Remove the API Gateway and Save
- Click Add Trigger and use the same API Gateway
- Select the deployment stage and security
- Click Add

### Install to GCP

- In Functions, create a new function, choose to ZIP upload
- Click Browse and select the `external-adapter.zip` file
- Select a Storage Bucket to keep the zip in
- Function to execute: gcpservice
- Click More, Add variable (repeat for all environment variables)
  - NAME: API_KEY
  - VALUE: Your_API_key
