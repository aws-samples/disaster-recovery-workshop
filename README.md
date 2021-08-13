## Disaster Recovery on AWS

Languages: [PT](README.pt.md)

The focus of this content is the reliability patterns and how to apply them to your solutions using AWS services. 
This material is based on the [Reliability Pillar](https://d1.awsstatic.com/whitepapers/architecture/AWS-Reliability-Pillar.pdf?ref=reliability-refarch) of the [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/?ref=reliability-refarch). 
It provides a set of mechanisms to help customers to apply best practices in the design, delivery, and maintenance of Amazon Web Services (AWS) environments. 
By adopting the architectural approaches in this content you will build solutions that have resilient components and proven recovery processes.

This content is intended for those in technology roles, such as Chief Technology Officers (CTOs), solutions architects, developers, and operations team members. 
After reading this, you will understand AWS best practices and strategies to use when designing cloud architectures for reliability.

![DR strategies](workshop/static/images/dr-multi-region.png)

## Getting Started

Visit the workshop on this URL: [disaster-recovery.workshop.aws](https://disaster-recovery.workshop.aws/)

## Developer Guide

This workshop is built with markdown as a static HTML site using [hugo](http://gohugo.io).

```bash
brew install hugo
```

Clone this repository and submodules:

```bash
git clone https://github.com/aws-samples/disaster-recovery-workshop.git
git submodule init
git submodule update
```

You'll find the content of the workshop in the [workshop/](workshop/) directory.

You can start up a local development server by running:

```bash
cd workshop
hugo server -D
```

On your browser open: http://localhost:1313/

## TO-DO:
- [ ] Add new labs - Intermediate Level
- [ ] Add new labs - Advanced Level


## Want to contribute?

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License Summary

The documentation is made available under the Creative Commons Attribution-ShareAlike 4.0 International License. See the LICENSE file.

The sample code within this documentation is made available under the MIT-0 license. See the LICENSE-SAMPLECODE file.
