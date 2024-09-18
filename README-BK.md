# Bulent's Notes

# Landing Zone setup using Control Tower

https://docs.aws.amazon.com/solutions/latest/landing-zone-accelerator-on-aws/prerequisites.html

Create a default landing zone with Control Tower.
* Instead of Sandbox, add an **Infrastructure** OU.
* Ensure the global Region for eu-west-2 (which is **us-east-1**) is accessible.

Landing zone setup failed due to lack of permissions to use the customer managed key by AWS Config and CloudTrail.
* Configured the customer managed key to add AWS Config and CloudTrail policies:
https://docs.aws.amazon.com/en_us/controltower/latest/userguide//configure-kms-keys.html#kms-key-policy-update
* Restarted the landing zone setup.


# Deploy the stack using CloudFormation

https://docs.aws.amazon.com/solutions/latest/landing-zone-accelerator-on-aws/deploy-the-solution.html

Disable anonymized data collection by making a small change in a local copy of the template:
https://docs.aws.amazon.com/solutions/latest/landing-zone-accelerator-on-aws/collection-of-operational-metrics.html


# Building the Accelerator code (No need)

(Not needed to deploy the Accelerator to AWS. Use CloudFormation instead.)

https://awslabs.github.io/landing-zone-accelerator-on-aws/latest/installation/

Login to AWS (SSO); set env variables for the access key & secret and session token.

```
cdk bootstrap aws://<ACCOUNT-ID>/eu-west-2
```

Install Node's latest LTS version:
```
nvm install --lts
```

```
npm install -g yarn
npm install -g aws-cdk
nvm install -g husky
```

```
cd source
yarn install
cd  packages/@aws-accelerator/installer
yarn build
yarn cdk synth
```

```
export BUCKET_NAME=<BUCKET-NAME>
aws s3 mb s3://$BUCKET_NAME # or create using AWS Console
export ACCOUNT_ID=$(aws sts get-caller-identity --query Account --output text)
aws s3api head-bucket --bucket $BUCKET_NAME --expected-bucket-owner $ACCOUNT_ID
aws s3 cp ./cdk.out/AWSAccelerator-InstallerStack.template.json s3://$BUCKET_NAME
```

Deploy the stack using CloudFormation.