# Section-8
# Deploying-Sample-App-on-AWS-(With-CodePipeline)

# cp-singleec2-lab-1
**Step 1.Goto AWS Management Console>Services>Developers Tools>CodePipeline>Create Pipeline**

**Step 2.In Pipeline Settings give following details:**
- Pipeline name - first-cp-single-ec2
- Service role - New service role
- Select Allow AWS CodePipeline to create a service role
- Advanced settings
  - Artifact store - Default location
  - Encryption key - Default AWS Managed key

Click on Next

**Step3-Source in Add source stage:**
- Source provider - AWS CodeCommit
- Repository name - Sample-Node-App
- Branch name - master
- Change detection options - Amazon CloudWatch Events
- CodePipeline - CodePipeline default

Click on Next

**Step 4.Build - optional in Add build stage**
- Build provider - AWS code build
- Region - Asia Pacific(Mumbai)
- Project name - first-cd-project
- Build type - Single build

Click on Next

**Step 5.Deploy-optional in Add deploy stage**
- Deploy provider - AWS CodeDeploy
- Region - Asia Pacific(Mumbai)
- Application name - cd-app
- Deployment group - cd-app-dg (Created in section-7)

Click on Next

**Step 6.Review all stages**
- Click on Create pipeline

**Step 7. The pipeline has been created**
- See that Source stage in progress

**Step 8.Goto S3>Buckets>codepipeline-ap-south-1-xxxxxx**
- Now Goto Objects>first-cp-single-ec2/>SourceArti/>4xRn5Ab>Details
  - Click on Object URI - It is Not Public

**Step 9.Go back to see the Pipeline progress**
- Source stage is completed and Source Artfacts are created in S3
- Build is in progress
  - After the build completion Build Artfacts will be created

**Step 10.Goto S3>Buckets>codepipeline-ap-south-1-xxxxxx**
- Now Goto Objects>first-cp-single-ec2/>BuildArtif/>xxxx
 
**Step 11.Go back to see the Pipeline progress**
- Build stage is completed and Build Artfacts are created in S3
- Deploy is succeeded

**Step 12.Deploy is completed**
- Copy the Public ip address and paste in browser to see that Application running

**Step 13.Goto CloudWatch>Events>Rules>codepipeline-Sample-master-640051-rule**
- See the Summary of Event pattern

**Step 14. Goto IAM>Roles>AWSCodePipelineServiceRole-ap-south-1-first-cp-single-ec2**
- See Permissions>Policy

**Step 15.Open Visual Studio Code and goto pages>index.ejs**
- Edit the file for new color
- save it
- Run the following command
```sh
$ git add .
$ git commit -m "change in index color for cp "
$ git push
```

**Step 16.Go back to see the change in Pipeline**
- Pipeline is activated Just now
- See the latest commit - "change in index color for cp "
- Monitor all stages Build>Source>Deploy

**Step 17. To see what is happening click on AWS CodeBuild**
- Developers Tools>CodeBuild>Build projects>first-cd-project>Build History
- Developers Tools>CodeDeploy>Applications>cd-app>cd-app-dg>deployment-id>deployment status

**Step 18.Copy the Public ip and paste in browser to see that Application is running with different color**
- Task completed

# End of lab


# cp-asg-alb-lab-2

**Step 1.Goto Developers Tools>CodeDeploy>Applications>cd-app>cd-app-asg-alb**
- Click to edit deployment group
  - In Deployment type - Select In-Place
  - Click on Save changes
Now deployment group is updated

**Step 2.In Pipeline Settings give following details:**
- Pipeline name - cp-asg-alb
- Service role - Select Existing service role
- Select Allow AWS CodePipeline to create a service role
- Advanced settings
  - Artifact store - Default location
  - Encryption key - Default AWS Managed key

Click on Next

**Step3-Source in Add source stage:**
- Source provider - AWS CodeCommit
- Repository name - Sample-Node-App
- Branch name - master
- Change detection options - Amazon CloudWatch Events
- CodePipeline - CodePipeline default

Click on Next

**Step 4.Build - optional in Add build stage**
- Build provider - AWS code build
- Region - Asia Pacific(Mumbai)
- Project name - first-cd-project
- Build type - Single build

Click on Next

**Step 5.Deploy-optional in Add deploy stage**
- Deploy provider - AWS CodeDeploy
- Region - Asia Pacific(Mumbai)
- Application name - cd-app
- Deployment group - cd-app-asg-alb

Click on Next

**Step 6.Review all stages**
- Before creating pipeline confirm that Ec2 instances are running in EC2>AutoScalingGroup
- Click on Create pipeline

**Step 7. The pipeline has been created**
- Monitor the progress of all the stages Build>Source>Deploy

**Step 8.Goto S3>Buckets>codepipeline-ap-south-1-xxxxxx**
- Now Goto Objects>cp-asg-alb>BuildArtif/>xxxx

**Step 9.Developers Tools>CodeDeploy>Applications>cd-app>cd-app-asg-alb**
- Click to see deployment details and history

**Step 10.Goto Ec2>target groups to see that no instances are there due to deregistration for deployment**

**Step 11.Goto Load Balancers and click on its Dns**
- 502 bad gateway error

**Step 12.Goto EC2>target groups>Registered target**
- 2 targets are registered

**Step 13.Goto Load Balancers and click on its Dns**
- See ALB is sending traffic to these targets

**Step 14.Go back to see the Pipeline progress**
- Deployment is completed

**Step 15.Open Visual Studio Code and goto pages>index.ejs**
- Edit the file for new color
- Save it
- Run the following command
```sh
$ git add .
$ git commit -m "change in index color for cp-asg-alb "
$ git push
```

**Step 16.Go back to see the change in Pipeline**
- Pipeline is activated Just now
- See the latest commit - "change in index color for cp-asg-alb "
- Monitor all stages Build>Source>Deploy

**Step 17.Developers Tools>CodeBuild>Build projects>first-cd-project**
- click on In-progress Build
  - Goto Phase details to see the status
  
**Step 18.Goto S3>Buckets>codepipeline-ap-south-1-xxxxxx**
- Now Goto Objects>cp-asg-alb
  - BuildArtif/>xxxx
  - SourceArti/

Click on BuildArtifacts and SourceArtifacts to see more details

# End of lab


# cp-ebs-lab-3

**Step 1.AWS Console>Services>Elastic Beanstalk>Create Application**
- Create a web app
  - Application Name- sample-node
  - Platform 
    - Platform 
      - Node.js
   - Platform branch
      - Node.js 12 running on 64bit Amazon Llinux2
   - Platform version
      - 5.2.3(Recommended)
   - Application Code - Select Sample application

Click on Create Application.

**Step 2. Creating environment started**
- Elastic BeanStalk launches an environment named SampleNode-env
- Click on SampleNode-env to see it running

**Step 3. In Pipeline Settings give following details:**
- Pipeline name - cp-ebs
- Service role - Select Existing service role
- Select Allow AWS CodePipeline to create a service role
- Advanced settings
  - Artifact store - Default location
  - Encryption key - Default AWS Managed key

Click on Next

**Step 4.Source in Add source stage:**
- Source provider - AWS CodeCommit
- Repository name - Sample-Node-App
- Branch name - master
- Change detection options - Amazon CloudWatch Events
- CodePipeline - CodePipeline default

Click on Next

**Step 5.Build - optional in Add build stage**
- Build provider - AWS code build
- Region - Asia Pacific(Mumbai)
- Project name - first-cd-project
- Build type - Single build

Click on Next

**Step 6.Deploy-optional in Add deploy stage**
- Deploy provider - AWS Elastic BeanStalk
- Region - Asia Pacific(Mumbai)
- Application name - sample-node
- Deployment group - Sample-node-env

Click on Next

**Step 7.Review all stages**
- Click on Create pipeline

**Step 8. The pipeline has been created**
- Monitor progress of all the stages Build>Source>Deploy

**Step 9 Deployment has been completed** 
- Refresh the Sample-node-env link 
- See that it is running

**Step 10.Open Visual Studio Code and goto pages>index.ejs**
- Edit the file for new color
- save it
- Run the following command
```sh
$ git add .
$ git commit -m "changed in index color for ebs dep with cp "
$ git push
```
**Step 11.Go back to see the change in Pipeline**
- Pipeline is activated Just now
- See the latest commit - "change in index color for ebs dep with cp "
- Monitor all stages Build>Source>Deploy


**Step 12.Deployment has been completed** 
- Refresh the Sample-node-env link
- See that color is changed

# End of Lab

# cp-staging-preprod-prod-lab-1

**Step 1.Open cp-ebs pipeline (Refer ebs lab)**
- Click on Edit
  - Edit:Deploy>+Add stage
    - Give stage name - Pre-prod
	- Click on Add stage

**Step 2.Goto Pre-prod stage>+Add action group**
- Action name - Approval-1
- Action provider - Manual approval
- SNS topic ARN - arn:aws:sns:ap-south-1xxxxx-Topic
- URL for review - Copy Sample-node-env URL	

Click on Done

**Step 3.Pre-prod>Approval-1>+ Add action group
- Action name - single-ec2-deployment
- Action provider - AWS CodeDeploy
- Region - Asia Pacific(Mumbai)
- Input artifacts - BuildArtifact
- Application name - cd-app
- Deployment group - cd-app-dg

Click on Done

**Step 4.Click on ADD stage**
- Stage name - Prod
- Click on Add stage

**Step 5.In Prod stage click + Add action group**
- Action name - Approval-2
- Action provider - Manual approval
- SNS topic ARN - arn:aws:sns:ap-south-1xxxxx-Topic
- URL for review - http://13.232.176.111:3000/	

Click on Done

**Step 6.In Prod stage click + Add action group**
- Action name - Multiple-ec2-deployment
- Action provider - AWS CodeDeploy
- Region - Asia Pacific(Mumbai)
- Input artifacts - BuildArtifact
- Application name - cd-app
- Deployment group - cd-app-asg-alb

To save Pipeline - Click on Save 

**Step 7. See that Pipeline has been started**

# End of Lab

# cp-staging-preprod-prod-lab-2

**Step 1.Goto "cp-ebs" Pipeline**
- Click on Release change>Release

**Step 2.Pipeline has been started**
- Source stage completed
- Build stage completed
- Deploy stage completed on Elastic BeanStalk
- Pre-prod stage -Waiting for approval

**Step 3.Goto your Email-Inbox to approve the Pre-prod stage**
- Search for email with Subject:APPROVAL NEEDED: AWS CodePipeline-cp-ebs for action Approval-1
- Click on link to Approve

**Step 4.Go back to Pipeline and click on Review in Pre-prod stage**
- Give approval comment
- Click on Approve

**Step 5.Pre-prod "single-ec2-deployment" started after approval**

**Step 6.Goto your Email-Inbox to approve the Prod stage**
- Search for email with Subject:APPROVAL NEEDED: AWS CodePipeline-cp-ebs for action Approval-2
- Click on link to Approve

**Step 7.Go back to Pipeline and click on Review in Prod stage**
- Give approval comment
- Click on Approve

**Step 8.Deployment to Multiple-ec2-deployment is succeeded**

**Step 9.To Validate it Goto Ec2>Load Balancers>alb-cd>Copy DNS name**
- Paste it in browser to see it running
- Refresh it and see that both Instances are receving traffic by ALB**

# End of lab




























