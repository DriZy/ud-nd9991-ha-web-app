# Deploy a High-Availability Web App using CloudFormation

## Problem

Your company is creating an Instagram clone called Udagram. Developers pushed the latest version of their code in a zip file located in a public S3 Bucket.

You have been tasked with deploying the application, along with the necessary supporting software into its matching infrastructure.

This needs to be done in an automated fashion so that the infrastructure can be discarded as soon as the testing team finishes their tests and gathers their results.

## Solution

### Diagram

![Diagram](/docs/udagram.png)

### Description

The solution consist modular infrastructure which diferential diferent stacks.

### Files structure

```md
.
├── README.md
├── bastion
│   ├── bastion-parameters.json
│   └── bastion-stack-template.yml
├── bucket
│   ├── s3-bucket-parameters.json
│   └── s3-bucket-stack-template.yml
├── docs
│   └── udagram.png
├── iam
│   ├── iam-parameters.json
│   └── iam-stack-template.yml
├── network
│   ├── network-parameters.json
│   └── network-stack-template.yml
├── scripts
│   ├── create-secure-key.sh
│   ├── create-stack.sh
│   ├── delete-secure-key.sh
│   ├── delete-stack.sh
│   └── update-stack.sh
├── server
│   ├── server-parameters.json
│   └── server-stack-template.yml
├── src
│   └── index.html
└── udacity.zip
```

### Instructions

This is a project that anyone could uses to learn about CloudFormation, if you want to deploy this project follow the instructions are below:

1. You need to create a Secure KeyPair that will use to connect to Bastion Host, in a terminal using the file `scripts/create-secure-key.sh`

    ```bash
    # Create secure keys
    scripts/create-secure-key.sh
    ```

2. (Optional) Create a zip file named `udacity.zip` to be uploaded to s3.

    ```bash
    zip udacity.zip src/*
    ```

3. You need to create the s3 and iam stacks, in a terminal using the file `scripts/create-stack-key.sh`.

    ```bash
    # Create IAM stack
    scripts/create-stack.sh iam-stack iam/iam-stack-template.yml iam/iam-parameters.json

    # Create S3 stack.
    scripts/create-stack.sh s3-stack bucket/s3-bucket-stack-template.yml bucket/s3-bucket-parameters.json
    ```

4. You need to upload the website files created previously to s3 bucket, in a terminal using aws cli

    ```bash
    # Upload zip to s3 bucket
    aws s3 cp udacity.zip s3://aanorbel-udagram-s3-store
    ```

5. You need to create the other stacks described above, in a terminal

    ```bash
    # Create the network described in the diagram above
    scripts/create-stack.sh network-stack network/network-stack-template.yml network/network-parameters.json

    # Create the bastion server
    scripts/create-stack.sh bastion-stack bastion/bastion-stack-template.yml bastion/bastion-parameters.json

    # Create the server stack
    scripts/create-stack.sh server-stack server/server-stack-template.yml server/server-parameters.json
    ```

You can access to the final website using this LoadBalancer [link](http://serve-webap-vwkrcg37fv0t-2068805576.us-west-2.elb.amazonaws.com/)
