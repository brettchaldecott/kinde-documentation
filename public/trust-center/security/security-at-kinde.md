
Kinde takes security threats very seriously as the integrity, confidentiality, and availability of our systems potentially impact our customers. If you have detected a security threat or vulnerability against Kinde systems or personnel, please reach out to your account manager or to the security mailing group at [security@kinde.com](mailto:security@kinde.com).

## Availability

Kinde production services are designed to be resistant to failure with multiple front-end servers and replicated back-end databases. Our main production databases are provisioned, but we use serverless for some supporting roles.

All services are setup in AWS and are configured to use multiple availability zones for redundancy. Please refer to the [Kinde status page](https://status.kinde.com/) for uptime metrics.

## Backups

Backups are performed at least daily to different storage for all production services and data. Databases holding customer data also have point in time recovery so that data can be retrieved from a particular update. Kinde performs restore testing at least annually to validate backup media and restore procedures.

## Business continuity and disaster recovery

Kinde has documented and tested both our business continuity plan and disaster recovery plan. These are updated and refreshed at least annually to ensure that technical events such as a database failover or business impacting events such as a communications tool outage are understood with redundancy plans in place.

## Compliance

Kinde holds a certificate of registration for ISO 27001:2022 and has a SOC 2 attestation. Please refer to our [Compliance certifications](/trust-center/privacy-and-compliance/compliance/) page for more information.

## Data residency

Data is stored in the region you selected during sign up of our product. Currently the available regions are Sydney, Australia; Dublin, Ireland; Oregon, USA; and London, UK. The only data replicated between regions is backend metadata to facilitate the operation of the production services. Otherwise, all user data is not replicated between the regions and will remain in it’s original region.

## Denial of service

Kinde uses a layered approach to protect it’s customers from denial of service attacks to the authentication pages and API, such as protections provided by AWS Shield, rate limiting rules with AWS WAF, and application based throttling.

## Encryption for data at rest

All customer data at rest is encrypted in the Kinde production databases using the industry standard AES256 encryption algorithm. Encryption is facilitated by AWS KMS with access to administer KMS strictly limited to privileged admins.

## Encryption for data in transit

All network traffic to Kinde production services uses TLS 1.2 or greater with a limited set of modern secure ciphers enforced. Security headers are applied to all production endpoints where possible.

## Identity management

Internal identity is generally managed through the company’s single sign-on (SSO) platform provided by Google Workspace. All systems are connected to the company SSO platform where supported. Multi-factor authentication is enforced for all users and audit logs are enabled and monitored for systems connected to the company SSO. For systems that do not support SSO, employees are required to use the company provided password manager to generate and manage secure credentials as well as enable MFA for added protection.

Access to systems is based on the employee’s job role with the least privileges assigned. Privileged access to any system is strictly limited based on the employee’s job role and accounts are individual to that user only.

## Penetration testing

Kinde has completed a network and web application penetration test conducted by [Strike](https://strike.sh/), which was scoped for anything and everything related to Kinde. The test included common OWASP techniques and specifically targeted workflows such as privilege escalations, authentication bypasses, and cloud security. We intend to perform these at least annually.

## Security awareness

All employees take part in a security onboarding session during their first week that covers topics such as acceptable use, phishing, data privacy, identity, endpoint protection, data classification, and incident response.

## Software development lifecycle

All Kinde source code is managed by a company managed source code repository. Source code is scanned using a suite of open source tools, which will alert on insecure coding that could lead to vulnerabilities. Access to the source code repository is restricted based on job role with the identity linked to the company single sign-on platform. Merges to source code require a peer review. Pull requests to the master branch are performed by senior engineers.

## Vulnerability management

Production services are deployed through a CICD pipeline using container technology. Server builds are done at least once a week and replace the existing servers. The build process will use the latest patched container host and container image to reduce the risk of vulnerabilities due to unpatched software.

All container images are scanned at build using AWS Inspector for dependencies and third party library vulnerabilities. External production URLs and any public facing cloud IP addresses are scanned weekly for vulnerabilities by Intruder. All vulnerability reports are triaged, analysed, and assigned to the engineering team for remediation based on vulnerability management SLAs.
