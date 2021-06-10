# Merging Latest Terraform Changes

When performing a version upgrade, it is sometimes necessary to update Terraform infrastructure to support the upgrade. 

The master Terraform repository is hosted in GitHub and looks like this. Please reach out to your Intellio DataOps project team to gain access to this repository.

![](../../.gitbook/assets/image%20%28361%29.png)

When deploying the initial DataOps workspace in Terraform, the Terraform Cloud account is connected to a forked version of this repository. As changes are added to the master repository, the forked repositories will slowly become behind the master in terms of commits.

![](../../.gitbook/assets/image%20%28363%29.png)

Merging the latest changes from the master dataops-infrastructure repository to the forked dataops-infrastructure can be done by navigating to the forked repository and making a pull request from the master dataops-infrastructure branch.

![](../../.gitbook/assets/image%20%28360%29.png)

Make sure that the base repository is the forked repository and the head repository is intellio/dataops-infrastructure. You may need to click "compare across forks" to get to the intellio/dataops-infrastructure option on the right side.

Once the pull request is created, review the changes and make sure that any custom changes that may be made aren't getting wiped out by the PR. If they are, manual edits may need to be made to the PR, or updates may need to be made post PR merge.

Once the forked repository is up to date with the latest changes, move forward with the version upgrade.

[AWS](deployment-to-amazon-web-services/new-version-upgrade-process-terraform.md) 

[Azure](deployment-to-microsoft-azure/new-version-upgrade-process.md)

