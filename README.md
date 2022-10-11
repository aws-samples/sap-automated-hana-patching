## Automated Patching for SAP HANA

Versions

Version | Author | Date | Description |
--- | --- | --- | --- |
1.0 | J. Bozelli | 10 October 2022 | Initial Version - HANA Automated Patching |

Description

This sample code is used to create an AWS Systems Manager (SSM) automation document that will automatically patch an SAP HANA database. Before implementing  this automation it is highly recommended to read the SAP Technical Documentation: <insert link here>
  
This sample code is a starting point for customers to understand how to build SSM automation documents that can help benefit the operation of SAP HANA workloads on AWS cloud.

:thought_balloon: Automation code execution diagram

![HANA_AUTO_PATCH_EXEC_DIAG](https://user-images.githubusercontent.com/115275673/195148216-d52e5db9-a43e-4935-a854-65224376e6be.jpg)

:stop_sign: !! Important !!
  
Prior to uploading the code into AWS SSM, it is required to adapt the {ARN} related fields to your specific ARN details. Refusing to do so will result in errors during the execution of the SSM document
  
:no_entry_sign: Not created
  
The code does not create any resources outside of the SSM document into the AWS account. There are several pre-requisites required to execute the SSM automation document as is (without adapting to your specific requirements)
  
* [Amazon S3 Bucket] : It may be required to host the SAP HANA database media files into an Amazon S3 bucket, depending on your execution method for the SSM automation document. 
  
* [EC2 Instance Tags] : The EC2 instances hosting the SAP HANA database workloads require at least two tags to run successfully:
DBSid = <SID>
HanaPatchGroup = <DEV|QAS|PRD|SBX>

* [AWS Secrets Manager] : To allow for reusability of certain parameters, the SSM document will fetch certain inputs from AWS Secrets Manager. The details of the required Secrets can be found in the AWS technical documentation specified in the beginning of this file.

* [AWS KMS] : To allow the EC2 instances running the SAP HANA workloads to decrypt the content of the Secrets from AWS Secrets Manager, KMS keys are required.
  
* [SAP HANA Database User] : A Valid SAP HANA database user ID is required on the SYSTEMDB to perform the update. 
  
* [AWS IAM] : The SSM automation document requires permissions to allow each service to interact with eachother. The AWS technical documentation provides examples of policies to help with the IAM setup. We do recommend you build IAM resources with the principle of least privilege in mind.

:rotating_light: Limitations
  
The SSM document sample code patches only the SAP HANA database, and no additional components. For SAP HANA databases running additional components (i.e. Application Function Library) the code will not work. 
  
:warning: Usage
  
This document has the potential to stop critical systems. Please ensure a valid database backup exists and all dependent applications are stopped. For systems with SAP HSR and/or clustered enabled, please make sure the appropriate pre-steps are executed prior to patching the database.
