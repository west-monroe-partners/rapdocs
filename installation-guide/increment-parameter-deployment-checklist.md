# Increment Parameter Deployment Checklist

| **Parameter** | **Description** | **Allowed Values \(Default Bolded\)** |
| :--- | :--- | :--- |
| **Account Foundations** |  |  |
| acmCert | Enter the certificate ARN to use for ALB TLS/SSL connections. This should be a certificate tied to your domain name. | N/A |
| acmCFCert | Enter the certificate ARN to use for CloudFront TLS/SSL connections. This should be a certificate tied to your domain name in the us-east-1 region | N/A |
| awsaza | Enter the first availability zone you would like to deploy to. Default is us-east-2a | us-east-2a |
| awsazb | Enter the second availability zone you would like to deploy to. Default is us-east-2b | us-east-2b |
| awsregion | Enter the Region you would like to deploy to. Default is us-east-2 \(Ohio\) | us-east-2 |
| awsregionshort | Enter the shorthand naming for the Region you would like to deploy to. Default is ue2 \(Ohio\) | ue2 |
| clientid | In order to support S3 blobally unique names, please enter a Client ID to be appeneded to S3 names. MUST BE LOWER CASE | N/A |
| Incrementname | In order to identify resources, please enter a name for the Increment. Examples: Management, Production, Test, Development, Test 2, etc... | N/A |
| Incrementnameshort | In order to identify resources, please enter a unique letter for the Increment. Examples: m = Management, p = Production, t = Test, d = Development, t2 = Test 2, etc... | N/A |
| myKeyPair | Enter an Existing Amazon EC2 Key Pair to enable SSH access to all EC2 Instances | N/A |
| **VPC Configuration** |  |  |
| mgmtRTId | Enter the Management Route Table ID | N/A |
| secmvpnid | Select the VPN Security Group ID | N/A |
| vpcflowlogrole | Enter the arn for the VPC Flow Log Role | N/A |
| vpcincrcidr | Enter the Increment VPC CIDR Reservation. Default is 10.2.0.0/16 | 10.2.0.0/16 |
| vpcmgmtcidr | Enter the Management VPC CIDR. Default is 10.0.0.0/16 | 10.0.0.0/16 |
| vpcmgmtid | Select the Management VPC ID | N/A |
| **Subnet Configuration** |  |  |
| subappaz1 | Enter the Increment VPC's AZ1 Application Tier Subnet Reservation. Default is 10.2.2.0/24 | 10.2.2.0/24 |
| subappaz2 | Enter the Increment VPC's AZ2 Application Tier Subnet Reservation. Default is 10.2.5.0/24 | 10.2.5.0/24 |
| subdbaz1 | Enter the Increment VPC's AZ1 Database Tier Subnet Reservation. Default is 10.2.3.0/24 | 10.2.3.0/24 |
| subdbaz2 | Enter the Increment VPC's AZ2 Database Tier Subnet Reservation. Default is 10.2.6.0/24 | 10.2.6.0/24 |
| subwebaz1 | Enter the Increment VPC's AZ1 Web Tier Subnet Reservation. Default is 10.2.1.0/24 | 10.2.1.0/24 |
| subwebaz2 | Enter the Increment VPC's AZ2 Web Tier Subnet Reservation. Default is 10.2.4.0/24 | 10.2.4.0/24 |
| **EC2 IPv4 Allocation** |  |  |
| apiip | Enter the Increment VPC's API Private IPv4 Reservation in the AZ1 Web Tier. Default is 10.2.2.100 | 10.2.2.100 |
| etlip | Enter the Increment VPC's ETL Private IPv4 Reservation in the AZ1 Application Tier. Default is 10.2.2.150 | 10.2.2.150 |
| pwrbigwip | Enter the Increment VPC's PowerBI Gateway Private IPv4 Reservation in the AZ1 Application Tier. Default is 10.2.2.125 | 10.2.2.125 |
| sftpip | Enter the Increment VPC's SFTP Private IPv4 Reservation in the AZ1 Web Tier, Default is 10.2.1.100 | 10.2.1.100 |
| **EC2 Instance Type** |  |  |
| APItype | Enter the API Instance Type. Default is t2.small | t2.micro,**t2.small**,t2.medium, t2.large, m4.large, m4.2xlarge, m4.4xlarge |
| EnablePWRBI | Select Yes if using PowerBI | Yes, **No** |
| ETLtype | Enter the Increment ETL Instance Type. Default is r4.large. | t2.micro,,t2.small,t2.medium, t2.large, m4.large, m4.2xlarge, m4.4xlarge, r4.2xlarge, r4.xlarge, **r4.large** |
| PWRBIGWtype | Enter the Increment SFTP Instance Type. Default is r4.large. Enter 'none' for no SFTP instance. | t2.micro,,t2.small,t2.medium, t2.large, m4.large, r4.2xlarge, r4.xlarge, **r4.large** |
| SFTPtype | Enter the Increment SFTP Instance Type. Default is t2.medium. Enter 'none' for no SFTP instance. | t2.micro,,t2.small,**t2.medium**, t2.large, m4.large, m4.2xlarge, m4.4xlarge, none |
| **Storage** |  |  |
| ebsetllanding | Enter the ETL EBS Landing Storage Allocation. Default is 125GB \(gp2\) | 125 |
| ebsetloutput | Enter the ETL EBS Output Storage Allocation. Default is 125GB \(gp2\) | 125 |
| ebspwrbisize | Enter the PowerBI SQL Storage Allocation. Default is 250GB \(gp2\) | 250 |
| ebssftpinbox | Enter the SFTP EBS Inbox Storage Allocation. Default is 25GB \(gp2\) | 25 |
| transitiontoAA | Enter the Number of Days to Transition S3 Data to the Infrequently Accessed Storage Class. Default is 30 Days | 30 |
| TransitiontoGLACIER | Enter the Number of Days to Transition S3 Data to the Glacier Storage Class. Default is 180 Days | 180 |
| **RDS Configuration** |  |  |
| RAPEnhancedMonitoringRole | Enter the RDS Enhanced Monitoring Role arn. | N/A |
| RDSclustername | Enter the Name for the RDS Cluster | Postgres |
| RDScount | Do you need 1 or 2 RDS isntances? \(Dev/test environments usually only need 1\) Default is 1 | **1**, 2 |
| RDSmasterpassword | Please Enter an Administrative Password for the RDS Database | N/A |
| RDSmasterusername | Please Enter an Administrative username for the RDS Database | rap\_admin |
| RDSport | Enter the RDS port | 6534 |
| RDSretentionperiod | The number of days for which automatic backups are retained. | 1,2,3,4,5,6,**7** |
| RDStype | Enter the Increment RDS Instance Type. Default is db.r4.large. | **db.r4.large**, db.r4.xlarge, db.r4.2xlarge |
| **Redshift** |  |  |
| EnableRedshift | Select Yes if using Redshift | No |
| RedshiftClusterType | Enter the type of Redshift cluster | **single-node**, multi-node |
| RedshiftDatabaseName | Enter the name of the first database to be created when the Redshift cluster is created | dev |
| RedshiftMasterUsername | Enter The user name that is associated with the Redshift master user account for the cluster that is being created | defaultuser |
| RedshiftMasterUserPassword | Enter the password that is associated with the Redshift master user account for the cluster that is being created | N/A |
| RedshiftNodeType | Enter the type of Redshift node to be provisioned | ds2.xlarge, ds2.8xlarge, **dc2.large**, dc2.8xlarge |
| RedshiftNumberOfNodes | Enter the number of compute nodes in the Redshift cluster. For multi-node clusters, the NumberOfNodes parameter must be greater than 1 | 1 |
| RedshiftPortNumber | Enter the port number on which the Redshift cluster accepts incoming connections | 5439 |
| **CloudFront** |  |  |
| cfCNAME | Enter the domain name to be used as a Cloudfront CNAMe. Example: mydomain.com, or demo.mydomain.com for a demo environment | N/A |
| **CodePipeline and CodeDeploy** |  |  |
| s3codepipelineartifactsname | Enter the S3 Git Deploy Bucket ARN | N/A |
| s3gitarn | Enter the S3 CodePipepline Artifacts Bucket Name. | N/A |

