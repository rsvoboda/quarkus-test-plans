# QUARKUS-7353 - Add Quarkus Flow as IBQ Platform Member

JIRA: https://redhat.atlassian.net/browse/QUARKUS-7353

## Feature Summary

Quarkus Flow is a lightweight, production-grade workflow engine for Quarkus applications.
This RFE covers the addition of Quarkus Flow as a platform member into IBM Build of Quarkus (IBQ) product platform.

**Important distinction**: Quarkus Flow will be part of the IBQ product platform but will NOT be a supported member of the RHBQ (Red Hat Build of Quarkus) platform.

## Scope of Testing
The primary role of the RHBQ QE team is to help the Quarkus Flow team with integration into existing product testing flows.

### In Scope
- Platform member BOM availability and correctness for IBQ
- Maven artifacts availability in IBQ maven repository
- Platform metadata validation via Marete testsuite
- Productized extension discovery in IBQ platform
- Community extension discovery in RHBQ platform

### Out of Scope
Quarkus Flow team covers:
- Marete configuration preparation for quarkus flow
- Comprehensive functional testing of workflow features
- Full compatibility matrix testing
- Native mode testing
- Performance benchmarking
- Production workload validation
- Documentation validation
- Security-specific testing beyond standard platform checks

## Planned Coverage

Marete testsuite is the primary target for the test development. StartStop testsuite will be extended too.  

### Coverage details
Marete testsuite coverage:
- Platform member BOM availability and correctness for IBQ
  - Availability of Quarkus Flow BOM with product `artifactId`
- Maven artifacts availability in IBQ maven repository
  - `io.quarkiverse.flow:*` artifacts available in the repo zip
- Platform metadata validation
  - Offering metadata checks
  - Support level metadata checks

StartStop testsuite coverage:
- Productized extension discovery in IBQ platform
  - `ArtifactGeneratorIbmTest` will be extended
- Community extension discovery in RHBQ platform
  - `ArtifactGeneratorRedHatTest` will be extended

### Impact on test suites and testing automation
- Marete and StartStop testsuites will be extended
- No adjustments of testing automation are expected 

### Impact on testing resources
- No need for additional testing resources
- Testing execution will be extended by 5-10 minutes 

## Contacts
- Tester: Rostislav Svoboda <rsvoboda@ibm.com>

## References
- Feature JIRA: [QUARKUS-7353](https://redhat.atlassian.net/browse/QUARKUS-7353)
- Quarkus Flow GitHub: https://github.com/quarkiverse/quarkus-flow
- Quarkus Flow Documentation: https://docs.quarkiverse.io/quarkus-flow/dev/
- Open Workflow Specification: https://github.com/cncf/wg-serverless-workflow
