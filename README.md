
Code metrics plugin
-------------------

Plugin page: [http://artifacts.griffon-framework.org/plugin/gmetrics](http://artifacts.griffon-framework.org/plugin/gmetrics)


The GMetrics Plugin provides provides calculation and reporting of size and
complexity metrics for Groovy source code. It uses the [GMetrics][1] library.
It began as a port of the [Grails GMetrics][2] plugin created by Scott Ryan.

Usage
----_
The plugin provides a script 'gmetrics' that will analyze your code and write
its results to an HTML file. Run the following command

    griffon gmetrics

to perform the analysis.

Configuration
-------------
The plugin requires no customization to run. By default it will analyze all Groovy files in

 * src/main
 * griffon-app/controllers
 * griffon-app/models
 * griffon-app/views
 * griffon-app/services

You can restrict which folders are included or add extra ones. The following
table lists settings that will be read from `griffon-app/conf/BuildConfig.groovy`
and used if available:

| *Property*                      | *Default Value*                           | *Meaning*                                            |
| ------------------------------- | ----------------------------------------- | ---------------------------------------------------- |
| gmetrics.reportName             | 'gmetrics.html'                           | the name of the analysis report file                 |
| gmetrics.reportLocation         | ${projectTargetDir}/test-reports/gmetrics | the location of the analysis report file             |
| gmetrics.reportType             | 'html'                                    | the report type; the only valid value is 'html'      |
| gmetrics.reportTitle            | "GMetrics - $griffonAppName"              | the report title                                     |
| gmetrics.processSrcGroovy       | true                                      | whether to analyze source in src/main/*.groovy       |
| gmetrics.processControllers     | true                                      | whether to analyze source in griffon-app/controllers |
| gmetrics.processModels          | true                                      | whether to analyze source in griffon-app/models      |
| gmetrics.processViews           | true                                      | whether to analyze source in griffon-app/views       |
| gmetrics.processServices        | true                                      | whether to analyze source in griffon-app/services    |
| gmetrics.processTestUnit        | true                                      | whether to analyze source in test/unit               |
| gmetrics.processTestIntegration | true                                      | whether to analyze source in test/integration        |
| gmetrics.extraIncludeDirs       | none                                      | extra source directories to include                  |
| gmetrics.metricSetFile          | none                                      | additional metrics to run on the source              |

### C.R.A.P. Metrics

[C.R.A.P.][3] stands for `Change Risk Anti-Patterns`. You can enable this metric
by defining a value for `gmetrics.metricSetFile`. Follow these steps to get basic
C.R.A.P. metrics on your application.

1. Create a new file that will hold the metric definitions, for example `crap.gmetrics`.
2. Paste the following code into the newly create file

    import org.gmetrics.metric.cyclomatic.CyclomaticComplexityMetric
    final COBERTURA_FILE = 'file:target/test-reports/cobertura/coverage.xml'
    metricset {
        def cyclomaticComplexityMetric = metric(CyclomaticComplexityMetric)
        def coberturaMetric = CoberturaLineCoverage {
            coberturaFile = COBERTURA_FILE
            functions = ['total']
        }
        CRAP {
            functions = ['total']
            coverageMetric = coberturaMetric
            complexityMetric = cyclomaticComplexityMetric
        }
    }

3. Add the file to the gmetrics configuration block in `BuildConfing.groovy`
   (pay attention to the relative path)

    gmtetrics.metricSetFile = 'file:crap.gmetrics'

4. Install the [code-coverage][4] plugin.
5. Run tests with coverage enabled

    griffon test-app -coverage

6. Run gmetrics after coverage has been stored

    griffon gmetrics

[1]: http://gmetrics.sourceforge.net
[2]: http://grails.org/plugin/gmetrics
[3]: http://googletesting.blogspot.com/2011/02/this-code-is-crap.html
[4]: /plugin/code-coverage

