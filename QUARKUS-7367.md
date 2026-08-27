# QUARKUS-7367 Implement Quantum-Safe TLS and mTLS Support in Quarkus

JIRA: https://issues.redhat.com/browse/QUARKUS-7367

Implement Quantum-Safe TLS and mTLS Support in Quarkus (Phase 1, Netty stack)

Upstream PRs:
- https://github.com/quarkusio/quarkus/pull/55629

Documentation:
- https://quarkus.io/guides/tls-registry-reference/#post-quantum-cryptography-pqc


## Scope of testing
The QUARKUS-7367 scope is to add hybrid key exchange.
This enables the use of post-quantum cryptography (PQC) mechanism to be used alongside with the traditional algorithms.
For users, this means they don't need to update their certificates, keystores and truststores and still use PQC for establishing a connection between server and client.

The Quarkus implements 3 enforcement policies:
* Strict, which enforces the PQC exchange, and client requests that do not offer PQC exchange are rejected.
* Client-negotiated, this mode allows for both (PQC and standard exchange) to be established, but the PQC one is preferred.
* Relaxed, does not offer PQC, only the standard exchange.

Currently, there is an option to set `quarkus.tls.ssl-engine` to either `openssl` or `jdkssl`.
The tests implementation will cover only `openssl` variant as the `jdkssl` using implementation of provided JDK.
The JDK which support PQC at the moment is JDK 27.
There is a plan to backport the PQC to older JDKs, but at the moment it's not done yet.
There is also a plan to have it in Graal and Mandrel, when these changes are backported to JDK 25.
Currently, planned support for this feature is planned for JVM mode only on `Linux x86_64` and `Linux aarch64`.
Both Linux platforms, when using the `openssl` engine, need to have SSL library in version 3.5+.
In case the of RHEL platforms, it's RHEL 10+.
There is a possibility to extend the coverage, if needed, when the PQC is backported to an older JDK version, so the tests will use `jdkssl` instead of `openssl` on RHEL 9 (coverage from QE is not planned in the current scope), this will be a topic for Quarkus 4.


Connection to databases with JDBC drivers won't be covered, as JDBC drivers do not use Netty stack and some of them are not fully ready for PQC.
This is also mentioned in QUARKUS-7367, that it will be part of phase 2.

The connection to different services (Keycloak and others) won't be also covered at the moment as for each service there would be a need for verification if the service supports PQC.
These connections are using the Quarkus TLS registry with a specific "bucket".
As the Quarkus TLS registry will be tested, we can trust that the PQC connection to services will also work, if the service supports it.


## Automated test development

Upstream coverage:
* The upstream PR adds multiple unit tests that cover basic functionality of this feature.
* There are no integration tests.

The tests in the Quarkus QE Test Suite will be implemented in the newly created module `security/pqc`.

The tests will cover various combinations of `quarkus.tls.pqc-enforcement-policy` and `quarkus.tls.key-exchange-groups` on the server side and client offering different exchange groups.
These tests will be cover both positive and negative scenarios.
For example, both server and client allow `X25519MLKEM768` so the connection is established, or the client supports only `x25519` and the server set a `strict` PQC policy enforcement, which results in the client failing to establish the connection.

As the implementation was for Quarkus TLS registry, the tests will cover the default `quarkus.tls` and "bucket" `quarkus.tls` settings of the TLS registry. 

There will be one test to verify proper logging/failing to start the application when the SSL library has a version lower than 3.5.
These tests will use `@DisabledIf`/`@EnabledIf` with the check for Open SSL version.
This will be executed on all platforms.

These tests are planned for OpenShift and will use `registry.access.redhat.com/ubi10/openjdk-25:latest`.
The OpenShift coverage at the moment of writing this TP has some problems as the Quarkus throwing `PQC enforcement policy STRICT requires X25519MLKEM768 but the configured SSL engine does not support it`.
The UBI 10 image has SSL library 3.5+, so there should not be any problem.
An issue for this will be created with the need to enhance the documentation, as the same simple application which work on bare-metal is not starting on OpenShift.

### Impact on TS and resources
The tests will be executed on bare-metal in JVM.
The expected time increase will be around 10 minutes, but as the native is not covered, the build and restarts of Quarkus application will be fast.

The tests will also run on OpenShift, as there is issue on OpenShift it hard to estimate impact on TS and resources.
As OpenShift tests will be subset of all tests to cover most common use cases, the rough estimate is around 15 minutes

## Getting familiar with the feature

Following actions were taken to ensure familiarity:

* Reviewed upstream pull request.
* Reviewed documentation around PQC and TLS registry
* Read the rfc9954 (https://datatracker.ietf.org/doc/rfc9954/)

## Contacts
- Tester: Jakub Jedlička <jjedlick@ibm.com>
