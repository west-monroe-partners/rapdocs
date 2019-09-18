# Management + Increment Script Deployment Steps

Before running the Management + Increment script, complete the parameter checklist located in [Management + Increment Parameter Deployment Checklist.](../management-+-increment-parameter-deployment-checklist.md)

1. **Login** to AWS Management Console and navigate to CloudFormation
2. **Select** Create Stack
3. Under “Choose a template” **check** “Specify an Amazon S3 template URL 
4. **Copy** the S3 link to the _Management + Increment_ script into the field
5. **Click** Next
6. **Fill in** the parameters based on the checklist values you created.
7. **Click** Next
8. **Click** Next
9. **Review** the parameters to be sure they are correct
10. **Tip**: Select “Quick Create Stack” at the bottom. This copies the parameters you entered to a new window so you do not need to reenter them in the event of a rollback.
11. **Check** the box that “I acknowledge that AWS CloudFormation might create IAM resources with custom names.”
12. **Click** Create
13. If rollback occurs, **assess** the errors and tweak parameters accordingly 
14. If successful, **continue** to [Post-Management + Increment Script Deployment Steps](../post-deployment-steps/post-management-+-increment-script-deployment-steps.md)

