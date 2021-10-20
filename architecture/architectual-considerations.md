# Architectual Considerations

The following is notes taken from watching Architectural session from "[Uncle Bob" Martin](https://www.youtube.com/results?search\_query=uncle+bob)

### Take away's:

* Make sure you have a ubiquitous language about architectural characteristics(Reliability, Scalability, performance, etc) so that people can have a common dictionary to ensure people do not talk past each other
* KISS - How-To -- Don't try to meet all the "illities" - because as you add implementation for ilities, you add complexity



### Business Requirements = Domain&#x20;

Non-functional Requirements = Cross-Feature Requirements = Architecture Characteristics

* Reliability
* Scalability
* Performance
* Availability

![Architectural Triad](<../.gitbook/assets/image (8).png>)

### Operational Architecture Characteristics

* Overlaps between distributed architecture & operational concerns
*   Metrics:

    &#x20; ○ Cumulative average response time - trigger alert when the average response times within a particular context exeeds 1400 ms)

    &#x20; ○ Maximum Response Time - Trigger alert anytime max response time of selected requests exceeds 2000ms

    &#x20; ○ Cumulative averge response time - trigger alert if response times continues to increase over 2 month period

### Quality Arch Characteristics

* Fault Tolerance
*   Reliability

    &#x20; ○ Considered a composite characteristic - too braod to have a concrete objective. But it is the sum of a bunch of things that can be measured

    &#x20; ○ Availability - using the nines chart

### Process Arch Characteristics

*   Agility - another composite characteristics

    &#x20; ○ Deployability -

    ```
      § Measure  & track the number of actual hours spent to deploy
      § Measure & track failed deployments or errors resulting from deployment
      § Measure & track frequency of deployment
    ```

    &#x20; ○ Testability

    &#x20; ○ Modularity

### Component coupling

* Afferent coupling - incoming - the degree to which other components are dependent on the target component&#x20;
* Efferent coupling - outgoing - the degree to which the target component is dependent on other components

### Coupling design



### Uncle Bob's metric&#x20;

* Too much abstractness & instability - zone of uselessness&#x20;
* Too little abstraction & instability - zone of pain

![](<../.gitbook/assets/image (10).png>)



### Partitioning Services

* Technical partitioning
  * Presentation, business rules, persistent are each layers
* Domain paritioning
  * UI, module 1, module 2, …

### Why Quantum?

* Operational view of architecture
* Holistic view of architecture
  * Database
  * User interface
* Useful for structural analysis

![](<../.gitbook/assets/image (12).png>)



### Consumer Driven Contracts:

* Martinfowler.com/articles/consumerDrivenContracts.html
* Pact
* Choas Testing:
  * Conformity Monkey - Used to ensure that charateristics are mapped to what they ned to me
  * Security monkey - ensuring you don't have any security characteristics
* ArchUnit -&#x20;
  * [https://archunit.org](https://archunit.org)
  * [https://Github.com/benmorris/netarchtest](https://github.com/benmorris/netarchtest)

![](<../.gitbook/assets/image (13).png>)

![](<../.gitbook/assets/image (14).png>)
