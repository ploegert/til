---
coverY: 0
---

# Architectural-"ilities"

What is an "Ility" you might ask? Well, there are all kinds of ilities in the context of software architecture. Here is a list of some of the most important and calls out some of the ways to measure them.

### Metrics joining to architecture characteristics

<table data-header-hidden><thead><tr><th width="279">Architectural Characteristics</th><th>'ilities</th></tr></thead><tbody><tr><td>Component</td><td><ul><li>Modularity</li><li>Maintainability</li><li>Testability</li><li>Availability</li><li>Deployment</li><li>Reliability</li><li>Scaleability</li><li>Evolutionary/migration-ability</li></ul><p></p></td></tr><tr><td>Complexity/WMC</td><td><ul><li>Maintainability</li><li>Testability</li><li>Reliability</li></ul><p></p></td></tr><tr><td>Coupling (CE, CA, CT)</td><td><ul><li>Modularity</li><li>Maintainability</li><li>Testability</li><li>Availability</li><li>Deployment</li><li>Reliability</li><li>Scalability</li><li>Evolutionary/Migration-ability</li></ul><p></p></td></tr><tr><td>Inheritance depth (DIT)</td><td><p></p><ul><li>Modularity</li><li>Maintainability</li><li>Testability</li><li>Deployment</li><li>Reliability</li><li>Evolutionary/Migration-ability</li></ul></td></tr><tr><td>Percent  comments</td><td><ul><li>Maintainability</li><li>Testability</li><li>Reliability</li></ul></td></tr></tbody></table>

### Operational Architecture

| Performance |   | <p></p><ul><li>Cumulative average response time - trigger alert when the average response times within a particular context exeeds 1400 ms)</li><li>Maximum Response Time - Trigger alert anytime max response time of selected requests exceeds 2000ms</li><li>Cumulative averge response time - trigger alert if response times continues to increase over 2 month period</li><li>Double Exponential Smoothing</li><li>Alert tuning Fitness function</li></ul> |
| ----------- | - | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Scalability |   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| Elasticity  |   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |

### Quality Architecture

| Reliability |  Fault Tolerance                   | <p></p><ul><li></li></ul>                                                                                                                                                                                                                                   |
| ----------- | ---------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Reliability |  Availability                      | <p> </p><ul><li><img src="../../.gitbook/assets/unknown (1).png" alt=""></li><li>Using the nines chart</li></ul>                                                                                                                                            |
| Agility     | <p>Deployability<br>Elasticity</p> | <ul><li>Measure  &#x26; track the number of actual hours spent to deploy</li><li>Measure &#x26; track failed deployments or errors resulting from deployment</li><li>Measure &#x26; track frequency of deployment</li><li>Measure errors/failures</li></ul> |
| Agility     | Modularity                         |                                                                                                                                                                                                                                                             |

### Internal Quality Architecture

|   | Component Coupling | <p>(tight -> loose coupling)</p><ul><li>Pathological coupling</li><li>External coupling</li><li>Control coupling</li><li>Data coupling</li></ul>                                 |
| - | ------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|   |  Abstractness      | Sum (abstract elements) / sum (concrete elements)                                                                                                                                |
|   |  Instability       | <ul><li>Efferent coupling / efferent coupling / afferent coupling</li><li>Distance from the main sequence</li><li><img src="../../.gitbook/assets/unknown.png" alt=""></li></ul> |
|   |                    |                                                                                                                                                                                  |
|   |                    |                                                                                                                                                                                  |









