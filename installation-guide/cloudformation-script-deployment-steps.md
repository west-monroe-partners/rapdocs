# CloudFormation Script Deployment Steps

Before running the Increment script, complete the parameter checklist located in [Increment Parameter Deployment Checklist.](increment-parameter-deployment-checklist.md)

1. **Login** to AWS Management Console and navigate to CloudFormation
2. **Select** Create Stack
3. Under “Choose a template” **check** “Specify an Amazon S3 template URL 
4. **Copy** the S3 link to the _Increment_ script into the field
5. **Review** the parameters to be sure they are correct
6. **Tip**: Select “Quick Create Stack” at the bottom. This copies the parameters you entered to a new window so you do not need to reenter them in the event of a rollback.
7. **Check** the box that “I acknowledge that AWS CloudFormation might create IAM resources with custom names.”
8. **Click** Create
9. If rollback occurs, **assess** the errors and tweak parameters accordingly 
10. If successful, **continue** to [Post Deployment Steps](post-deployment-steps.md).



