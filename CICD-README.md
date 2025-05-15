# CICD Pipeline Configuration

## Description

AWS Codepipeline was selected as the CICD tool of choice due to it's ability to visualize and automate the steps required to deploy the required infrastructure. 

### Pipeline stages

The CICD pipeline consists of the following stages:

- Source (category:source)	- Uses a CodeStarSourceConnection to connect to GitHub repository containing Infrastructure files
- qa-vpc-setup (category:deploy)	- Deploys the vpc-setup.json to a QA account based on the 'TemplatePath'
- qa-ec2-setup (category:deploy)	- Deploys the ec2-setup.json to a QA account based on the 'TemplatePath'
- production-setup-approval (category:approval)	- Approval stage which uses an SNS topic of your choice to approve the production builds.
- vpc-setup (category:deploy)		- Deploys the vpc-setup.json to a Production account based on the 'TemplatePath'
- ec2-setup (category:deploy)		- Deploys the ec2-setup.json to a Production account based on the 'TemplatePath'		


### Pipeline rationale

- The source stage being Github will be used due to it's easy integration nature with Codepipeline. 
- Two QA deploy stages with the first being the networking setup as the compute/ec2 is dependent.
- Approval stage via either email, Slack/Teams integration to be certain that we want a deployment to go to Production.
- Two Production deployment stages again with the first being the networking setup as the compute/ec2 is dependent.

For the sake of this workflow, it makes sense for it to flow in a linear fashion (non parallel deployments) due one section being QA with a dependancy and one being Production also with a dependancy.


### Pipeline monitoring

Codepipeline was also chosen due to it's variety of informative events upon execution. This events could be monitored by 'AWS Eventbridge' i.e:

{
  "source": ["aws.codepipeline"],
  "detail-type": ["CodePipeline Pipeline Execution State Change"]
}

This data could then be routed to a target such as SNS and/or a Lambda which notifies a team, performs a rollback based on a failure etc.




### Deployment commands




### Additional changes