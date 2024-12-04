---
type: docs
title: "Authenticating to AWS"
linkTitle: "Authenticating to AWS"
weight: 10
description: "Information about authentication and configuration options for AWS"
aliases:
  - /developing-applications/integrations/authenticating/authenticating-aws/
---

All Dapr components using various AWS services (DynamoDB, SQS, S3, etc) use a standardized set of attributes for configuration via the AWS SDK. [Learn more about how the AWS SDK handles credentials](https://docs.aws.amazon.com/sdk-for-go/v1/developer-guide/configuring-sdk.html#specifying-credentials).

You can configure the AWS SDK using the default provider chain, or using one of the following built-in AWS authentication profiles. Test the component configuration and inspect the log output from the Dapr runtime to ensure that components initialize correctly.

### AWS: Access Key ID and Secret Access Key
Authenticate to AWS using static Access Key and Secret Key information. You can do this through metadata fields on the component, or by using the [default AWS configuration](https://docs.aws.amazon.com/sdkref/latest/guide/creds-config-files.html) for this authentication profile. 

{{% alert title="Important" color="warning" %}}
You should leverage the loading of the default AWS configuration instead of providing AWS access key, secret key, and tokens in the definition of the component spec you're using:
- When running the Dapr sidecar (`daprd`) with your application on EKS (AWS Kubernetes)
- If using a node/pod that has already been attached to an IAM policy defining access to AWS resources 
{{% /alert %}}

| Attribute | Required | Description | Example |
| --------- | ----------- | ----------- | ----------- |
| `region` | Y | Which AWS region to connect to. | "us-east-1" |
| `accessKey` | N | AWS Access key id. | "AKIAIOSFODNN7EXAMPLE" |
| `secretKey` | N | AWS Secret access key. Use together with `accessKey` to explicitly specify credentials. | "wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY" |
| `sessionToken` | N | AWS Session token. Used together with `accessKey` and `secretKey`. When using a regular IAM user's access key and secret, a session token is normally not required. | |

### Assume IAM Role
Assume a specific IAM Role. This authentication profile assumes your running the Dapr sidecar on EKS (AWS Kubernetes), or using a node/pod that has already been attached to an IAM policy defining access to AWS resources.

This authentication profile is only supported on Kafka and PostgreSQL components at this time.

| Attribute | Required | Description | Example |
| --------- | ----------- | ----------- | ----------- |
| `region` | Y | Which AWS region to connect to. | "us-east-1" |
| `assumeRoleArn` | N | IAM role that has access to AWS resource. This field will be marked required in Dapr 1.17. | "arn:aws:iam::123456789:role/mskRole" |
| `sessionName` | N | The session name for assuming a role. Default is `"DaprDefaultSession"`. | "MyAppSession" |

### Credentials from Environment Variables
Authentication can be done through setting [environment variables](https://docs.aws.amazon.com/sdkref/latest/guide/environment-variables.html) providing credentials. This supports situations, such as when running Dapr in self-hosted mode where the Dapr sidecar injector doesn't allow for configuring environment variables on the Dapr sidecar. 

There are no metadata fields required for this authentication profile.

### IAM Roles Anywhere
[IAM Roles Anywhere](https://aws.amazon.com/iam/roles-anywhere/) is an AWS service that extends IAM role-based authentication to workloads running outside of AWS. It eliminates the need for long-term credentials by using cryptographically signed certificates, anchored in a trust relationship. This leverages the existing PKI, Dapr Sentry control plane service, to sign X.509 certificates and assign a unique [SPIFFE](https://spiffe.io/) identity to each application. To configure this authentication profile:
1. Create a Trust Anchor in the trusting AWS account by uploading the Dapr certificate bundle as an `External certificate bundle`.
2. Create an IAM role with the resource permissions policy necessary, as well as a trust entity for the Roles Anywhere AWS service. Here, you can specify SPIFFE identities allowed.
3. Create an IAM Profile under the Roles Anywhere service, linking the IAM Role.

This is the most secure AWS authentication profile as Dapr handles credential rotation at half the session lifespan.


| Attribute | Required | Description | Example |
| --------- | ----------- | ----------- | ----------- |
| `trustAnchorArn` | Y | ARN of the Trust Anchor in the AWS account granting trust to the Dapr Certificate Authority. | arn:aws:rolesanywhere:us-west-1:012345678910:trust-anchor/01234568-0123-0123-0123-012345678901 |
| `trustProfileArn` | Y | ARN of the AWS IAM Profile in the trusting AWS account. | arn:aws:rolesanywhere:us-west-1:012345678910:profile/01234568-0123-0123-0123-012345678901 |
| `assumeRoleArn` | Y | ARN of the AWS IAM role to assume in the trusting AWS account. | arn:aws:iam:012345678910:role/exampleIAMRoleName |

### Additional Optional Fields

Some AWS components include additional optional fields:

| Attribute | Description |
| --------- | ----------- |
| `endpoint` | N | The endpoint is normally handled internally by the AWS SDK. However, in some situations it might make sense to set it locally - for example if developing against [DynamoDB Local](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DynamoDBLocal.html). |

Furthermore, non-native AWS components such as Kafka and PostgreSQL that support AWS authentication profiles have metadata fields to trigger the AWS authentication logic. Be sure to check your specific component documentation.

## Alternatives to explicitly specifying credentials in component manifest files

In production scenarios, it is recommended to use a solution such as:
- [Kiam](https://github.com/uswitch/kiam) 
- [Kube2iam](https://github.com/jtblin/kube2iam) 

If running on AWS EKS, you can [link an IAM role to a Kubernetes service account](https://docs.aws.amazon.com/eks/latest/userguide/create-service-account-iam-policy-and-role.html), which your pod can use.

All of these solutions solve the same problem: They allow the Dapr runtime process (or sidecar) to retrive credentials dynamically, so that explicit credentials aren't needed. This provides several benefits, such as automated key rotation, and avoiding having to manage secrets.

Both Kiam and Kube2IAM work by intercepting calls to the [instance metadata service](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/configuring-instance-metadata-service.html).

### Use an instance profile when running in stand-alone mode on AWS EC2

If running Dapr directly on an AWS EC2 instance in stand-alone mode, you can use instance profiles. 

1. Configure an IAM role.
1. [Attach it to the instance profile](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2_instance-profiles.html) for the ec2 instance.

Dapr then authenticates to AWS without specifying credentials in the Dapr component manifest.

### Authenticate to AWS when running dapr locally in stand-alone mode

{{< tabs "Linux/MacOS" "Windows" >}}
 <!-- linux -->
{{% codetab %}}

When running Dapr (or the Dapr runtime directly) in stand-alone mode, you can inject environment variables into the process, like the following example: 

```bash
FOO=bar daprd --app-id myapp
```

If you have [configured named AWS profiles](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html) locally, you can tell Dapr (or the Dapr runtime) which profile to use by specifying the "AWS_PROFILE" environment variable:

```bash
AWS_PROFILE=myprofile dapr run...
```

or

```bash
AWS_PROFILE=myprofile daprd...
```

You can use any of the [supported environment variables](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-envvars.html#envvars-list) to configure Dapr in this manner.

{{% /codetab %}}

 <!-- windows -->
{{% codetab %}}

On Windows, the environment variable needs to be set before starting the `dapr` or `daprd` command, doing it inline (like in Linux/MacOS) is not supported.

{{% /codetab %}}

{{< /tabs >}}


### Authenticate to AWS if using AWS SSO based profiles

If you authenticate to AWS using [AWS SSO](https://aws.amazon.com/single-sign-on/), some AWS SDKs (including the Go SDK) don't yet support this natively. There are several utilities you can use to "bridge the gap" between AWS SSO-based credentials and "legacy" credentials, such as:
- [AwsHelper](https://pypi.org/project/awshelper/) 
- [aws-sso-util](https://github.com/benkehoe/aws-sso-util)

{{< tabs "Linux/MacOS" "Windows" >}}
 <!-- linux -->
{{% codetab %}}

If using AwsHelper, start Dapr like this:

```bash
AWS_PROFILE=myprofile awshelper dapr run...
```

or

```bash
AWS_PROFILE=myprofile awshelper daprd...
```
{{% /codetab %}}

 <!-- windows -->
{{% codetab %}}

On Windows, the environment variable needs to be set before starting the `awshelper` command, doing it inline (like in Linxu/MacOS) is not supported.

{{% /codetab %}}

{{< /tabs >}}

## Next steps

{{< button text="Refer to AWS component specs >>" page="components-reference" >}}

## Related links

For more information, see [how the AWS SDK (which Dapr uses) handles credentials](https://docs.aws.amazon.com/sdk-for-go/v1/developer-guide/configuring-sdk.html#specifying-credentials).
