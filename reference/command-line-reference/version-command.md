# version

Displays the version of SpecSync and the license information available based on the current project location.

The license information is displayed based on the configuration and license files available at the current folder. Alternatively you can specify a different configuration file or license file. 

Pulls changes from Azure DevOps server to the local repository. This by default includes creation of new scenarios from unlinked test cases \(create\) and changing scenarios of linked test cases \(change\).

## Options

In addition the the options listed here, all [common command line options](./#common-command-line-options) can also be used, except the user credential specific options, like `--user`.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Option</th>
      <th style="text-align:left">Description</th>
      <th style="text-align:left">Default</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p><code>--bare</code>
        </p>
        <p>(short name <code>-b</code>)</p>
      </td>
      <td style="text-align:left">Only displays the product version and does not attempt to check the configuration
        and the license file.</td>
      <td style="text-align:left">Version, release date and license information are displayed.</td>
    </tr>
  </tbody>
</table>

## Examples

Displays the version and license information based on the license and configuration files in the current folder:

```text
C:\Work\MyCalculator\MyCalculator.Specs> dotnet specsync version

SpecSync for Azure DevOps v3.2.5 (7/1/2021)
Copyright © 2016-2021 Spec Solutions

Enterprise license for Spec Solutions, valid until 2023/04/07, support code: XXXXX
(Please provide the support code for reporting incidents or license renewal.)

Configuration loaded from:
  C:\Work\MyCalculator\MyCalculator.Specs\specsync.json
  C:\Users\gaspar\AppData\Local\SpecSync\specsync.json

```

Displays version and license information of a specific license file:

```text
dotnet specsync version --license C:\MyProject\specsync.lic
```

Displays the bare product version:

```text
> dotnet specsync version --bare

3.2.5
```

