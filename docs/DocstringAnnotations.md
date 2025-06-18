
Docstring annotations
=====================

The annotations can be found in the jar with reference `io.codelaser:codelaser-metrics-support:1.0.0`.

The following is a sample of a `package-info.java` file containing docstrings for the 3 types of some package:

```java

@Docstrings({
        @Docstring(type = Main.class,
                summary = "The entry point class for a Java inspection application that initializes and executes Java code analysis functionality. It appears to coordinate the inspection of Java elements like fields, types, and expressions.",
                tags = {"application", "code-analysis", "coordinator", "entry-point", "java-inspection", "viewer"},
                embedding = "-0.0077609136f, -0.006571619f, -0.14944068f, -0.0835727f, 0.08013757f, -0.03646823f, 0.1008635f, -0.0059653763f, 0.008750587f, -0.032723054f, 0.030002179f, 0.030303288f, 0.06408068f, 0.0063456916f, 0.051549207f, 0.015563211f, 0.012759667f, -0.010462954f, -0.030081317f, -0.04666882f, -0.047956407f, -0.014383931f"),
        @Docstring(type = Source.class,
                summary = "A record that represents a source code entity with its name, content, associated tags, and processing result. It serves as a data container for handling and displaying source code in a viewer component.",
                tags = {"container", "data", "immutable", "metadata", "record", "source code", "viewer"},
                embedding = "-0.015866391f, 0.021556074f, -0.15672955f, -0.100543186f, 0.049120422f, -0.07445214f, -0.003645494f, 0.04237684f, -0.067126915f, -0.028673176f, 0.010898684f, 0.03525499f, 0.11260703f, 0.029673f, 0.028562516f, -0.07312919f, -0.025579944f, -0.06401586f, -0.022726564f, 0.018133352f, 0.026597371f, 0.00775433f, 0.013837435f, -0.0144621f, -0.079256736f, -0.032256912f, -0.045063257f, 0.006669314f"),
        @Docstring(type = TableRow.class,
                summary = "A data record representing a row in a table display, encapsulating information about an operation including its source name, step, operation ID, source number, and associated edit. This record likely serves as a view model for displaying operational data in a tabular format.",
                tags = {"data", "immutable", "operation", "record", "tabular", "view-model"},
                embedding = "{-0.021687685f, 0.052172694f, -0.18998192f, -0.055679787f, 0.022924492f, -0.08119153f, 0.050168615f, 0.010357551f, -0.0021513468f, -0.008401546f, -3.7312097E-4f, 0.029774306f, 0.1185465f, -0.021423457f")
})

package io.codelaser.jfocus.stdbase.viewer;

import io.codelaser.jfocus.metrics.support.Docstrings;
import io.codelaser.jfocus.metrics.support.Docstring;

```