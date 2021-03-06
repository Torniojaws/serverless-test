# Testing serverless

Testing out how it works, following the guide in
https://hackernoon.com/a-crash-course-on-serverless-with-node-js-632b37d58b44#422a
It appears that uncommenting `events` is not needed anymore. It is automatically correct.

## Steps without an AWS account

You can run the entire thing locally with these steps. You don't need an AWS account at all, unless
you want to deploy.

1. `npm install -g serverless`
1. Create a service: `serverless create --template aws-nodejs --path my-service`
1. So that you don't need to deploy it to AWS (or use it without an account), you can use `serverless-offline`
1. Open the subdir `my-service` and run `npm init` there
1. Then install: `npm install serverless-offline --save-dev`
1. Add these two lines to the end of the `serverless.yml` file. They are in the root level - no spaces:
  ```
  plugins:
  - serverless-offline
  ```
1. Then you can run it offline: `serverless offline start`, which has a local API Gateway and Lambda
1. Call `curl http://localhost:3000/1/functions/my-service-dev-hello/invocations -XPOST` and it should show up

## Steps with an AWS account

1. `npm install -g serverless`
1. Create an IAM role in AWS Console (`Users -> Add user`) with name `my-name`
1. Give it `programmatic access`
1. Then `Attach existing policies` and check `Administrator access`
1. Save the AWS `Access Key ID` and `Access Secret Key`, and keep them for example in your env
1. Then add the Key and Secret to the serverless config from your env variables:
  `serverless config credentials --provider aws --key AWS_ACCESS_KEY --secret AWS_SECRET_KEY`
1. Create a service: `serverless create --template aws-nodejs --path my-service`
1. Deploy the function to AWS: `serverless deploy -v`
1. Call it from the command line: `serverless invoke -f hello -l`
1. So that you don't need to deploy it to AWS everytime you want to try it, you can use `serverless-offline`
1. Open the subdir `my-service` and run `npm init` there
1. Then install: `npm install serverless-offline --save-dev`
1. Add these two lines to the end of the `serverless.yml` file. They are in the root level - no spaces:
  ```
  plugins:
  - serverless-offline
  ```
1. Run the command: `serverless`, and there should be an option for `offline`
1. Then you can run it offline: `serverless offline start`, which has a local API Gateway and Lambda
1. Call `curl http://localhost:3000/1/functions/my-service-dev-hello/invocations -XPOST` and it should show up
1. For better logging than CloudWatch, try: https://www.dashbird.io/
  - Tutorial: https://dashbird.io/help/basic/get-started/


## Notes

- API Gateway = event trigger, Lambda = callback
  - ie. you need API Gateway (which creates the endpoint) before you can call (invoke) the Lambda function
  - Then you will call it via an url like: https://abcdef123.execute-api.eu-central-1.amazonaws.com/dev/hello/get
  - It returns the output of the function to the browser as such
