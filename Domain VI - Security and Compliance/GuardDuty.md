## Amazon GuardDuty Cheat Sheet

- An intelligent threat detection service. It analyzes billions of events across your AWS accounts from AWS CloudTrail (AWS user and API activity in your accounts), Amazon VPC Flow Logs (network traffic data), and DNS Logs (name query patterns).

## **How It Works**

![Amazon GuardDuty](https://td-mainsite-cdn.tutorialsdojo.com/wp-content/uploads/2020/01/amazon-guardduty.jpg)

- GuardDuty is a regional service.
- Threat detection categories
    - **Reconnaissance** — Activity suggesting reconnaissance by an attacker, such as unusual API activity, intra-VPC port scanning, unusual patterns of failed login requests, or unblocked port probing from a known bad IP.
    - **Instance compromise** — Activity indicating an instance compromise, such as cryptocurrency mining, backdoor command and control activity, malware using domain generation algorithms, outbound denial of service activity, unusually high volume of network traffic, unusual network protocols, outbound instance communication with a known malicious IP, temporary Amazon EC2 credentials used by an external IP address, and data exfiltration using DNS.
    - **Account compromise** — Common patterns indicative of account compromise include API calls from an unusual geolocation or anonymizing proxy, attempts to disable AWS CloudTrail logging, changes that weaken the account password policy, unusual instance or infrastructure launches, infrastructure deployments in an unusual region, and API calls from known malicious IP addresses.
- Amazon GuardDuty provides three severity levels (Low, Medium, and High) to allow you to prioritize response to potential threats.
- CloudTrail Event Source
    - GuardDuty analyzes CloudTrail management events and S3 data events. (Read about types of CloudTrail trails for more information.)
    - GuardDuty processes all CloudTrail events that come into a region, including global events that CloudTrail sends to all regions, such as [AWS IAM](https://tutorialsdojo.com/aws-identity-and-access-management-iam/), AWS STS, [Amazon CloudFront](https://tutorialsdojo.com/amazon-cloudfront/), and Route 53.
- VPC Flow Logs Event Source
    - VPC Flow Logs capture information about the IP traffic going to and from Amazon EC2 network interfaces in your VPC.
- DNS Logs Event Source
    - If you use AWS DNS resolvers for your EC2 instances (the default setting), then GuardDuty can access and process your request and response DNS logs through the internal AWS DNS resolvers. Using other DNS resolvers will not provide GuardDuty access to its DNS logs.
- GuardDuty vs Macie
    - Amazon GuardDuty provides broad protection of your AWS accounts, workloads, and data by helping to identify threats such as attacker reconnaissance, instance compromise, and account compromise. Amazon Macie helps you protect your data in Amazon S3 by helping you classify what data you have, the value that data has to the business, and the behavior associated with access to that data.

## **GuardDuty Findings**

- - GuardDuty generates **findings** when it detects unexpected and potentially malicious activity in your AWS environment. These are viewable via Console, GuardDuty CLI or API operations.
    - A Finding’s summary includes:
        - **Finding type** – a concise yet readable description of the potential security issue.
        - **Severity** – a finding’s assigned severity level of either High, Medium, or Low.
        - **Region** – the AWS region in which the finding was generated.
        - **Count** – the number of times GuardDuty generated the finding after you enabled GuardDuty in your AWS account.
        - **Account ID** – the ID of the AWS account in which the activity took place that prompted GuardDuty to generate this finding.
        - **Resource ID** – the ID of the AWS resource against which the activity took place that prompted GuardDuty to generate this finding.
        - **Threat list name** – the name of the threat list that includes the IP address or the domain name involved in the activity that prompted GuardDuty to generate the finding.
        - **Last seen** – the time (your local timezone if checked through console, and UTC if checked through CLI or API) at which the activity took place that prompted GuardDuty to generate this finding.
    - A finding’s **Resource affected** section includes:
        - **Resource role** – a value that usually is set to **Target** because the affected resource can be a potential target of an attack.
        - **Resource type** – the type of the affected resource. This value is either **AccessKey** or **Instance**.
        - **Instance ID** – the ID of the EC2 instance involved in the activity that prompted GuardDuty to generate the finding.
        - **Port** – the port number for the connection used during the activity that prompted GuardDuty to generate the finding.
        - **Access key ID** – access key ID of the user engaged in the activity that prompted GuardDuty to generate the finding.
        - **Principal ID** – the principal ID of the user engaged in the activity that prompted GuardDuty to generate the finding.
        - **User type** – the type of user engaged in the activity that prompted GuardDuty to generate the finding.
        - **User name** – The name of the user engaged in the activity that prompted GuardDuty to generate the finding.
    - A finding’s **Action** section includes:
        - **Action type** – the finding activity type. This value can be one of the following: NETWORK_CONNECTION, AWS_API_CALL, PORT_PROBE, or DNS_REQUEST.
        - **API** – the name of the API operation that was invoked and thus prompted GuardDuty to generate this finding.
        - **Service name** – the name of the AWS service (GuardDuty) that generated the finding.
        - **Connection direction** – the network connection direction observed in the activity that prompted GuardDuty to generate the finding. The values can be INBOUND, OUTBOUND, and UNKNOWN.
        - **Protocol** – the network connection protocol observed in the activity that prompted GuardDuty to generate the finding.
    - A finding’s **Actor** section includes:
        - **Location** – location information of the IP address involved in the activity that prompted GuardDuty to generate the finding.
        - **Organization** – ISP organization information of the IP address involved in the activity that prompted GuardDuty to generate the finding.
        - **IP address** – the IP address involved in the activity that prompted GuardDuty to generate the finding.
        - **Port** – the port number involved in the activity that prompted GuardDuty to generate the finding.
        - **Domain** – the domain involved in the activity that prompted GuardDuty to generate the finding.
    - A finding’s **Details** section includes:
        - **ThreatPurpose** – describes the primary purpose of a threat or a potential attack. Can have the following values:
            - **Backdoor** – this value indicates that the attack has compromised an AWS resource and is capable of contacting its home command and control (C&C) server to receive further instructions for malicious activity.
            - **Behavior** – this value indicates that GuardDuty is detecting activity or activity patterns that are different from the established baseline for a particular AWS resource.
            - **Cryptocurrency** – this value indicates that GuardDuty is detecting software that is associated with cryptocurrencies.
            - **Pentest** – sometimes owners of AWS resources or their authorized representatives intentionally run tests against AWS applications to find vulnerabilities, like open security groups or access keys that are overly permissive. These pen tests are done in an attempt to identify and lock down vulnerable resources before they are discovered by attackers.
            - **Persistence** – this value indicates that a principal in your AWS environment is exhibiting behavior that is different from the established baseline. Such as a principal has no prior history of updating network configuration settings, or updating policies or permissions attached to AWS users or resources.
            - **Policy** – this value indicates that your AWS account is exhibiting behavior that goes against recommended security best practices.
            - **PrivilegeEscalation** – this value informs you that a specific principal in your AWS environment is exhibiting behavior that can be indicative of a privilege escalation attack.
            - **Recon** – this value indicates that a reconnaissance attack is underway, scoping out vulnerabilities in your AWS environment by probing ports, listing users, database tables, and so on.
            - **ResourceConsumption** – this value indicates that a principal in your AWS environment is exhibiting behavior that is different from the established baseline. Such as a principal has no prior history of launching EC2 instances.
            - **Stealth** – this value indicates that an attack is actively trying to hide its actions and its tracks.
            - **Trojan** – this value indicates that an attack is using Trojan programs that silently carry out malicious activity. Sometimes this software takes on an appearance of a legitimate program. Sometimes users accidentally run this software. Other times this software might run automatically by exploiting a vulnerability.
            - **UnauthorizedAccess** – this value indicates that GuardDuty is detecting suspicious activity or a suspicious activity pattern by an unauthorized individual.
        - **ResourceTypeAffected** – describes which AWS resource is identified in this finding as the potential target of an attack. Currently, only EC2 instances and principals (and their credentials) can be identified as affected resources in GuardDuty findings.
        - **ThreatFamilyName** – describes the overall threat or potential malicious activity that GuardDuty is detecting.
        - **ThreatFamilyVariant** – describes the specific variant of the **ThreatFamily** that GuardDuty is detecting. Attackers often slightly modify the functionality of the attack, thus creating new variants.
        - **Artifact** – describes a specific resource that is owned by a tool that is used in the attack.
    - You can create filters for your GuardDuty findings.
        - A _suppression rule_ is a filter used to automatically archive new findings. After you create a suppression rule, new findings that match the criteria defined in the rule are automatically archived.
    - GuardDuty supports exporting active findings to CloudWatch Events and, optionally, to an Amazon S3 bucket. New Active findings that GuardDuty generates are automatically exported within about 5 minutes after the finding is generated. 

## **Trusted IP Lists and Threat Lists**

- - **Trusted IP lists** consist of IP addresses that you have **whitelisted for secure communication** with your AWS infrastructure and applications. GuardDuty does not generate findings for IP addresses on trusted IP lists. 
    - At any given time, you can have only one uploaded trusted IP list per AWS account per region.
    - **Threat lists** consist of known **malicious** IP addresses. GuardDuty generates findings based on threat lists. 
    - At any given time, you can have up to six uploaded threat lists per AWS account per region.

## **Amazon GuardDuty Pricing**

Pricing is based on the quantity of AWS CloudTrail Events analyzed (per 1,000,000 events) and the volume of Amazon VPC Flow Log and DNS Log data analyzed (per GB).