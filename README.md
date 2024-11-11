# Strengths and Achievements

## Adjustments in Risk Parameters

- Modified default setting for "Skip Error FRTB Sensitivities" to `TRUE`.
- Removed the "SKIPERRORFRTBSENSITIVITIES" parameter from RRAO methods.
- Added a new risk parameter, "Enable SBA Optimized Aggregation (SBAOPTIMIZEDAGGREGATION)," in the MET GUI, with values `True`, `False`, and `Default` (False).

## New Method Development

- Implemented a method for CRIF data handling named `FRTBCRIFEnrichedSensitivities` (Reference: MARSGUI-4049).
- Added new FRTB CRIF methods:
  - **FRTBSBACRIFEnrichedSensitivities**: Supports SBA CRIF List with enriched sensitivities, specific regulators, and currency approaches.
  - **FRTBDRCCRIFEnrichedSensitivities**: Manages DRC CRIF Non-Sec Gross JTD data with various configurations for rating aggregation, seniority methods, and internal ratings.
  - **FRTBRAOCRIFEnrichment**: Provides enrichment for RRAO CRIF, handling additional risk types and regulators with specific parameter controls.

## Merlin Enhancement for Qlik Reporting

- Enhanced Qlik reporting (Phase 2) under project MARS-18377.

## Optimizations and Performance Improvements

- **Optimized User Object and Handle Count**: Improved memory usage by generating user objects on demand instead of pre-allocating memory for unused objects.
- **Enhanced Responsiveness**: Applied asynchronous await patterns to improve responsiveness in the Result Output Window for handling extensive reports, moving away from main-thread operations.
- **Improved Hierarchy Manager Load Time**: Reduced the loading time in the Hierarchy Manager from 114 seconds to under 5 seconds through parallel programming.

## Setup and Authentication for CS Applications in UBS Workspace VDI

- Established setup and authentication requirements for accessing PKI and Kerberos-authenticated CS applications within the UBS Workspace VDI environment.
- Supported both Windows and Web applications to ensure seamless and secure access for end-users.

## Support for Merlin and MAR GUI Applications

- Provided support for both the Merlin and MAR GUI applications, contributing to smooth functionality and issue resolution.

## Proof of Concept for MARS Excel Add-In

- Conducted a POC for the MARS Excel Add-In to explore the feasibility of rewriting VBA code in C# to eliminate CORBA dependency.
- Assessed the feasibility of consuming the gRPC endpoint provided by the MARS team for enhanced integration and performance.

## UBS Workspace Migration POCs

- Executed multiple POCs for the UBS Workspace migration, testing the use of client certificates and successfully retrieving the user PID. Verified that this approach works seamlessly, providing a secure and effective solution.
- Conducted a POC using the Identity Provider provided by the Technology Services Team, where a Client ID, Client Secret, and Authority were passed to obtain an Access Token. This token was then successfully attached to HTTP requests, validating secure access to services.

## Issue Fix in Marnet2

- Resolved an issue in the Marnet2 where creating a new file registration was failing with errors ("File not registered" and "invalid date format").
- Addressed the problem where the mandatory checkbox was disabled during new file registration. Identified that after adding and then editing the file, the checkbox became enabled, and made adjustments to ensure the checkbox is available as expected during the initial registration process.

## Scala Training for Team Ramp-Up

- Conducted a comprehensive training session on Scala, including practical code samples, to help the team acquire knowledge and build confidence in working with Scala for upcoming assignments.

## UAT Configuration Updates

- Updated the UAT configuration for several applications, including Riskportal, Marnet, Uploads, Security Server, and AuraSync.
- Coordinated with the team to deploy the updated configurations and pointed the application’s `web.config` to the UAT MSSQL Server `GBWU2709731`, ensuring all components function seamlessly in the UAT environment.

## Certifications Completed

- Successfully completed **Microsoft Certified: Azure Developer Associate (AZ-203)**.
- Successfully completed **Microsoft Certified: Power BI Data Analyst Associate (PL-300)**.
