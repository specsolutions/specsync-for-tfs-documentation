# re-link

Re-links scenarios (local test cases) to cloned Test Cases in Azure DevOps.

See more details about the command in the [Re-link scenarios to new Test Cases](../../features/common-synchronization-features/re-link-scenarios.md) article.

## Options

In addition the the options listed here, all [common command line options](./#common-command-line-options) can also be used.

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
      <td style="text-align:left"><code>--cloneOpId </code>&lt;CLONE&#x2011;OPERATION&#x2011;ID&gt;</td>
      <td style="text-align:left">The Id (e.g. <code>11</code>) of the Clone Operation the scenarios should be re-linked for.</td>
      <td
      style="text-align:left">detect from Test Cases</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>---workItemClonesFile </code>&lt;CSV&#x2011;FILE&#x2011;PATH&gt;</td>
      <td style="text-align:left">A path of a CSV file that contains additional Work Item clones to be processed in link tags. The file must have a `source` and a `target` column.</td>
      <td
      style="text-align:left">Work item link tags are not changed</td>
    </tr>
  </tbody>
</table>

## Examples

Performs the re-link operation where SpecSync will detect the performed Clone Operation automatically from the Test Cases.

```text
dotnet specsync re-link
```

Performs the re-link operation for a specific Code Operation ID `42`.

```text
dotnet specsync re-link --cloneOpId 42
```

Performs the re-link operation in dry-run mode (no actual changes are made)

```text
dotnet specsync re-link --dryRun
```

{% page-ref page="./" %}
