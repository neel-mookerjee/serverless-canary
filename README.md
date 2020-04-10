# Canary on AWS using Serverless

### Purpose

Run canaries for HTTP endpoints. This canary uses Labmda and is maintained using serverless framework.

## Adoption

1. Add your function under _functions_ in `serverless.yml`

E.g.
```yaml
functions:
  example1:
    handler:           handler.checkTarget
    events:
      # Invoke Lambda function every 2 minutes
      - schedule:
          rate:        rate(2 minutes)
          enabled:     true
          input:
            name:      example1
            url:       https://example.org
            type:      http
            namespace: "Canary"
            timeout:   10000
    tags:              # Function specific tags
      org:             arghanil
```

## Deploy

Serverless Framework helps develop and deploy your AWS Lambda functions easily.

Learn more about serverless: https://serverless.com/framework/docs/providers/aws/guide/quick-start/

```bash
## set aws profile
npm install --production
sls deploy --stage production
```

* The deployment creates a Lambda application for each function.
* The Lambda application has the following:
  * CouldWatch Event Rule Schedule
  * Lambda function
  * CloudWatch log group
* The serverless deployment uses an S3 bucket to store all artifacts (e.g. CloudFormation template)

## Telemetry

### Metrics sent to CloudWatch

| Name | Description |
|------|-------|
| `aws.lambda.invocations` | CloudWatch metrics sent by the lambda service on invocation |
| `canary.status` | post actual Http Response code |
| `canary.timing_total` | post total execution time of the call as reported by _curl_ |
| `aws.lambda.errors` | CloudWatch metrics sent by the lambda service on error |


### Dashboards

| Type | URL |
| --- | --- |
| logs | Logs for the canary will be available under CouldWatch -> Log Groups -> /aws/lambda/canary-production-<your function> |

## Additional Notes
