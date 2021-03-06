# Metrics collected by PSCodeHealth

## Metrics collected for each function

### LinesOfCode  
It measures the number of lines of code in the specified function definition.  
The single line comments, multiple lines comments and comment-based help are not executable code, so they are not counted.  

Lower is better, because lenghty functions tend to be difficult to read and maintain.  

#### Default thresholds : ([Source](http://www.ndepend.com/docs/code-metrics#NbLinesOfCode))  
The thresholds are 50% higher than the recommendations in the above article because PowerShell advanced functions tend have more boilerplate code than C# methods (mainly due to the various parameter attributes being more widely used in PowerShell, for validation purposes, pipeline input, etc...).  
  - Warning : greater than 30  
  - Fail : greater than 60  
           
### ScriptAnalyzerFindings  
It counts the total number of best practice violations found by [PSScriptAnalyzer](https://github.com/PowerShell/PSScriptAnalyzer) in the specified function definition, regardless of their severity.  

**PSScriptAnalyzer** is a static code checker for PowerShell. It checks readability, quality and security best practices in PowerShell code by running a set of rules. The rules are based on PowerShell best practices identified by PowerShell Team and the community.  

PSCodeHealth uses the default PSScriptAnalyzer rules.  

Lower is better, because high numbers of PSScriptAnlyzer findings generally reflects low code quality, maintainability or safety.  

#### Default thresholds :  
  - Warning : greater than 7  
  - Fail : greater than 12  
  
### ContainsHelp  
Tells whether or not the specified function definition contains comment-based help. Possible values are : True or False.  

It is an important best practice to have help information for each function, even private function (for contributors, or code reviewers).  
At the very least, it should have a short statement describing the function's purpose. That is the **Synopsis**.

### TestCoverage  
It measures the percentage of lines of code in the specified function definition that are exercised (executed) during a suite of tests.  
Note : the code **executed** during tests cannot always be considered as **tested**. It is only **tested** if it is executed **and** there is one or more test(s) specifically designed to validate that the code behaves as expected.  

Higher is better. Good tests provide confidence that the code behaves as expected. How confident depends mostly on the percentage of code which is covered by the tests. This is why code coverage of the tests is an important metric. It is an indicator of how thoroughly the different possible code paths and edge cases are tested.  

#### Default thresholds : ([Source](https://github.com/PowerShell/DscResources/blob/master/HighQualityModuleGuidelines.md))  
  - Warning : less than 80  
  - Fail : less than 70  

### CommandsMissed  
The number of commands in the specified function which are not exercized by the tests.  
This is the number of commands, not lines, because the code coverage feature of **Pester** uses breakpoints, which can only be triggered by commands ([Source](https://github.com/pester/Pester/wiki/Code-Coverage)).  

Lower is better. Any command not exercized by tests is untested, which means any defect it may contain will not be detected. Or at best, it will only be detected at later stages of the code lifecycle (user acceptance testing, QA, or in production).  
The later a defect is detected, the more expensive (in time and money) it is to fix.  

#### Default thresholds :   
  - Warning : greater than 6  
  - Fail : greater than 12  

### Complexity  
This is the cyclomatic complexity of a given function. Cyclomatic complexity measures the number of possible execution paths through a given section of code. This is intended to evaluate code complexity.  

Lower is better, because complex code tends to have the following properties :  
  - Difficult to read/understand  
  - Difficult to test  
  - More prone to defects  
  - Make defects more difficult to identify
  - More difficult and risky to change/refactor/maintain  

#### Default thresholds : ([Source](http://www.ndepend.com/docs/code-metrics#CC))  
  - Warning : greater than 15  
  - Fail : greater than 30  

For more details on how the cyclomatic complexity is calculated, please refer to [this article](http://theshellnut.com/measuring-powershell-code-complexity-why-and-how/).

### MaximumNestingDepth  
This is the depth of the most deeply nested code in a given piece of code (a function, here). This measures a different aspect of complexity from the "cyclomatic complexity", so these 2 metrics are complementary. The nesting depth of a piece of code is an indication of the complexity of its context.  

Again, lower is better.  

#### Default thresholds : ([Source](http://www.ndepend.com/docs/code-metrics#ILNestingDepth))  
  - Warning : greater than 4  
  - Fail : greater than 8  

For more details on how the maximum nesting depth is calculated, please refer to [this article](http://theshellnut.com/measuring-powershell-code-complexity-why-and-how/).
  
## Metrics collected for the overall health report  

These are the overall metrics for all PowerShell code in the files or folder specified via the **Path** parameter.

### Files  
The number of PowerShell files. These are files with an extension like : '\*.ps\*1'.  
Test scripts are excluded, as are '\*.ps1xml' files because these don't contain any code.  

### Functions  
The overall number of function definitions found.  

### LinesOfCodeTotal  
The total number of lines of code across all files and functions.  

#### Default thresholds :  
  - Warning : greater than 1000  
  - Fail : greater than 2000  
  
### LinesOfCodeAverage  
The average number of lines of code per function.  

#### Default thresholds : ([Source](http://www.ndepend.com/docs/code-metrics#NbLinesOfCode))  
The thresholds are 50% higher than the recommendations in the above article because PowerShell advanced functions tend have more boilerplate code than C# methods (mainly due to the various parameter attributes being more widely used in PowerShell, for validation purposes, pipeline input, etc...).  
  - Warning : greater than 30  
  - Fail : greater than 60  
           
### ScriptAnalyzerFindingsTotal  
The total number of best practice violations found by [PSScriptAnalyzer](https://github.com/PowerShell/PSScriptAnalyzer) across all files.  

#### Default thresholds :  
  - Warning : greater than 30  
  - Fail : greater than 60  
           
### ScriptAnalyzerErrors  
The total number of best practice violations of 'Error' severity found by [PSScriptAnalyzer](https://github.com/PowerShell/PSScriptAnalyzer) across all files.  

#### Default thresholds :  
  - Warning : greater than 1  
  - Fail : greater than 3  
           
### ScriptAnalyzerWarnings  
The total number of best practice violations of 'Warning' severity found by [PSScriptAnalyzer](https://github.com/PowerShell/PSScriptAnalyzer) across all files. 

#### Default thresholds :  
  - Warning : greater than 10  
  - Fail : greater than 20  
           
### ScriptAnalyzerInformation  
The total number of best practice violations of 'Information' severity found by [PSScriptAnalyzer](https://github.com/PowerShell/PSScriptAnalyzer) across all files.  

#### Default thresholds :  
  - Warning : greater than 20  
  - Fail : greater than 40  
           
### ScriptAnalyzerFindingsAverage  
The average number of best practice violations found by [PSScriptAnalyzer](https://github.com/PowerShell/PSScriptAnalyzer) per function.  

#### Default thresholds :  
  - Warning : greater than 7  
  - Fail : greater than 12  

### FunctionsWithoutHelp  
The total number of the function which do not contain any comment-based help.  

It is an important best practice to have help information for each function, even private function (for contributors, or code reviewers).  
At the very least, it should have a short statement describing the function's purpose. That is the **Synopsis**.  
There is no compliance rule for this metric because it is highly dependent on the total number of functions, and arguably, on the number of public functions.  

### NumberOfTests  
The total number of tests found by Pester.  
Keep in mind that this metric is purely quantitative : it does not indicate anything about the **quality** or the **coverage** of the tests. This is why there is no compliance rule associated with this metric.

### NumberOfFailedTests  
The total number of failed tests. This is using `Invoke-Pester` with the `Strict`parameter, this means that any test marked as skipped, pending, or inconclusive is considered as failed.  
Lower is better, 0 is highly recommended because any failing test indicates that the code doesn't behave as intended (or at least, as intended when the test(s) were written).  
This means there is most likely a defect either in the code or in the test(s). This type of problems should be prioritized over any other type of code quality issues.  

#### Default thresholds :  
  - Warning : greater than 1  
  - Fail : greater than 3  
           
### NumberOfPassedTests  
The total number of passed tests.  
This metric is purely quantitative and highly dependent on the total number of tests : it does not indicate anything about the **quality**, **effectiveness** or **coverage** of the tests. This is why there is no compliance rule associated with this metric.  

### TestsPassRate  
The overall percentage of passing tests.  
The higher the better, because it gives an indication of the quality of the code being tested. More specifically, it answers the question : **"To which extent the code behaves as intended ?"**, assuming the intent is properly reflected in the tests.  

#### Default thresholds :  
  - Warning : less than 99  
  - Fail : less than 97  

### TestCoverage  
The overall percentage of code which is exercised across all PowerShell files by the tests.  
Note : the code **executed** during tests cannot always be considered as **tested**. It is only **tested** if it is executed **and** there is one or more test(s) specifically designed to validate that the code behaves as expected.  

Higher is better. Good tests provide confidence that the code behaves as expected. How confident depends mostly on the percentage of code which is covered by the tests. This is why code coverage of the tests is an important metric. It is an indicator of how thoroughly the different possible code paths and edge cases are tested.  

#### Default thresholds : ([Source](https://github.com/PowerShell/DscResources/blob/master/HighQualityModuleGuidelines.md))  
  - Warning : less than 80  
  - Fail : less than 70  

### CommandsMissedTotal  
The total number of commands which are not exercized by the tests.  
This is the number of commands, not lines, because the code coverage feature of **Pester** uses breakpoints, which can only be triggered by commands ([Source](https://github.com/pester/Pester/wiki/Code-Coverage)).  

Lower is better. Any command not exercized by tests is untested, which means any defect it may contain will not be detected. Or at best, it will only be detected at later stages of the code lifecycle (user acceptance testing, QA, or in production).  
The later a defect is detected, the more expensive (in time and money) it is to fix.  

#### Default thresholds :   
  - Warning : greater than 200  
  - Fail : greater than 400  

### ComplexityAverage  
This is the average cyclomatic complexity per function. Cyclomatic complexity measures the number of possible execution paths through a given section of code. This is intended to evaluate code complexity.  

Lower is better, because complex code tends to have the following properties :  
  - Difficult to read/understand  
  - Difficult to test  
  - More prone to defects  
  - Make defects more difficult to identify
  - More difficult and risky to change/refactor/maintain  

#### Default thresholds : ([Source](http://www.ndepend.com/docs/code-metrics#CC))  
  - Warning : greater than 15  
  - Fail : greater than 30  

For more details on how the cyclomatic complexity is calculated, please refer to [this article](http://theshellnut.com/measuring-powershell-code-complexity-why-and-how/).

### ComplexityHighest  
This is the cyclomatic complexity of the function which has the highest complexity value in the health report. Cyclomatic complexity measures the number of possible execution paths through a given section of code. This is intended to evaluate code complexity.  

Lower is better, because complex code tends to have the following properties :  
  - Difficult to read/understand  
  - Difficult to test  
  - More prone to defects  
  - Make defects more difficult to identify
  - More difficult and risky to change/refactor/maintain  

#### Default thresholds : the double of the average ([Source](http://www.ndepend.com/docs/code-metrics#CC))  
  - Warning : greater than 30  
  - Fail : greater than 60  

### NestingDepthAverage  
This is the average of **MaximumNestingDepth** per function.  
MaximumNestingDepth is the depth of the most deeply nested code in a given function. This measures a different aspect of complexity from the "cyclomatic complexity", so these 2 metrics are complementary. The nesting depth of a piece of code is an indication of the complexity of its context.  

Again, lower is better.  

#### Default thresholds : ([Source](http://www.ndepend.com/docs/code-metrics#ILNestingDepth))  
  - Warning : greater than 4  
  - Fail : greater than 8  

For more details on how the maximum nesting depth is calculated, please refer to [this article](http://theshellnut.com/measuring-powershell-code-complexity-why-and-how/).  

### NestingDepthHighest  
This is the **MaximumNestingDepth** of the function which has the highest value in the health report.  
MaximumNestingDepth is the depth of the most deeply nested code in a given function. This measures a different aspect of complexity from the "cyclomatic complexity", so these 2 metrics are complementary. The nesting depth of a piece of code is an indication of the complexity of its context.  

Again, lower is better.  

#### Default thresholds : the double of the average ([Source](http://www.ndepend.com/docs/code-metrics#ILNestingDepth))  
  - Warning : greater than 8  
  - Fail : greater than 16  
