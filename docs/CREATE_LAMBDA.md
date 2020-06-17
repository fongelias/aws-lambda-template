# How to create a lambda
Instructions on creating lambdas, from source code to deployment

## AWS
### Role
Create a role based on [this template](./policy_template/BasicExecutionRole.json) to be used for the execution role of all lambdas in this application.

You'll need to specify this role's ARN in Travis as a variable, `AWS_ROLE`

### Lambda
Navigate to the [AWS Management Console](https://console.aws.amazon.com/console/home?region=us-east-1), then:
```
Lambda > Functions > Create Function > Create from scratch
```

For the execution role, select the role created in the previous step

### Policy
Create a policy based on [this template](./policy_template/BasicTravisPipelinePolicy.json) to be used for pushing updates to AWS lambdas from Travis CI

### User
Create a user for travis deployment, and assign the policy you just created to the user.

You may now create Access keys (under the Security Credentials tab), which can be utilized by Travis to authenticate the deployment of code after the CI runs.

You'll need the Access Key ID (`AWS_ACCESS_KEY_ID`) and its corresponding Secret Access Key (`AWS_SECRET_ACCESS_KEY`) to be specified as Travis variables.


## Travis
### .travis.yml
This file is responsible for the configuration of your CI/CD in Travis.

Variable names, like `AWS_SECRET_ACCESS_KEY` are declared in interface located on the Travis website.

For each lambda, you'll need to add a block under deploy for individual deployments, like this one here:
```
  - provider: lambda
    access_key_id: $AWS_ACCESS_KEY_ID
    secret_access_key: $AWS_SECRET_ACCESS_KEY
    function_name: [function name]
    region: "us-east-1"
    role: $AWS_ROLE
    runtime: "nodejs12.x"
    handler_name: "handler"
    zip: [path to lambda]
```
