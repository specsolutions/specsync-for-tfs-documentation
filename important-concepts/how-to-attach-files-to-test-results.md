# How to attach files to test results

The publish-test-results command loads and publishes test results from the test result output file of your test execution tool (e.g. a TRX or XML file). These files might not only contain test execution outcome, error messages and other execution details, but might also contain a reference to additional files called test result attachments (e.g. a screenshot of the execution when the test failed). When SpecSync detects such an attachment, it attaches the files to the test result, so you can also browse the file in Azure DevOps along with the other test results.

The different test execution tools provide different methods for attaching files to the current test execution, and unfortunately there are some [test execution tools that do not support](#frameworks-no-attachments) such an option. The aim to this guide is to provide help about how this can be achieved with the different tools. The list might not be complete. If you have found a way to attach files with a tool that is not listed here, please [contact us](../contact/specsync-support.md) so that we can extend the list. 

## Attaching files to test results with SpecFlow & NUnit

With NUnit you need to save the attachment file to the file system and then report the file path to NUnit using the `TestContext.AddTestAttachment` method:

```C#
NUnit.Framework.TestContext.AddTestAttachment(attachmentPath, "My attachment");
```

## Attaching files to test results with SpecFlow & MsTest

With MsTest you need to save the attachment file to the file system and then report the file path to MsTest using the current `TestContext` instance. To obtain the current `TestContext` instance, you need to specify it among the constructor parameters of your step definition class and save it to an instance field. (In case of hooks, you can list the `TestContext` in the hook method parameters.)

```C#
[Binding]
public class MyStepDefinitions
{
  private readonly TestContext _testContext;

  public MyStepDefinitions(TestContext testContext)
  {
    _testContext = testContext;
  }

  [When(@"a test attachment is saved")]
  public void WhenATestAttachmentIsSaved()
  {
    string attachmentPath = // save attachment & calculate attachment path

    _testContext.AddResultFile(attachmentPath);
  }
}
```

## Test execution frameworks that do not support test attachments <a href="frameworks-no-attachments" id="frameworks-no-attachments"></a>

For the frameworks that do not support recording test result attachments, you can use the following workaround: Save all files to be attached in a new folder and then attach all files to the published Test Run using the `--attachedFiles` option of the [publish-test-result command](../reference/command-line-reference/publish-test-results-command.md). Unfortunately in this case the files will not be attached to a particular test result but to the entire Test Run, so it is advised to use file names that contain identifiable information about the scenario.

* SpecFlow & xUnit (xUnit does not support test attachments)
