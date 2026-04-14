## <center> Functional and Non Functional Requirements </center>

- `Functional Requirements:` Define what the system should do (features and operations).
- `Non-Functional Requirements:` Define how the system should perform (quality, performance, and constraints).

![req](https://media.geeksforgeeks.org/wp-content/uploads/20250822132836297050/requirements.webp)

---
### 1. Functional Requirements
Functional requirements define the specific features and operations a system must perform to meet business and user needs. They describe what the system should do and how it interacts with users or other systems.
1. Focus on the behavior and functionality of the system.
2. Represent features that can be directly observed and tested in the final product. 
3. Examples include user authentication, data processing, search functionality, payment processing, and report generation.

### 2. Non-Functional Requirements
Non-Functional requirements (NFRs) define how a system should operate, focusing on performance, reliability, and user experience rather than specific features. They ensure the system is efficient, secure and maintainable over time. 

```ini
Performance: speed and responsiveness
Security: protection against unauthorized access
Usability: ease of use
Reliability: system stability and availability
Scalability: ability to handle growth
Maintainability: ease of updates and fixes
Portability: ability to run in different environments
```

### 3. Extended Requirements
Extended requirements define additional capabilities or considerations that enhance the system but are not part of the core functional features. These requirements help improve monitoring, reliablitity, and future expansion of the system. 

```ini
Logging: recording system activities and errors for debugging and analysis
Monitoring & Alerting: tracking system health, performance, and failures
Analytics: collecting usage data to understand user behavior and system performance
Backup & Disaster Recovery: ensuring data can be restored in case of failures
Rate Limiting: controlling the number of requests to prevent system overload or abuse
Feature Flags / A-B Testing: enabling controlled feature releases and experiments
```
---

<table><thead><tr><th style="text-align: center;"><b><strong>Functional Requirements</strong></b></th><th style="text-align: center;"><b><strong>Non-Functional Requirements</strong></b></th></tr></thead><tbody><tr><td style="text-align: center;"><span>Define what the system should do (features &amp; functionality)</span></td><td style="text-align: center;"><span>Define how the system should perform (quality attributes)</span></td></tr><tr><td style="text-align: center;"><span>Focus on system behavior and operations</span></td><td style="text-align: center;"><span>Focus on performance, usability, security, reliability</span></td></tr><tr><td style="text-align: center;"><span>Describe specific actions like login, data processing, transactions</span></td><td style="text-align: center;"><span>Describe constraints like response time, scalability, availability</span></td></tr><tr><td style="text-align: center;"><span>Directly visible to users and business needs</span></td><td style="text-align: center;"><span>Indirectly visible, improve overall user experience</span></td></tr><tr><td style="text-align: center;"><span>Easier to measure (output-based validation)</span></td><td style="text-align: center;"><span>Harder to measure, uses metrics like SLAs, benchmarks</span></td></tr><tr><td style="text-align: center;"><span>Drive the core functionality of the system</span></td><td style="text-align: center;"><span>Influence architecture and system design decisions</span></td></tr><tr><td style="text-align: center;"><span>Documented using use cases, user stories</span></td><td style="text-align: center;"><span>Documented using technical specs, performance criteria</span></td></tr><tr><td style="text-align: center;"><b><strong>Examples:</strong></b><span> login authentication, data input/output, transaction processing.</span></td><td style="text-align: center;"><b><strong>Examples:</strong></b><span> scalability, security, response time, reliability, maintainability.</span></td></tr></tbody></table>

---
## Common Challenges in Defining these Requirements

Defining system requirements can be challenging because they must balance functionality, performance, and long-term system goals. Poorly defined requirements can lead to design issues, delays, or systems that fail to meet user expectations.

* **Ambiguity in Requirements:** Vague or incomplete requirements make it difficult to clearly define what the system should do and how it should perform.
* **Changing Requirements:** Business goals, market conditions, or user expectations may change over time, requiring updates to the system design.
* **Difficulty in Prioritization:** Functional requirements often receive more attention, while important aspects like scalability, security, or monitoring may be overlooked.
* **Measuring Non-Functional Requirements:** Features are easier to test, but qualities like usability, scalability, and reliability are harder to measure and validate.
* **Overlapping or Conflicting Requirements:** Some requirements may conflict with others, such as strong security measures potentially affecting system performance.

---