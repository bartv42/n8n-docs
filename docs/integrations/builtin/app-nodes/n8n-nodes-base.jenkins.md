---
title: Jenkins
description: Documentation for the Jenkins node in n8n, a workflow automation platform. Includes details of operations and configuration, and links to examples and credentials information.
contentType: integration
---

# Jenkins

Use the Jenkins node to automate work in Jenkins, and integrate Jenkins with other applications. n8n has built-in support for a wide range of Jenkins features, including listing builds, managing instances, and creating and copying jobs. 

On this page, you'll find a list of operations the Jenkins node supports and links to more resources.

/// note | Credentials
Refer to [Jenkins credentials](/integrations/builtin/credentials/jenkins/) for guidance on setting up authentication. 
///
/// note | Examples and templates
For usage examples and templates to help you get started, take a look at n8n's [Jenkins integrations](https://n8n.io/integrations/jenkins/){:target="_blank" .external-link} list.
///

## Basic Operations

* Build
    * List Builds
* Instance
    * Cancel quiet down state
    * Put Jenkins in quiet mode, no builds can be started, Jenkins is ready for shutdown
    * Restart Jenkins immediately on environments where it's possible
    * Restart Jenkins once no jobs are running on environments where it's possible
    * Shutdown once no jobs are running
    * Shutdown Jenkins immediately
* Job
    * Copy a specific job
    * Create a new job
    * Trigger a specific job
    * Trigger a specific job

