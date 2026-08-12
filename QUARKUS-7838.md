# QUARKUS-7838 - Broad Camel Extension Catalog Inclusion

JIRA: https://redhat.atlassian.net/browse/QUARKUS-7838

## Feature Summary

Apache Camel Quarkus ports and packages 300+ Apache Camel components as Quarkus extensions, providing a comprehensive
cloud-native integration framework. This RFE covers the addition of Camel Quarkus extensions as platform members to IBM
Build of Quarkus (IBQ) product platform.

**Important distinction**: Camel Quarkus extensions will be part of the IBQ product platform but will NOT be supported
members of the Red Hat Build of Quarkus (RHBQ) platform. Supported Camel Quarkus extensions are provided on the Red Hat side 
under different subscription than RHBQ.

## Scope of Testing
Supported Camel Quarkus extensions are provided on the Red Hat side already for several years.
The primary primary focus of the testing will be around integration into IBQ product platform.

### In Scope
- Platform member BOM availability
- Maven artifacts availability in IBQ maven repository
- Platform metadata validation via Marete testsuite
- Productized extension discovery in IBQ platform
- Community extension discovery in RHBQ platform

### Out of Scope
Camel Quarkus team covers:
- Marete configuration preparation
- Comprehensive functional testing
- Full compatibility matrix testing
- Native mode testing
- Performance benchmarking
- Production workload validation
- Documentation validation
- Security-specific testing beyond standard platform checks

## Planned Coverage

Marete testsuite is the primary target for the test development. StartStop testsuite will be extended too.

### Coverage details
Existing Marete testsuite coverage: 
- Platform BOM availability
- Maven artifacts availability

New Marete testsuite coverage:
- Platform metadata validation for IBQ
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
- Feature JIRA: [QUARKUS-7838](https://redhat.atlassian.net/browse/QUARKUS-7838)
- Apache Camel Quarkus GitHub: https://github.com/apache/camel-quarkus
- Apache Camel Website: https://camel.apache.org
