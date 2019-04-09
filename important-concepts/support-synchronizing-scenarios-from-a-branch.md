# Support synchronizing scenarios from a branch

In many larger project there are different teams that work on the same product using feature branches. Feature branches provide separation of the code from the master branch, however Azure DevOps test cases cannot be "branched" or "merged".

As long as the feature branches only add new scenarios and there are only minor modifications, the impact of this problem is not that high, but with more active changes of the scenarios this might cause inconsistencies.

As one option to solve this problem, SpecSync provides a feature called "branch-tags". The branch-tags are alternative tag prefixes for linking scenarios on a branch to test cases. E.g. for the master, you use the `@tc:123` style tags for linking the scenarios. On a branch for Feature A, you can configure to use another prefix, like `tc-feature-a` \(`@tc-feature-a:1234` style\). 

The branch tag feature should be enabled on the feature branches in the main `specsync.json` congifuration file \(in this case these changes must not be merged back to the master\), or to a branch-specific config file, like `specsync-feature-a.json`. Each different feature branch can have different branch tag prefix.

When the branch-tag feature is enabled, SpecSync will link new scenarios with the branch-tag prefix. If it detects changes on existing linked scenarios \(that were linked before the branch\), instead of changing those, it creates a new test case and links it with the branch-tag prefix \(so these scenarios will have two test case tags on the branch\). Once the branch has been merged back, the branch-tag prefixes have to be removed or converted to normal tag prefixes.

The branch tag feature can be configured in the [`customizations` section](../configuration/configuration-customizations.md) of the configuration file.

