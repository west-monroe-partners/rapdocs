# Post-Increment Script Deployment Steps

Once the CloudFormation stack is CREATE\_COMPLETE, several post deployment steps must be performed. 

1. **Verify** resource deployment 

   a. **Check** CloudFront is deployed 

   b. **Check** CoudBuild, and CodeDeployment was created 

   c. **Check** load balancers were created and see target instances 

2. **Setup** OpenVPN Primary instance \(If applicable\) 

   a. Link to [AWS OpenVPN Documentation ](https://openvpn.net/vpn-server-resources/amazon-web-services-ec2-tiered-appliance-quick-start-guide/)

3. **Stop** OpenVPN Backup Instance 
4. **Create** IAM group **WMP.Rap.Engineers** and grant AmazonRDSFullAccess, AmazonEC2FullAccess, AmazonS3FullAccess, AWSCodeDeployFullAccess, CloudFrontFullAccess, AmazonSESFullAccess, AmazonSESFullAccess, AmazonSSMFullAccess, AWSCodeBuildFullAccess, AmazonRoute53FullAccess 

   a. **Create** user accounts for the Rap team and save the csv files 

5. **Create** IAM group **WMP.Deployment.Accounts** and grant AmazonRDSFullAccess, AmazonS3FullAccess, AWSCodeDeployFullAccess 

   a. **Create the following users** for programmatic access and save the AWS keys 

        i. **Create** svc.s3user and grant svc-s3user-rds-access-policy, this policy can be found in WMP-      
           RAP VSTS environment 

        ii. **Create** svc.ses.smtp and grant AmazonSESFullAccess

        iii. **Create** svc.&lt;increment&gt;.vsts and Grant &lt;increment&gt;-ETL-API-SSM-PARAM-Policy that was created during the Management + Increment script 

6. **Configure** DNS records for the environment 

   a. **A record alias**: Elastic Load Balancer api.&lt;domainname&gt;.com 

   b. **A record alias**: CloudFront Distribution www.&lt;domainname&gt;.com 

7. **Configure** Simple Email Service \(SES\) in the US East \(N. Virginia\) region. 

   a. **Click** Domains, verify a New Domain, enter the domain and generate DKIM Settings 

   b. **Add** the appropriate Route53 Records 

The following steps will be completed by the Rap Engineers: 

    8. **Add** parameters to the EC2 parameter store for the increment environment   
        a. **Optional**: Apply tags to the parameters   
    9. **Configure** EC2 [Parameter Store parameters.](../parameter-store-parameters.md)  
  a. **Note**: The script only creates String type parameters due to a limitation of CloudFormation.   
      Secure String parameters need to be manually created. 

The environment is now ready to deploy code into.

