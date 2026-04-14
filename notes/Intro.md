# <center> Introduction</center>

System design is the process of planning and structing the architecture of a software system based on user requirements. It defines how different components of the system will work together to achieve the desired functinality efficiently.
* Translates user requirements into a clear techical blueprint for developers.
* Defines system components, data flow, and interactions between services.
* The goal is to create a well-organized and efficient structure that meets the intended purpose while considering factors like scalability, maintainability and performance.


**Two kinds of interviews, both expected at staff:**

|           | HLD (High-Level Design)                      | LLD (Low-Level Design)                            |
| --------- | -------------------------------------------- | ------------------------------------------------- |
| Scope     | Distributed system, whole product            | One service, one module                           |
| Focus     | Scale, data flow, storage choice, trade-offs | Classes, methods, patterns, APIs                  |
| Example   | "Design Twitter"                             | "Design a parking lot / rate limiter / LRU cache" |
| Output    | Boxes-and-arrows diagram + numbers           | Class diagram + code stubs                        |
| Key skill | Trade-off articulation                       | Clean OOP, SOLID, patterns                        |