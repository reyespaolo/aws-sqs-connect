# AWS SQS Connect

Connects to AWS SQS (Amazon Simple Queue Service) and gets messages

## Getting Started on AWS SQS Connects

```
const ConsumeData = require('aws_sqs_service');
const Config = require('./config')
const AWS = require('aws-sdk');

AWS.config.update({
  region: Config.region,
  accessKeyId: Config.accessKeyId,
  secretAccessKey: Config.secretAccessKey
});

const app = ConsumeData.initialize({
  queueUrl: Config.queueUrl,
  batchSize: 10,
  visibilityTimeout: 50,
  attributeNames: ['All'],
  messageAttributeNames: ['All'],
  sqs: new AWS.SQS(),
  handleMessage: (message, done) => {
    deleteMessage(message)
    done();

  },
});

app.on('error', (err) => {
  console.log(err.message);
});

app.on('message_received', (message) => {
  console.log(message)
});

function deleteMessage(message){
  app.deleteSQSMessage(message, function(message){
    console.log(message)
  })
}

app.start();

```
