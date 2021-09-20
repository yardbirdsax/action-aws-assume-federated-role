# Assume AWS IAM Roles Using OIDC Federated Authentication

This Action allows a workflow to assume an AWS IAM Role using OIDC based authentication.

> **PLEASE NOTE**: This is as-of-yet (19th of September 2021) undocumented functionality, so this action should probably not be used in production environments as GitHub may change things before public release. See [this GitHub Roadmap item](https://github.com/github/roadmap/issues/249) for a current status of the feature.

This can be used in concert with [this Terraform module](https://github.com/yardbirdsax/terraform-aws-github-action-federated-role), which can create a special role that can be assumed using this process.

For a good explanation of all this, see [this excellent blog post](https://awsteele.com/blog/2021/09/15/aws-federation-comes-to-github-actions.html) by Aidan Steele.

## Usage

Here's an example of how to use this action in a workflow:

```
name: Pull Request Check
on:
  pull_request:
    branches:
      - "trunk"

jobs:
  pull_request:
    name: Pull Request Check
    runs-on: ubuntu-20.04
    # This section is required
    permissions:
      id-token: write
      contents: read
    steps:
      - name: Check out
        uses: actions/checkout@v2
        with:
          ref: ${{ github.event.pull_request.head.ref }}
      - name: Acquire AWS credentials
        uses: "yardbirdsax/action-aws-assume-federated-role@master"
        with:
          aws_iam_role_arn: "arn:aws:iam::xxxxxxxxxxxx:role/rolename"
          aws_default_region: us-east-1
```
