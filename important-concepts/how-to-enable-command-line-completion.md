# How to enable command line completion

SpecSync supports command line completion through the .NET `dotnet-suggest` tool (see changelog v5.0.0). The setup differs depending on how you use SpecSync.

## SpecSync as global tool or downloaded executable

If you install SpecSync as a global .NET tool or use the downloaded executable, follow the official Microsoft guide for enabling tab completion: https://learn.microsoft.com/en-us/dotnet/standard/commandline/how-to-enable-tab-completion

## SpecSync as local .NET tool

When SpecSync is installed as a local .NET tool, PowerShell needs an extra shim script to register completions for the `dotnet specsync` command. Use these steps after completing the Microsoft guide above:

1. Enable `dotnet-suggest` according to the Microsoft instructions.
2. Download the SpecSync shim script from https://content.specsolutions.eu/specsync/dotnet-tool-completion-shim.ps1 and place it next to the `dotnet-suggest-shim.ps1` script referenced in your PowerShell profile.
3. Update your PowerShell profile (for example `Microsoft.PowerShell_profile.ps1`) to load both scripts (in this order):

```
. "$PSScriptRoot\dotnet-suggest-shim.ps1"
. "$PSScriptRoot\dotnet-tool-completion-shim.ps1"
```

After saving the profile, restart PowerShell or reload the profile, then try pressing <kbd>Tab</kbd> after `dotnet specsync` to confirm completion works.
