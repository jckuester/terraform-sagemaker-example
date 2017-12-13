# Sagemaker Example with Terraform


Work on Terraform support for Sagmaker is in progress. You have to checkout the HEAD of 
https://github.com/terraform-providers/terraform-provider-aws/pull/2479 and compile your custom AWS Terraform provider
until Sagmaker support is being officially realeased.

### Usage

We use the [example model provided by 
AWS](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/advanced_functionality/scikit_bring_your_own/scikit_bring_your_own.ipynb)
to show how to host your own algorithm container (i.e. trained model) with Sagemaker:

```
    # get the sagemaker example model
    git clone https://github.com/awslabs/amazon-sagemaker-examples.git
    cd amazon-sagemaker-examples/advanced_functionality/scikit_bring_your_own/container
    
    # export credentials for your account
    export AWS_ACCESS_KEY_ID=<your-access-key-id>
    export AWS_SECRET_ACCESS_KEY=<your-secret-access-key>
    
    # create docker container and push it to ECS
    ./build_and_push.sh foo
    
    terraform apply
    
    # make test call to the deployed model
    aws runtime.sagemaker invoke-endpoint --endpoint-name terraform-sagemaker-example \
        --body "`cat ./local_test/payload.csv`" --content-type "text/csv" "output.dat"
```
