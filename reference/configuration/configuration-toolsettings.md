# toolSettings

This configuration section contains settings for the synchronization tool.

The following example shows the available options within this section.

```javascript
{
  ...
  "toolSettings": {
    "licensePath": "specsync.lic",
    "disableStats": false,
    "outputLevel": "normal",
    "parentConfig": "specsync-common.json",
    "ignoreParentConfig": false
  }, 
  ...
}
```

## Settings

<table>
  <thead>
    <tr>
      <th style="text-align:left">Setting</th>
      <th style="text-align:left">Description</th>
      <th style="text-align:left">Default</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>licensePath</code>
      </td>
      <td style="text-align:left">Path for the license file. Can contain an absolute or a relative path
        to the config file folder. It may contain environment variables in <code>...%MYENV%...</code> form.
        Can be overridden by the <code>--license</code>  <a href="../command-line-reference/">command line option</a>.
        See <a href="../../licensing.md">Licensing</a> for details.</td>
      <td style="text-align:left"><code>specsync.lic</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>disableStats</code>
      </td>
      <td style="text-align:left">If set to true, SpecSync will not collect anonymous error diagnostics
        and statistics. Can be overridden by the <code>--disableStats</code>  <a href="../command-line-reference/">command line option</a>.</td>
      <td
      style="text-align:left"><code>false</code>
        </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>outputLevel</code>
      </td>
      <td style="text-align:left">Set the detail level of error messages and trace information displayed
        by the tool. Available options: <code>normal</code>, <code>verbose</code> and <code>debug</code>.
        Can be overridden by the <code>--verbose</code>  <a href="../command-line-reference/">command line option</a>.</td>
      <td
      style="text-align:left"><code>normal</code>
        </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>parentConfig</code>
      </td>
      <td style="text-align:left">Path for the parent config file. See <a href="../../features/general-features/hierarchical-configuration-files.md">Hierarchical configuration files</a> for
        details.</td>
      <td style="text-align:left">not specified, the files in the parent folder will be considered</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>ignoreParentConfig</code>
      </td>
      <td style="text-align:left">If set to true, SpecSync will not apply the configuration settings of
        the config files in the parent folders or the user-specific configuration
        settings. See <a href="../../features/general-features/hierarchical-configuration-files.md">Hierarchical configuration files</a> for
        details.</td>
      <td style="text-align:left"><code>false</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>logFile</code>
      </td>
      <td style="text-align:left">Path for the log file to be created. Can be overridden by the <code>--log</code> 
        <a
        href="../command-line-reference/#common-command-line-options">command line option</a>.</td>
      <td style="text-align:left">no log file</td>
    </tr>
    <tr>
      <td style="text-align:left">plugins</td>
      <td style="text-align:left">
        <p>An array of plugin configurations. See <a href="../../features/general-features/specsync-plugins.md">SpecSync Plugins</a> for
          details.</p>
        <ul>
          <li><code>assemblyPath</code> &#x2014; The path for the SpecSync plugin assembly
            (dll file)</li>
          <li><code>parameters</code> &#x2014; Optional parameters for the plugin in
            key-value pairs. Please contact the maintainer of the plugin for the specific
            values.</li>
        </ul>
      </td>
      <td style="text-align:left"></td>
    </tr>
  </tbody>
</table>

{% page-ref page="./" %}

{% page-ref page="../../features/general-features/hierarchical-configuration-files.md" %}

