
  API Testing is the subset of testing approaches focused on the API contract. In the case of API-first services, functional and regression tests will be conducted primarily via APIs, making robust and well maintained API tests a critical enabler of agile development and continuous delivery.API testing is described in various different ways depending on where you look. This overview is not an attempt to be dogmatic, but to lay out broadly applicable principles. The terms for, and granularity of different testing phases may be described differently across various test tooling, and the mapping to environments will differ from site to site.

##This high-level overview will consider the following topics:

1.API Test Evolution
2.Phases of API Testing
3.Testing Approaches
4.Security Testing
5.Test Tooling Supporting OpenAPI Test Generation
6.API Test Evolution

API testing platforms will typically enable an interface (or contract) test collection to be generated from an OpenAPI document. A test collection will grow and evolve through different phases of development. It is important to refine tests and maintain collections to facilitate this evolution.


A test suite may be grouped into several collections to allow ‘modular’ composition into the required test type/granularity/coverage. Test collections may potentially be run in parallel to reduce test execution time.

API Testing Phases
Note: In order to simplify concepts, testing phases and environments are deliberately conflated here. However, while testing requirements in higher environments may become increasingly stringent, more mature tests such as security and negative tests (when available) may be run through all environments.

1. API Specification Development & Integration
API document linting: An API definition document rules and standards compliance validation— comparable to code analysis for the API document. Linting against API Documentation Rules should be incorporated into IDE tools & continuous integration pipelines.

2. Sandpit / Early Dev

Interface tests: Test defined REST operations with basic scenarios. The back-end MAY be mocked to allow early exposure of APIs to client developers in order to surface issues and elicit feedback. Interface tests may be generated from an OpenAPI document.

API Unit tests: An interface test concerned with exercising the modules and components that implement the API. Requests are validated and correctly handled, responses are schema valid.

3. Development

Integration tests: Verify that different modules or services work well together. For example, interactions with databases, IAM/ACLs or microservices. May be rolled into functional testing.

Functional tests: Positive/happy-path testing returns valid business responses. Functional tests would expect to GET specific values per POSTed inputs. Collection filters return expected records.

Resource lifecycle tests: Replicates user/system interaction with one or more specific business entities through the entire E2E resource state-lifecycle from instantiation to retirement, and verifies that state transitions triggered by API operations work as expected.

4. Test

Regression tests: Tests combinations of API operations to ensure every function behaves as expected after an update. It will accumulate tests for bugs and issues. A consolidation of functional and end-to-end testing, with an additional focus on issues and edge-cases.

Negative tests: Negative testing ensures that your application can gracefully handle invalid input or unexpected user behavior. Write tests expecting specific error responses. Explore edge cases and field limits to increase the resilience of your APIs in production.

Security tests: Policy-as-Code controls and security scheme validation is applied during deployment. Automated testing of applicable security controls is performed maintained by the product team either adjunct to, or incorporated into regression tests.

5. Acceptance / Staging

Performance testing: Performance tests evaluate how an API performs under high workloads, measuring the reliability, speed, scalability, and responsiveness of an application. It will validate caching, assess performance against SLAs, and help identify bottlenecks and stress points.

Acceptance testing: Formal, integrated tests that are sometimes employed as a production release stage-gate. Acceptance tests focus on replicating user behaviors and external processes to verify if a system satisfies business requirements.

API penetration testing: API security testing targeting known risks and vulnerabilities (incl. OWASP top 10) is undertaken regularly in acceptance and periodically in production, and is regularly reviewed and maintained by the API platform and Cyber Security teams.

Testing Approaches
API Validation / Linting
APIs will typically be validated for standards, policy and security compliance during deployment by automation. Linting of the OpenAPI document during Continuous Integration is recommended to ensure timely feedback on format and configuration issues.

Spectral OpenAPI rules are widely adopted as a base ruleset, and may be extended to enforce enterprise patterns and conventions. OpenAPI validation tools should be made available to product teams for incorporation into IDE tools & continuous integration pipelines.

Test Code Coverage Analysis
As the service comes together, and if coverage instrumentation is supported, test/code coverage analysis may be applied and reported by continuous integration pipelines. Coverage analysis will help ensure untested functions are not released to production.

Ad-hoc API Testing
Test collections generated from your OpenAPI spec may be used to quickly and easily interact with APIs under development. Individual API requests or entire collections of requests can be run against the API. Scripts may be employed to insert response data into subsequent requests.

Testing Against Mock APIs
Mock APIs are used to simulate a working API — generating schema conformant API responses. This tactic may be used to unblock client developers, and/or to expose the REST model/APIs to client developers early to surface issues and elicit feedback.

A mock API service may various levels of sophistication depending on the purpose:

Stub: A placeholder to backend interface testing usually returning static or randomly generated data.
Mock: basic functionality required for a specific testing or development purpose — should create and return instance data.
Virtual Service: complete and persistent state-lifecycle emulation deployed into system integration environments.
An API Gateway may offer out-of-the-box service mocking. Mock servers may also be generated from an OpenAPI specification.

Automated Testing
Once an API delivery pipeline has been configured, promotion through test environments to the Stage/User-Acceptance environment may be automated on the basis of full-coverage, automated functional (dev), regression (test) and security testing. Coverage and maintenance of functional, regression and security tests is a product team responsibility.

Security Testing
Verifying the application of security controls applicable to your business API is a product team responsibility, and will include both code analysis and automated testing.

As well as automated Sonarqube analysis of your software source code, continuous integration and delivery pipelines can validate API security and API policy configuration.

Automated security testing should be built into delivery pipelines and may be a separate suite of tests or consolidated into regression tests. Security tests should target security controls directly applicable to your API, as well as general and high-risk API vulnerabilities.

Test coverage might include (among other things):
HTTPS scheme only, no redirect from HTTP to HTTPS.
API gateway returns a HTTP status code of “400 — Bad Request “ for schema invalid JSON content
Operation-level client authorization is applied. The API gateway returns a HTTP status code of “401 — Unauthorized” to client applications that are not subscribed/authorized
Granular user/role level authorization is applied to individual records (where applicable), and “401 — Unauthorized” is returned to an unauthorized user.
A ‘read’ authorization does not provide access to ‘Unsafe’ (e.g. ‘write’ protected) HTTP methods
Vectors for command Injection are neutralized
Rate limit is applied and API gateway returns a HTTP status code of “429 — Too Many Requests” (may be covered by performance testing)
Pagination is applied, and payload sizes are within predetermined size limits.
Field size validation is applied. The API gateway returns an HTTP status code of “400 — Bad Request” when size limits are exceeded.
API authorization event logs for all touchpoints are captured by SIEM.
Consider vulnerabilities articulated in the OWASP API Security Top 10

Look for security testing guidance specific to the testing platform used by your team/org, such as the following Postman/Newman security testing guidance:

Leveraging postman for security testing
Integrating API security testing into postman lifecycle
API Penetration Testing
API security testing targeting known risks and vulnerabilities (incl. OWASP API Security Top 10) is undertaken regularly in stage/acceptance and periodically in production. It is usually overseen by Cyber Security in consultation with the API platform team, and routinely reviewed and maintained.
