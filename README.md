# AWS CLI Lab: IAM Policy Retrieval and JSON Validation

## 1. Lab Overview

This lab demonstrates how to use the AWS Command Line Interface (AWS CLI) to interact with AWS Identity and Access Management (IAM), retrieve an IAM policy document, and save the policy locally as a JSON file.

The lab provided practical experience with AWS CLI commands, query filtering, output formatting, file redirection, and JSON validation.

## 2. Objectives

By completing this lab, I learned how to:

- Use AWS CLI commands to list IAM users and customer-managed policies.
- Retrieve an IAM policy version using its Amazon Resource Name (ARN).
- Filter command output using the `--query` parameter.
- Format command output as JSON.
- Save an IAM policy document to a local file.
- Validate JSON syntax using Python.

## 3. Environment

- **Platform:** Amazon Web Services (AWS)
- **Operating system:** Amazon Linux EC2 environment
- **Tool:** AWS CLI
- **AWS service:** Identity and Access Management (IAM)
- **File format:** JSON

## 4. Lab Tasks and Commands

### Task 1: List IAM users

```bash
aws iam list-users
```

This command retrieves the IAM users available to the configured AWS identity, subject to its permissions.

### Task 2: List customer-managed IAM policies

```bash
aws iam list-policies --scope Local
```

This command lists customer-managed IAM policies in the AWS account.

### Task 3: Filter the policy information

```bash
aws iam list-policies --scope Local \
  --query "Policies[?PolicyName=='lab_policy'].[PolicyName,Arn,DefaultVersionId]" \
  --output table
```

This command filters the results to display the policy name, ARN, and default version in a readable table.

### Task 4: Retrieve the policy document

```bash
aws iam get-policy-version \
  --policy-arn "arn:aws:iam::613881455420:policy/lab_policy" \
  --version-id v1
```

This command retrieves version `v1` of the specified IAM policy.

### Task 5: Save the policy document as JSON

```bash
aws iam get-policy-version \
  --policy-arn "arn:aws:iam::613881455420:policy/lab_policy" \
  --version-id v1 \
  --query 'PolicyVersion.Document' \
  --output json > lab_policy.json
```

The `--query` parameter extracts the policy document, while `--output json` formats it as JSON. The `>` operator redirects the output into `lab_policy.json`.

### Task 6: Verify the saved file

```bash
ls -l lab_policy.json
```

This command confirms that the file exists and displays its size and permissions.

### Task 7: Validate the JSON syntax

```bash
python -m json.tool lab_policy.json
```

This command parses and formats the JSON document. Successful output confirms that the file contains valid JSON syntax.

## 5. IAM Policy Structure

The retrieved policy contains two statements:

- **AllowAllActions:** Allows the actions explicitly listed in the statement, subject to applicable AWS authorization rules.
- **RestrictActions:** Explicitly denies selected EC2 actions.

The policy includes permissions associated with services such as EC2, CloudWatch, CloudFormation, Systems Manager, IAM, SNS, and other AWS services.

**Security note:** The policy uses `Resource: "*"`, which can apply permissions across resources covered by the listed actions. Permissions should follow the principle of least privilege in production environments. Explicit denies override conflicting allows.

## 6. Challenges and Troubleshooting

During the lab, I encountered an error while retrieving the policy version because the command was incomplete and the required `--version-id` parameter was missing.

I resolved the issue by supplying the correct policy ARN and specifying version `v1`.

This reinforced the importance of reading AWS CLI error messages and checking required command parameters.

## 7. Key Learnings

- AWS CLI enables command-line interaction with AWS services.
- IAM policies define permissions for AWS actions and resources.
- The `--query` parameter helps filter command output.
- The `--output` parameter controls output formatting.
- Shell output redirection can save command results to files.
- Python can validate JSON syntax.
- IAM permissions must be reviewed carefully to avoid excessive access.

## 8. Conclusion

This hands-on lab strengthened my understanding of AWS CLI operations, IAM policy inspection, and JSON file handling. These skills are useful for cloud administration, troubleshooting, security reviews, and automation.

**Lab status:** Completed policy retrieval, file creation, and JSON validation.
