## Just a short little demo running Step Functions locally

- link to AWS documentation on running step functions locally https://docs.aws.amazon.com/step-functions/latest/dg/sfn-local.html


### Prerequisites

- you need AWS CLI installed
- you need AWS SAM CLI installed

### Modify your parameters

Look at the following files and edit them according to your setup:

- localsettings.txt
- StateMachine.json
- template.yaml

### Commands used:

- Start the local Lambda endpoint // you can skip the host and port if you want default (localhost) or you can edit IP and port to your needs
```bash
sam local start-lambda --host 127.0.0.1 --port 3001
```

- Start docker container for local Step Functions, on first execution it should automatically pull the needed docker image // make sure you edit localsettings.txt to suit your needs (IP)
```bash
docker run -p 8083:8083 --env-file localsettings.txt amazon/aws-stepfunctions-local
``` 

- Create state machine
```bash
aws stepfunctions create-state-machine --endpoint http://localhost:8083 --definition file://StateMachine.json --name "HelloFromLocalStepFunctions" --role-arn "arn:aws:iam::012345678901:role/DummyRole"
```

- invoke Step Function execution
```bash
aws stepfunctions --endpoint http://localhost:8083 start-execution --state-machine arn:aws:states:us-east-1:123456789012:stateMachine:HelloFromLocalStepFunctions --name test
```

- Execute the describe execution command to see the full details of the execution
```bash
aws stepfunctions --endpoint http://localhost:8083 describe-execution --execution-arn arn:aws:states:us-east-1:123456789012:execution:HelloFromLocalStepFunctions:test
``` 
