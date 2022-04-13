# Versioning Update

Starting with all versions after 2.5.0, we are changing our versioning to follow a semantic versioning standard.&#x20;

Old version for 2.5.0 and versions prior followed the format of PlatformVersion.Major.Minor, such as IDO Version 2, Major Version 5, and Minor version 0. This format presented challenges and pain points for the team when patches and hotfixes were necessary because we couldn't display the hotfix version which led to confusion about which version was actually deployed out in the world.

The new versioning change will remove the "PlatformVersion" concept and add an integer for hotfix version, which will more closely follow the principles of [Semantic Versioning](https://semver.org). The versioning will be Major.Minor.Hotfix, and as an example for our first release using this format, the version will be 5.1.0 instead of 2.5.1.

Nothing involving infrastructure is changing as a result of this change. As an example for post 2.5.0, all that needs to be done for the release is to get latest Terraform changes using the versioned master branch (master-5.1.0), update the imageVersion variable in Terraform to 5.1.0, and plan/apply the Terraform changes.

{% hint style="warning" %}
We will no longer support rerunnable Deployment containers for hotfix patching - all hotfixes will now require an imageVersion variable update in Terraform and a Terraform run.
{% endhint %}

