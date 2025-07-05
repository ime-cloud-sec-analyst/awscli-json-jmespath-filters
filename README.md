# awscli-json-jmespath-filters
A collection of AWS CLI examples demonstrating JSON querying with JMESPath, alongside common Linux tools (jq, grep, sed) for filtering and parsing.
awscli-cloudformation-jmespath-lab

A collection of AWS CLI examples demonstrating JSON querying with JMESPath, alongside common Linux tools (jq, grep, sed) for filtering and parsing.

Table of Contents

Introduction

Prerequisites

Repository Structure

Usage Examples

1. Querying Local JSON with JMESPath

2. Filtering with Linux Tools

3. AWS CLI + CloudFormation + JMESPath

Contributing

License

Introduction

This repository contains step-by-step examples and reference snippets to:

Work with local JSON files using jmsepath expressions, jq, grep, sed, and nl.

Use the AWS CLI for CloudFormation operations, including stack creation, troubleshooting, and JMESPath-based output filtering.

Illustrate common patterns for extracting, filtering, and transforming JSON data both locally and via AWS services.

Prerequisites

AWS CLI v2 installed and configured with credentials and default region.

Python and pip installed (for optional jq Python wrapper).

Common Linux utilities: jq, grep, sed, nl, and curl.

Repository Structure

README.md           ← This guide
local-json/         ← Examples for querying and filtering local JSON files
├── desserts.json
└── stack-resources.json
aws-cloudformation/ ← CloudFormation lab scripts & templates
├── template1.yaml
└── lab-commands.sh

Usage Examples

1. Querying Local JSON with JMESPath

# Extract all dessert names
aws cloudformation describe-stack-resources --stack-name dummy \
  --query "StackResources[*].LogicalResourceId" --output json

JMESPath tip: Use [] for arrays and ? filters for conditional selection.

2. Filtering with Linux Tools

# Print line numbers
nl desserts.json

# Show only the Ice cream block
sed -n '7,10p' desserts.json

grep -A 1 'Carrot cake' desserts.json

Linux tip: Combine grep, sed, and nl to inspect JSON without jq.

3. AWS CLI + CloudFormation + JMESPath

# Create a stack (no rollback on failure)
aws cloudformation create-stack \
  --stack-name myStack \
  --template-body file://template1.yaml \
  --capabilities CAPABILITY_NAMED_IAM \
  --on-failure DO_NOTHING \
  --parameters ParameterKey=KeyName,ParameterValue=myKey

# Watch resource statuses every 5s
watch -n 5 aws cloudformation describe-stack-resources \
  --stack-name myStack \
  --query 'StackResources[*].[ResourceType,ResourceStatus]' --output table

# Retrieve the EC2 instance ID
aws cloudformation describe-stack-resources \
  --stack-name myStack \
  --query "StackResources[?ResourceType=='AWS::EC2::Instance'].PhysicalResourceId" --output text

JMESPath in AWS CLI: Pass --query to filter/shape JSON responses directly.

Contributing

Feel free to open an issue or submit pull requests for improvements, additional examples, or corrections.

License

This project is licensed under the MIT License - see the LICENSE file for details.

