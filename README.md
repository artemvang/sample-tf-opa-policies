# Terraform OPA policies examples

[![OPA tests](https://github.com/Scalr/sample-tf-opa-policies/workflows/OPA/badge.svg)](https://github.com/Scalr/sample-tf-opa-policies/actions?query=workflow%3AOPA)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](https://github.com/Scalr/sample-tf-opa-policies/pulls)
[![Documentation Status](https://readthedocs.com/projects/scalr-athena/badge/?version=latest)](https://docs.scalr.com/en/latest/opa.html)

A sample collection of OPA policies to test against Terraform run in Scalr.

## Resources

* Blog series for getting started with OPA for Terraform in Scalr : Link
* Documentation for using OPA with Scalr : https://docs.scalr.com/en/latest/opa.html
* OPA documentation : https://www.openpolicyagent.org/docs/latest/

## Policy Files

Each sample policy comprises 3 files.

- `*.rego` : The OPA policy
- `*.mock.json` : Input data for testing the policy with `opa test`
- `*.test.rego` : Definition of the tests that `opa test` should run and expected results

Each folder also includes an example `scalr-policy.hcl` file. These files are used by Scalr to implement enforcement levels for each policy (`hard-mandatory`, `soft-mandatory`, `advisory`). See [Enabling and Enforcing Policy](https://docs.scalr.com/en/latest/opa.html#enabling-and-enforcing-policy) for more details.

## Policy Evaluation

You can evaluate a policy against your own terraform plans using the Terraform CLI and `opa eval` as follows.

```bash
$ terraform plan --out planfile
$ terraform show -json planfile > plan.json
$ opa eval --format pretty --data policy.rego -i plan.json data.terraform.deny
```

## Policies

Summary decscriptions of each policy. Detailed descriptions of each rule can be found as comments in the policy file.
Many policies contain arrays of values that are checked against resources. The arrays and reason messages are of course customisable.

| Policy                                 | Description                                                              |
| -------------------------------------- | ------------------------------------------------------------------------ |  
| [aws/enforce_aws_iam_and_workspace.rego](https://github.com/Scalr/sample-tf-opa-policies/blob/master/aws/enforce_aws_iam_and_workspace.rego) | Checks valid IAM roles for provder and workspace.                        |
| [aws/enforce_aws_resource.rego](https://github.com/Scalr/sample-tf-opa-policies/blob/master/aws/enforce_aws_resource.rego) | Check resource types against an allowed list.                            |
| [aws/enforce_ebs_del_on_term.rego](https://github.com/Scalr/sample-tf-opa-policies/blob/master/aws/enforce_ebs_del_on_term.rego) | Check `delete_on_termination = true` is set for EBS volumes.             |
| [aws/enforce_iam_instance_profiles.rego](https://github.com/Scalr/sample-tf-opa-policies/blob/master/aws/enforce_iam_instance_profiles.rego) | Check IAM instance profiles is in allowed list.                          |
| [aws/enforce_kms_key_names.rego](https://github.com/Scalr/sample-tf-opa-policies/blob/master/aws/enforce_kms_key_names.rego) | Check KMS keys (by name) against allowed list.                           |
| [aws/enforce_s3_buckets_encryption.rego](https://github.com/Scalr/sample-tf-opa-policies/blob/master/aws/enforce_s3_buckets_encryption.rego) | Check encryption is set for S3 buckets.                                  |
| [aws/enforce_sec_group.rego](https://github.com/Scalr/sample-tf-opa-policies/blob/master/aws/enforce_sec_group.rego) | Check security groups have been sepcified and are in allowed list.       |
| [cost/limit_monthly_cost.rego](https://github.com/Scalr/sample-tf-opa-policies/blob/master/cost/limit_monthly_cost.rego) | Check estimated cost against an upper limit.                             |
| [external_data/random_decision.rego](https://github.com/Scalr/sample-tf-opa-policies/blob/master/external_data/random_decision.rego) | Example of using external data (HTTP GET) in a policy.                   |
| [management/denied_provisioners.rego](https://github.com/Scalr/sample-tf-opa-policies/blob/master/management/denied_provisioners.rego) | Checks provisioner types against an allowed list.                        |
| [management/enforce_ami_owners.rego](https://github.com/Scalr/sample-tf-opa-policies/blob/master/management/enforce_ami_owners.rego) | Checks AMI's being used belong to allowed list of AMI owners.            |
| [management/instance_types.rego](https://github.com/Scalr/sample-tf-opa-policies/blob/master/management/instance_types.rego) | Checks instance types/sizes against allowed list. AWS, Azure and GCP.    |
| [management/resource_tags.rego](https://github.com/Scalr/sample-tf-opa-policies/blob/master/management/resource_tags.rego) | Checks required tags are configured for all clouds.                      |
| [management/whitelist_ami.rego](https://github.com/Scalr/sample-tf-opa-policies/blob/master/management/whitelist_ami.rego) | Checks AMI against allowed list or configured from data source.          |
| [management/workspace_name.rego](https://github.com/Scalr/sample-tf-opa-policies/blob/master/management/workspace_name.rego) | Simple example of using `tfrun` data and validating a workspace name.    |
| [management/workspace_tags.rego](https://github.com/Scalr/sample-tf-opa-policies/blob/master/management/workspace_tags.rego) | Checks workspace is tagged with provider name.                           |
| [modules/pin_module_version.rego](https://github.com/Scalr/sample-tf-opa-policies/blob/master/modules/pin_module_version.rego) | Enforces use of specific module versions.                                |
| [modules/required_modules.rego](https://github.com/Scalr/sample-tf-opa-policies/blob/master/modules/required_modules.rego) | Checks resources are only be created via specific modules.               |
| [placement/cloud_location.rego](https://github.com/Scalr/sample-tf-opa-policies/blob/master/placement/cloud_location.rego) | Checks resources are deployed to specific regions in each cloud.         |
| [providers/blacklist_provider.rego](https://github.com/Scalr/sample-tf-opa-policies/blob/master/providers/blacklist_provider.rego) | Implements a provider blacklist.                                         |
| [user/user.rego](https://github.com/Scalr/sample-tf-opa-policies/blob/master/user/user.rego) | Restricts which users can trigger terrafrom runs. Works for CLI and VCS. |

## Contributions

We welcome contributions in many ways!

### Report Bugs

Submit bug reports at https://github.com/Scalr/sample-tf-opa-policies/issues.

Be sure to include the following.

* Link to the policy file.
* The `terraform plan` data in JSON format you tested against.
* The `opa eval` command used to reproduce the problem and it's output.

### Feedback and Suggestions

Submit feed back and suggestions at https://github.com/Scalr/sample-tf-opa-policies/issues.

Be sure to include the following.

* Detailed description of how you expect a policy to work.
* Sample `terraform plan` data the policy should work against.

### Pull Requests

Better still have a go at fixing bugs or implementing new policy examples yourself and submit a Pull Request.

If you submit a new policy you must include the following files.

* The `*.rego` file with the policy code.
* `*.mock.json` containing test data mocks. You should inlcude data for both valid and invalid evaluation of each rule in the policy,.
* `*.test.rego` defining the tests to be run and expected results when the PR checks are performed.

To submit a PR do the following.

1. Clone the repo locally.
```
# SSH
$ git clone git@github.com:Scalr/sample-tf-opa-policies.git

# HTTPS
$ git clone https://github.com/Scalr/sample-tf-opa-policies.git
```
2. Create a branch.
```
$ git checkout -b policy-name
```

Now write the policy or fix the bug. Confirm it works using `opa eval` as shown above and validate the mock based tests work using `opa test`.

Example test

```
# opa test enforce_sec_group.* -v 
data.terraform.test_valid: PASS (7.390418ms)
data.terraform.test_invalid: PASS (603.837µs)
data.terraform.test_missing: PASS (483.272µs)
--------------------------------------------------------------------------------
PASS: 3/3
```

3. Commit and push to GitHub.
```
    $ git add .
    $ git commit -m "Description"
    $ git push origin policy-name
```

4. Submit a pull request in GitHub.


## License

The examples are licensed under the [MIT License](https://github.com/Scalr/sample-tf-opa-policies/blob/master/LICENSE).


