# workshop-lambda

This project has build on the GitOps principles, so this Git repo act as your **Source of Truth**.....Elaborate on DR, IAC,....

## TODO (prioritised)
1. Create an application bucket to hold all artefacts and documentation
	Set correct CORS for swagger....
1. Add a custom domain name 
1. Add DLQ
1. Add a post hook example
1. Add a custom metric


## Requirements

* AWS CLI already configured with at least PowerUser permission
* [Python 3 installed](https://www.python.org/downloads/)
* [Pipenv installed](https://github.com/pypa/pipenv)
    - `pip install --user pipenv`
* [Docker installed](https://www.docker.com/community-edition)
* [AWS SAM CLI installed](https://github.com/awslabs/aws-sam-cli) 
	- `pip install --user aws-sam-cli`

Provided that you have the requirements above installed, proceed by installing the application dependencies and development dependencies:

```bash
pipenv install
pipenv install -d
```

### AWS SAM

This project uses [AWS Serverless Application Model (AWS SAM)](https://github.com/awslabs/serverless-application-model).

> **See [Serverless Application Model (SAM) HOWTO Guide](https://github.com/awslabs/serverless-application-model/blob/master/HOWTO.md) for more details in how to get started.**


## Add your code to Github
1. In your GitHub create a new repository named `workshop-lambda.
2. [Add your code to Github](https://help.github.com/articles/adding-an-existing-project-to-github-using-the-command-line/):

	```bash
	git init
	git add .
	git commit -m "Initial commit"
	git remote add origin git@github.com:bogguard/workshop-lambda.git
	git push -u origin master
	```

## Testing

`Pytest` is used to discover tests created under `tests` folder - Here's how you can run tests our initial unit tests:

```bash
make test
```

## Local development with AWS SAM CLI 

In order to run your code locally all dependencies need to be build and packaged on your local machine:
```
make build
```

Afterwards invoking your functions locally through a local API Gateway can be achieved by running:

```bash
make sam
```

If the previous command ran successfully you should now be able to hit the following local endpoint to invoke your functions:
- `http://localhost:3000/rest/one`
- `http://localhost:3000/rest/two`


## Continuous Deployment

### GitHub

In order to install the pipeline a GitHub token is required.
To create a token go to: https://github.com/settings/tokens

**Create a token with ```repo``` and ```admin:repo_hook``` permissions.**

### Deploying CodePipeline

A pipeline is included in this stack in order to make it easy to deploy your code on AWS. The pipeline has 3 steps:

1. **Source**: listen for Github code changes
1. **Build**: 
	- clean the workspace
	- lint your code and run unit tests
	- package the code for deployment
1. **Deploy**: deploy your code through CloudFormation on AWS


To install the pipeline run:

```bash
make create-pipeline OAUTH_TOKEN=your_github_token 
```
To update the pipeline run:

```bash
make update-pipeline OAUTH_TOKEN=your_github_token
```

To remove the pipeline run:

```bash
make delete-pipeline
```

## Appendix

### Makefile

It is important that the Makefile created only works on OSX/Linux but the tasks above can easily be turned into a Powershell or any scripting language you may want too.

Find all available targets: `make help`


