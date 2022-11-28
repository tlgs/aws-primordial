# aws-primordial

Collection of useful CloudFormation templates to bootstrap an AWS account.

## Data lake S3 buckets

Basic set of S3 buckets as recommended in
[Defining S3 bucket and path names for data lake layers on the AWS Cloud](https://docs.aws.amazon.com/prescriptive-guidance/latest/defining-bucket-names-data-lakes/welcome.html).
**This has been setup with a personal account in mind:**

- Only `dev` and `prod` environments; No `test` for now.
- Versioning enabled for the raw data layer in the `prod` environment bucket -
  I want to be able to _seamlessly delete_ files during development.
- No lifecycle policies attached to any layers / environments -
  S3 standard is more than enough for me for now (for both `dev` and `prod` workloads).
  See [AWS::S3::Bucket LifecycleConfiguration](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-s3-bucket-lifecycleconfig.html)
  and [Object Lifecycle Management](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lifecycle-mgmt.html).

Create stack (while setting `BUCKET_ENVIRONMENT` appropriately):

```
aws cloudformation create-stack \
  --stack-name datalake-buckets-"$BUCKET_ENVIRONMENT" \
  --template-body file://datalake-buckets.yaml \
  --parameters ParameterKey=EnvType,ParameterValue="$BUCKET_ENVIRONMENT"
```

## Application S3 buckets

WIP

Create stack (while setting `BUCKET_ENVIRONMENT` appropriately):

```
aws cloudformation create-stack \
  --stack-name application-buckets-"$BUCKET_ENVIRONMENT" \
  --template-body file://application-buckets.yaml \
  --parameters ParameterKey=EnvType,ParameterValue="$BUCKET_ENVIRONMENT"
```

## GitHub OIDC Provider

OIDC Provider that applications using GitHub Actions
can interact with to request short-lived credentials.

[aws-actions/configure-aws-credentials](https://github.com/aws-actions/configure-aws-credentials)
is the _de facto_ reference on setting up GitHub Actions to interface
with AWS resources.
See also:
- [Configuring OpenID Connect in Amazon Web Services](https://docs.github.com/en/actions/deployment/security-hardening-your-deployments/configuring-openid-connect-in-amazon-web-services)
  (GitHub docs)
- [Creating OpenID Connect (OIDC) identity providers](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_oidc.html)
  (AWS docs)
- [Configuring a role for GitHub OIDC identity provider](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-idp_oidc.html#idp_oidc_Create_GitHub)
  (AWS docs)

Create stack:

```
aws cloudformation create-stack \
  --stack-name github-oidc-provider \
  --template-body file://github-oidc.yaml
```
