## Automated Patching for SAP HANA

Versions

Version | Author | Date | Description |
--- | --- | --- | --- |
1.0 | J. Bozelli & A. Fellipe | 1 Feb 2023 | Initial Version - HANA Automated Patching |
1.1 | J. Bozelli | 15 Mar 2023 | Added HANA 2.0 SP06 Revision 67 as a target |
1.2 | J. Bozelli | 10 Apr 2023 | Added HANA 2.0 SP07 Revision 00 as a target |
1.3 | J. Bozelli | 24 Jul 2023 | Added "Prepare" as an option and local SAPCAR |


Description

This sample code is used to create an AWS Systems Manager (SSM) automation document that will patch an SAP HANA database. Before implementing  this automation it is highly recommended to read the SAP Technical Documentation: https://docs.aws.amazon.com/sap/latest/sap-hana/automated-patching.html
  
This sample code is a starting point for customers to build SSM automation documents that can benefit the operation of SAP workloads on AWS cloud.

:thought_balloon: Automation code execution diagram
  
 The diagram below shows the execution flow of each step of the SSM automation document in the sample code file (sap-hana-patch-sample.yml) 
 

![HANA_AUTO_PATCH_EXEC_DIAG](https://user-images.githubusercontent.com/115275673/207960737-286ebc6b-21c4-4ad2-8788-3c0fb75a0efe.jpg)


:stop_sign: !! Important !! :stop_sign:
  
Prior to uploading the code into AWS SSM, it is required to adapt the {ARN} related fields to your specific ARN details. Refusing to do so will result in errors during the execution of the SSM document
  
:no_entry_sign: Not created :no_entry_sign:
  
The code does not create any resources outside of the SSM document into the AWS account. The below resources are required to run the SSM automation document as is (without adapting to your specific requirements)
  
* [Amazon S3 Bucket] : It may be required to host the SAP HANA database media files into an Amazon S3 bucket, depending on your execution method for the SSM automation document. 
  
* [EC2 Instance Tags] : The EC2 instances hosting the SAP HANA database workloads require two tags to run successfully:
DBSid = {SID} & HanaPatchGroup = {DEV|QAS|PRD|SBX}

* [EC2 Instance | AWS CLI] : Ensure that AWS CLI is properly configured on the instance running HANA. 

* [AWS Secrets Manager] : The SSM document will fetch parameter inputs from AWS Secrets Manager. The details of the required Secrets can be found in the AWS technical documentation specified in the beginning of this file.

* [AWS KMS] : To allow the EC2 instances running the SAP HANA workloads to decrypt the content of the Secrets from AWS Secrets Manager, KMS keys are required.
  
* [SAP HANA Database User] : A Valid SAP HANA database user ID is required on the SYSTEMDB to perform the update. 
  
* [AWS IAM] : The SSM automation document requires permissions to allow each service to interact with eachother. The AWS technical documentation provides examples of policies to help with the IAM setup. We do recommend you build IAM resources with the principle of least privilege in mind.

:rotating_light: Limitations :rotating_light:
  
The SSM document sample code patches only the SAP HANA database, and no additional components. For SAP HANA databases running additional components (i.e. Application Function Library) additional steps are required to incorporate patching the additional components into the SSM automation document. 

:speech_balloon: CloudWatch Logging :speech_balloon:	

The SSM automation document will output logs to CloudWatch Log Groups. The log group in the sample code is SSMHanaAutomatedPatchLogs. To alter to a log group of your choice, update the value of the parameter CloudWatchLogGroupName in each step of the automation document where this parameter is present.
  
:warning: Usage :warning: 
  
This document has the potential to stop critical systems. Please ensure a valid database backup exists and all dependent applications are stopped. For systems with SAP HSR and/or clustered enabled, please make sure the appropriate pre-steps are run prior to patching the database. This document allows you to either run the update directly or run the Prepare Update process. For more information on Preparing an Update, refer to SAP's official documentation [Prepare SAP HANA Packages for Installation and Update](https://help.sap.com/docs/SAP_HANA_PLATFORM/2c1988d620e04368aa4103bf26f17727/1d5920fb965646a5bee76a9bfe290565.html)

:construction: Updates Required :construction:

The following must be updated in the sample code YAML in order to ensure the code can run "out-of-the-box":

{ARN for SAP HANA Software S3 Bucket} - Must be updated to contain the ARN of the S3 bucket where you are storing the SAP HANA media software
  
{ARN for SAP HANA Upgrade username} - Must be updated to contain the ARN of the AWS Secret which has the user ID used to update SAP HANA database
  
{ARN for DEV SAP HANA Upgrade User Password} - Must be updated to contain the ARN of the AWS Secret which has the password for the user ID used to update SAP HANA database for Development
  
{ARN for QAS SAP HANA Upgrade User Password} - Must be updated to contain the ARN of the AWS Secret which has the password for the user ID used to update SAP HANA database for Quality
  
{ARN for PRD SAP HANA Upgrade User Password} - Must be updated to contain the ARN of the AWS Secret which has the password for the user ID used to update SAP HANA database for Production
  
{ARN for SBX SAP HANA Upgrade User Password} - Must be updated to contain the ARN of the AWS Secret which has the password for the user ID used to update SAP HANA database for Sandbox
  
{ARN for SAP HANA version repository S3 bucket} - Must be updated to contain the ARN of the S3 bucket where you are storing the SAP HANA database revision versions
