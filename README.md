# Deploy-Static-Website-to-AWS-S3

This is a short project that automatically deploys your static website to AWS s3 bucket once it's pushed to your repository.



## pre-requisites

1 - AWS user credentials with s3 full access permission, this link can help you to  [create AWS user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html)

2 -  Create S3 bucket and configure it for static hosting [ help ](https://docs.aws.amazon.com/AmazonS3/latest/dev/HostingWebsiteOnS3Setup.html)

## STEPS

1- Go to https://github.com and create your repository.

2- On your github repository, go to Settings then Secrets

3- Click New Secret.

4- Enter AWS_ACCESS_KEY_ID on Name field.

5- Enter your AWS access key on the Value field.

6- Click Add secret

7- Repeat 4 - 6 for the AWS_SECRET_ACCESS_KEY

8- Clone your repository to your local machine.
 
## Creating github actions workflow
1- go to your repository directory and create workflow directory

```mkdir -p .github/workflows ```

2- Create a file named main.yml inside .github/workflows folder.

```touch .github/workflows/main.yml```

3- Open the yaml file and past the following 
```
name: CI
on: 
  push:
    branches:
      - main #here we choose to deploy only when a push is detected on the main branch
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v1
    - name: Configure AWS Credentials
      uses: aws-actions/configure-aws-credentials@v1
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: us-east-1 # Use your bucket region here
        
    # Here you could add some building steps if you were dealing with some angular/react/Vue...
    # - name: Build static site
    #  run: yarn install && npm run-script build
    
    - name: Deploy static site to S3 bucket
      run: aws s3 sync ./thefoldertodeploy s3://BUCKET_NAME --delete
```

4- Change BUCKET_NAME with the name of your bucket created earlier. Same with the AWS-region.

5- Create a folder named ```thefoldertodeploy``` in your repo. root directory and move your website to it.

6- Push your repository 
```
git add .

git commit -m "Commit message"

git push 
```
7- Go to your AWS account and check your website.


## Usage
*** You can simple clone this repo and add your website to the folder named ```thefoldertodeploy``` and push it back to your Repo.

