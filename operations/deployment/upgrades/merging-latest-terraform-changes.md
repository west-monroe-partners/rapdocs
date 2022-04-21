# Merging Latest Terraform Changes

When performing a version upgrade, it is sometimes necessary to update Terraform infrastructure to support the upgrade.&#x20;

The master Terraform repository is hosted in GitHub and looks like the below image. Please reach out to your Intellio DataOps project team to gain access to this repository. There will be a master-\* branch for each minor and major version starting with 2.4.3, i.e. "master-2.4.3" and "master-2.5.0".

![](<../../../.gitbook/assets/image (380) (1) (1) (1) (1).png>)

When deploying the initial DataOps workspace in Terraform, the Terraform Cloud account is connected to a forked version of this repository. As new master version branches are added the forked repositories will slowly become behind the master in terms of commits.

![](<../../../.gitbook/assets/image (363).png>)

Merging the latest changes from the dataops-infrastructure repository to the forked dataops-infrastructure can be done by navigating to the forked repository and making a pull request from the master dataops-infrastructure branch that matches the version that the infrastructure is upgrading to. For example, if an upgrade to 2.5.0 is being made, then the pull request should be made from "master-2.5.0".

![](<../../../.gitbook/assets/image (381) (1) (1) (1) (1) (1).png>)

Once the pull request is created, review the changes and make sure that any custom changes that may be made aren't getting wiped out by the PR. If they are, manual edits may need to be made to the PR, or updates may need to be made post PR merge.

Once the forked repository is up to date with the latest changes, move forward with the version upgrade.

[AWS](../deployment/deployment-to-amazon-web-services/new-version-upgrade-process-terraform.md)&#x20;

[Azure](../deployment/deployment-to-microsoft-azure/new-version-upgrade-process.md)
