# Generating Synthetic Datasets for Evaluating Retrieval Augmented Generation Systems

As Retrieval Augmented Generation (RAG) systems become more prevalent, evaluating their performance is essential to ensure quality and performance. However, collecting real-world data for evaluation can be costly and time-consuming, especially in the early stages of a project. This lab provides a hands-on guide to leveraging large language models and knowledge retrieval context to generate synthetic evaluation datasets that mimic real human interactions. It covers setting up an end-to-end workflow using Python and the Amazon Bedrock API.

By leveraging large language models and knowledge retrieval context, the proposed approach ensures that the synthetic datasets are diverse, realistic, and representative of real-world scenarios. This solution is relevant for developers and researchers working on RAG systems, as it streamlines the evaluation process and accelerates the iterative development cycle, ultimately leading to better-performing AI systems.

The notebook guides you through generating a synthetic dataset for a QA-RAG application using the Bedrock API, Python and Langchain. The notebook consists of the following chapters: 

1. Set-up of the environment
2. Loading and preparing context data
3. Initial Question Generation
4. Answer Generation
5. Extracting Relevant Context
6. Evolving Questions to fit end-users behaviour
7. Automated Dataset Generation
8. Assessing the Questions quality


## Getting started

### Choose a notebook environment

This lab is presented as a **Python notebook**, which you can run from the environment of your choice:

- [SageMaker Studio](https://aws.amazon.com/sagemaker/studio/) is a web-based integrated development environment (IDE) for machine learning. To get started quickly, refer to the [instructions for domain quick setup](https://docs.aws.amazon.com/sagemaker/latest/dg/onboard-quick-start.html).
- [SageMaker Notebook Instance](https://docs.aws.amazon.com/sagemaker/latest/dg/howitworks-create-ws.html) is a machine learning (ML) compute instance running the Jupyter Notebook App.
- To use your existing (local or other) notebook environment, make sure it has [credentials for calling AWS](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html).


### Enable AWS IAM permissions for Bedrock

The AWS identity you assume from your notebook environment (which is the [*Studio/notebook Execution Role*](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-roles.html) from SageMaker, or can be a role or IAM User for self-managed notebooks), must have sufficient [AWS IAM permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html) to call the Amazon Bedrock service.

To grant Bedrock access to your identity:

- Open the [AWS IAM Console](https://us-east-1.console.aws.amazon.com/iam/home?#)
- Find your [Role](https://us-east-1.console.aws.amazon.com/iamv2/home?#/roles) (if using SageMaker or otherwise assuming an IAM Role), or else [User](https://us-east-1.console.aws.amazon.com/iamv2/home?#/users)
- Select *Add Permissions > Create Inline Policy* to attach new inline permissions, open the *JSON* editor and paste in the below example policy:

```
{
    "Version": "2012-10-17",
    "Statement": {
        "Sid": "AllowInference",
        "Effect": "Allow",
        "Action": [
            "bedrock:InvokeModel"
        ],
        "Resource": "arn:aws:bedrock:*::foundation-model/*"
    }
}
```

> ℹ️  **Note:** With Amazon SageMaker, your notebook execution role is typically be *separate* from the user or role that you log in to the AWS Console with. If you want to explore the AWS Console for Amazon Bedrock, you need to grant permissions to your Console user/role too. You can run the notebooks anywhere as long as you have access to the AWS Bedrock service and have appropriate credentials

For more information on the fine-grained action and resource permissions in Bedrock, check out the Bedrock [Developer Guide](https://docs.aws.amazon.com/bedrock/latest/userguide/what-is-bedrock.html).

### Clone and use the notebooks

> ℹ️ **Note:** In SageMaker Studio, you can open a "System Terminal" to run these commands by clicking *File > New > Terminal*

Once your notebook environment is set up, clone this workshop repository into it.

```sh
git clone https://github.com/aws-samples/generating-synthetic-datasets-for-evaluating-retrieval-augmented-generation-systems.git
cd generating-synthetic-datasets-for-evaluating-retrieval-augmented-generation-systems/Notebook 
```


You're now ready to explore the lab notebook! You will be guided through connection the notebook to Amazon Bedrock for large language model access.


## Contributing

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License 
This lab is licensed under the MIT-0 License. 

