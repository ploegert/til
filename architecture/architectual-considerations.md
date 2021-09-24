# Architectual Considerations

The following is notes taken from watching Architectural session from "[Uncle Bob" Martin](https://www.youtube.com/results?search_query=uncle+bob)

### Take away's:

* Make sure you have a ubiquitous language about architectural characteristics\(Reliability, Scalability, performance, etc\) so that people can have a common dictionary to ensure people do not talk past each other
* KISS - How-To -- Don't try to meet all the "illities" - because as you add implementation for ilities, you add complexity



### Business Requirements = Domain 

Non-functional Requirements = Cross-Feature Requirements = Architecture Characteristics

* Reliability
* Scalability
* Performance
* Availability

![Architectural Triad](../.gitbook/assets/image%20%289%29.png)

### Operational Architecture Characteristics

* Overlaps between distributed architecture & operational concerns
* Metrics:

    ○ Cumulative average response time - trigger alert when the average response times within a particular context exeeds 1400 ms\)

    ○ Maximum Response Time - Trigger alert anytime max response time of selected requests exceeds 2000ms

    ○ Cumulative averge response time - trigger alert if response times continues to increase over 2 month period

### Quality Arch Characteristics

* Fault Tolerance
* Reliability

    ○ Considered a composite characteristic - too braod to have a concrete objective. But it is the sum of a bunch of things that can be measured

    ○ Availability - using the nines chart

### Process Arch Characteristics

* Agility - another composite characteristics

    ○ Deployability -

  ```text
    § Measure  & track the number of actual hours spent to deploy
    § Measure & track failed deployments or errors resulting from deployment
    § Measure & track frequency of deployment
  ```

    ○ Testability

    ○ Modularity

### Component coupling

* Afferent coupling - incoming - the degree to which other components are dependent on the target component 
* Efferent coupling - outgoing - the degree to which the target component is dependent on other components

### Coupling design



### Uncle Bob's metric 

* Too much abstractness & instability - zone of uselessness 
* Too little abstraction & instability - zone of pain

![](../.gitbook/assets/image%20%288%29.png)



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

![](../.gitbook/assets/image%20%287%29.png)



### Consumer Driven Contracts:

* Martinfowler.com/articles/consumerDrivenContracts.html
* Pact
* Choas Testing:
  * Conformity Monkey - Used to ensure that charateristics are mapped to what they ned to me
  * Security monkey - ensuring you don't have any security characteristics
* ArchUnit - 
  * [https://archunit.org](https://archunit.org)
  * [https://Github.com/benmorris/netarchtest](https://Github.com/benmorris/netarchtest)

![](../.gitbook/assets/image%20%282%29.png)

![](../.gitbook/assets/image%20%285%29.png)

