# Pre-Management + Increment Script

1. **Create/Login** to AWS account/management console

       a. [Creating an AWS Account](https://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account/)

2. **Acquire** domain name to be used in the environment
3. **Create** Route53 hosted zone for the new domain. Create applicable records if using a sub domain in the parent domain DNS registrar
4. **Generate** certificates in AWS Certificate Manager. Be sure to enter all alternate names that will be needed:

       a. **Ex**. \*.example.com, example.com

       b. Certificates need to be created in us-east-1 \(N. Virginia\) for CloudFront, and the region you are   
            deploying the infrastructure into for the load balancers

5. **Create** a EC2 key pair to be used in the deployment. **BE SURE TO SAVE THE KEY!**
6. **Create** a S3 bucket to store the CloudFormation and instance install scripts

       a. This bucket can and should be private

       b. **Bucket Name**: s3-ue2-rap-deploy- &lt;clientID&gt;

7. **Copy** the _Management + Increment script, API install script, and ETL install script_ to the S3 bucket you created.

       a. **Note**: CloudFormation scripts are stored in the WMP-RAP VSTS environment

8. **Subscribe** to the Centos 7, OpenVPN, Microsoft Server 2016 Base image. 

       a. **Centos 7**: [https://aws.amazon.com/marketplace/fulfillment?productId=b7ee8a69-ee97-4a49-  
           9e68-afaee216db2e&ref\_=dtl\_psb\_continue](https://aws.amazon.com/marketplace/fulfillment?productId=b7ee8a69-ee97-4a49-9e68-afaee216db2e&ref_=dtl_psb_continue)

       b. **OpenVPN**: [http://aws.amazon.com/marketplace/pp?sku=8icvdraalzbfrdevgamoddblf](http://aws.amazon.com/marketplace/pp?sku=8icvdraalzbfrdevgamoddblf)

       c. **Microsoft Server 2016 Base:** [https://aws.amazon.com/marketplace/fulfillment?  
           productId=13c2dbc9-57fc-4958-922e-a1ba7e223b0d&ref\_=dtl\_psb\_continue](https://aws.amazon.com/marketplace/fulfillment?productId=13c2dbc9-57fc-4958-922e-a1ba7e223b0d&ref_=dtl_psb_continue)

9. You are now ready to run the Management + Increment script. **Continue** to [Management + Increment Script Deployment Steps](../cloudformation-script-deployment-steps/management-+-increment-script-deployment-steps.md)

\*\*\*\*

