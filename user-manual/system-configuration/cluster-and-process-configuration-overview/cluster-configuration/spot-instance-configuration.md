# Spot instance configuration

Databricks jobs can be configured to use [SPOT ](https://aws.amazon.com/ec2/spot/)instances which significantly reduces costs (typically by 50-70%). IDO default cluster cluster configuration is configured to use databricks pool with all SPOT instance types.

When SPOT capacity is not available, jobs configured to use SPOT instances from the pool will fail to start with error "no Spot capacity available".  There are 2 approaches to get around this:

#### Configure pool with instance type that is least likely to get interruptions

1. Open [AWS spot advisor](https://aws.amazon.com/ec2/spot/instance-advisor/), select your region and find the instance type supported by databricks (list here) with minimal Frequency of Interruption (<5%). For general workloads we recommend all purpose instance types (m5\*.large or m5\*.xlarge). For jobs having complex joins, aggregations, window functions and large data volumes we recommend memory-optimized (r5\*.large, r5\*.xlarge) types.&#x20;
2. Create new databricks pool to use selected instance type: ![](<../../../../.gitbook/assets/image (380).png>)![](<../../../../.gitbook/assets/image (381).png>)
3. Click create, and copy new pool id from the browser URL:![](<../../../../.gitbook/assets/image (378).png>)
4. Update IDO cluster configuration to use pool\_id of the new pool:![](<../../../../.gitbook/assets/image (384).png>)

If this approach still results in SPOT availability errors during job launch:&#x20;

#### Update IDO cluster configuration to use SPOT\_FALLBACK

SPOT_FALLBACK option allows databricks to use regular-priced EC2 instances when SPOT capacity is not available. As of Dec 2021, databricks pools don't support SPOTFALLBACK option. To get around this limitation, reconfigure IDO cluster to not use pool:_![](<../../../../.gitbook/assets/image (383).png>) __&#x20;

&#x20;_and select SPOT\_WITH\_FALLBACK option:_![](<../../../../.gitbook/assets/image (382).png>)__
