# Versioning Update

Starting with all versions after 2.5.0, we are changing our versioning to follow a semantic versioning standard.&#x20;

Old version for 2.5.0 and versions prior followed the format of PlatformVersion.Major.Minor, such as IDO Version 2, Major Version 5, and Minor version 0. This format presented challenges and pain points for the team when patches and hotfixes were necessary because we couldn't display the hotfix version which led to confusion about which version was actually deployed out in the world.

The new versioning change will remove the "PlatformVersion" concept and add an integer for hotfix version, which will more closely follow the principles of [Semantic Versioning](https://semver.org). The versioning will be Major.Minor.Hotfix, and as an example for our first release using this format, the version will be 5.1.0 instead of 2.5.1.

This shouldn't change anything infrastructure or deployment related for environments, as this change is not meant to affect those processes.

