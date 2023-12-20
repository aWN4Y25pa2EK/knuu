# knuu

[![CodeQL](https://github.com/celestiaorg/knuu/workflows/CodeQL/badge.svg)](https://github.com/celestiaorg/knuu/actions) [![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0) [![OpenSSF Best Practices](https://bestpractices.coreinfrastructure.org/projects/7475/badge)](https://bestpractices.coreinfrastructure.org/projects/7475)

## Description

The goal of knuu is to provide a framework for writing integration tests.
The framework is written in Go and is designed to be used in Go projects.
The idea is to provide a framework that uses the power of containers and Kubernetes without the test writer having to know the details of how to use them.

We invite you to explore our codebase, contribute, and join us in developing a framework to help projects write integration tests.

## Features

Knuu is designed around `Instances`, which you can create, start, control, communicate with other Instances, stop, and destroy.

Some of the features of knuu are:

- Initialize an Instance from a Container/Docker image
- Configure startup commands
- Configure Networking
  - What ports to expose
  - Disable networking to simulate network outages
- Configure Storage
- Execute Commands
- Configure HW resources
- Create a pool of Instances and control them as a group
- Allow AddFile after Commit via ConfigMap
- Implement a TTL value for Pod cleanup
- Add a timeout variable
- See this issue for more upcoming features: [#91](https://github.com/celestiaorg/knuu/issues/91)

> If you have feedback on the framework, want to report a bug, or suggest an improvement, please create an issue [here](https://github.com/celestiaorg/knuu/issues/new/choose).

## Contributing

We warmly welcome and appreciate contributions.

By participating in this project, you agree to abide by the [CNCF Code of Conduct](https://github.com/cncf/foundation/blob/main/code-of-conduct.md).

See the [Contributing Guide](./CONTRIBUTING.md) for more information.

To ensure that your contribution is working as expected, please run [knuu-example](https://github.com/celestiaorg/knuu-example) with your fork and branch.

<!---
## Governance

[Describe the governance model for your project. Reference the GOVERNANCE.md file.]

## Adopters

[Provide information about the public adopters of your project. Reference the ADOPTERS.md file.]

## Security and Disclosure Information

See [SECURITY.md](SECURITY.md) for security and disclosure information.
--->

## Licensing

Knuu is licensed under the [Apache License 2.0](LICENSE).

<!---
## Contact

[Provide contact information for the project maintainers.]
--->
