# SageMaker with Terraform

[Amazon SageMaker](https://aws.amazon.com/sagemaker/) is the new service in AWS to build, train,
and deploy machine learning models at scale.

We've already [started to build Terraform support](https://github.com/terraform-providers/terraform-provider-aws/issues/2493) for SageMaker
and if you don't want to wait until its released, we have good news - you basically can start using it right now. Below, we show you how.

You find the Terraform code in the [main.tf](main.tf).

### Prerequisites

* awscli  (>= 1.14.2)
* terraform (>= 0.10.0)

### Build a custom provider

As SageMaker support isn't released yet, you need to build your own custom AWS provider based on the new feature
branch:

```
    git clone https://github.com/cloudetc/terraform-provider-aws.git $GOPATH/src/github.com/terraform-providers/terraform-provider-aws
    cd $GOPATH/src/github.com/terraform-providers/terraform-provider-aws/
    git checkout feature/resource-aws-sagemaker-endpoint
    
    make build
    
    cp $GOPATH/bin/terraform-provider-aws  ~/.terraform.d/plugins/
``` 


### Deploy an example model

Let's use the [example provided by 
AWS](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/advanced_functionality/scikit_bring_your_own/scikit_bring_your_own.ipynb)
to show how to deploy your own algorithm container with SageMaker using Terraform:

```
    # clone the example repo from AWS
    git clone https://github.com/awslabs/amazon-sagemaker-examples.git
    cd amazon-sagemaker-examples/advanced_functionality/scikit_bring_your_own/container/
    
    # export credentials for your account
    export AWS_ACCESS_KEY_ID=<your-access-key-id>
    export AWS_SECRET_ACCESS_KEY=<your-secret-access-key>
    
    # create docker algorithm container and push it to ECS
    ./build_and_push.sh foo
    
    git clone https://github.com/cloudetc/terraform-sagemaker-example.git
    cd terraform-sagemaker-example/
    terraform init
    terraform apply
    
    # make test call to the deployed model
    aws runtime.sagemaker invoke-endpoint --endpoint-name terraform-sagemaker-example \
        --body "`cat ./local_test/payload.csv`" --content-type "text/csv" "output.dat"
```
