BC SPCA Microsoft Fabric Governance & Operating Model (Draft)

Version: 0.1 (Draft for Review)
 Owner: BC SPCA Information Technology Team
 Status: Draft
 Audience: IT Leadership, Data Team, Business Owners, Data Owners, Product Owners

1. Purpose

This document provides a proposed governance and operating model for Microsoft Fabric at BC SPCA.

The purpose of this document is to consolidate existing Microsoft Fabric architecture, governance, security, operational, and cost management documentation into a single entry point for discussion and review.

As BC SPCA explores broader adoption of Microsoft Fabric across additional departments, a common set of standards and governance practices will help ensure solutions remain:

Secure
Governed
Scalable
Cost-effective
Supportable
Consistent across the organization

This document is intended as a starting point for discussion with IT leadership and the data team and is not yet considered an approved corporate standard.

2. Vision

BC SPCA will utilize Microsoft Fabric as a centralized enterprise data platform to support:

Business Intelligence
Operational Reporting
Data Engineering
Self-Service Analytics
Data Products
Organizational Decision-Making

The platform should enable departments to leverage organizational data while following consistent standards for governance, architecture, security, and operational support.

3. Guiding Principles
Security by Default

All Fabric solutions should follow least-privilege access principles and adhere to BC SPCA security requirements.

Governance First

Data governance requirements should be defined before solutions are deployed. Data ownership and accountability should be clearly established.

Reuse Before Rebuild

Shared datasets, dimensions, and engineering assets should be leveraged whenever possible to reduce duplication and maintenance effort.

Standardized Architecture

Departments should follow approved Fabric architecture and workspace patterns to ensure consistency and supportability.

Cost Transparency

Capacity usage, storage growth, and processing costs should be visible and monitored.

Business Ownership

Business units own business definitions, requirements, and decisions.

IT provides platform governance, architecture guidance, and technical support.

4. Microsoft Fabric Operating Model
Roles and Responsibilities
Role	ResponsibilityIT Team	Platform administration and governance
Data Team	Architecture, development standards, implementation support
Business Owner	Business requirements and prioritization
Data Owner	Data quality, governance, and business definitions
Product Owner	Data product lifecycle and adoption
End Users	Report consumption and feedback
5. Department Onboarding Framework

When a department requests a Microsoft Fabric solution, the following process is proposed:

Step	Activity1	Identify Business Sponsor
2	Identify Data Owner
3	Define Business Objectives
4	Review Data Sources
5	Assess Sensitive Data Requirements
6	Review Security Requirements
7	Provision Workspace
8	Apply Architecture Standards
9	Implement and Test Solution
10	Deploy to Production
11	Establish Ongoing Ownership and Support
6. Governance Documentation Library
Document Title	Category	High-Level Description	Link	Primary Audience
Technical Architecture Specification – Fabric BI Engine	Architecture	Defines the core Microsoft Fabric BI architecture, including data ingestion, medallion architecture, semantic models, reporting, and Power Apps integration.	https://bcspca.sharepoint.com/:w:/r/sites/IT-HUB/_layouts/15/Doc.aspx?sourcedoc=%7BD8BAAA45-5307-4A0C-A6D3-DC685F1CBDA0%7D	BI Developers, Data Engineers, Architects
BCSPCA Fabric Data Platform Architecture Standard	Architecture	Enterprise-standard architecture for Bronze, Silver, Gold, shared warehouse strategy, ownership boundaries, and scalable data product design.	https://bcspca.sharepoint.com/:w:/r/sites/IT-HUB/_layouts/15/Doc.aspx?sourcedoc=%7B67D7581A-B734-405D-BD9F-C6C825435E1D%7D	Data Team, Architects, Technical Leads
BCSPCA Microsoft Fabric Workspace Strategy	Platform Governance	Defines workspace hierarchy, centralized data engineering model, domain workspaces, OneLake shortcut strategy, and environment structure.	https://bcspca.sharepoint.com/:w:/r/sites/IT-HUB/_layouts/15/Doc.aspx?sourcedoc=%7B82E3698D-C2F6-462B-9E38-FC2BB6B9DF8E%7D	BI Team, Data Engineers, IT Managers
BCSPCA Fabric Data Engineering Naming Standard	Standards	Establishes naming conventions for Lakehouses, Warehouses, Pipelines, Notebooks, Semantic Models, Tables, and Views.	https://bcspca.sharepoint.com/:w:/r/sites/IT-HUB/_layouts/15/Doc.aspx?sourcedoc=%7B993DF3E2-17EE-424F-93F1-1B591496AA07%7D	BI Developers, Data Engineers
Enterprise Governance, Security, and Operational Monitoring Framework for Microsoft Fabric	Governance & Security	Defines platform governance, RBAC, monitoringRC[-2]:RCperational controls, CI/CD standards, and security practices.	https://bcspca.sharepoint.com/:w:/r/sites/IT-HUB/_layouts/15/Doc.aspx?sourcedoc=%7B79151F3E-DB1A-4F69-996C-EE51B8B5C1CF%7D	IT Leadership, Security Team, Data Team
Microsoft Fabric Sensitive Data Handling Standard	Security & Compliance	Defines sensitive data classification, PII handling, Microsoft Purview guidance, access controls, governance practices, and security risks.	https://bcspca.sharepoint.com/sites/IT-HUB/RDLC%20Document%20Library/Technical/Architecture/Microsoft%20Fabric%20Sensitive%20Data%20Handling%20Standard.pdf	Data Owners, Business Owners, IT Leadership
Fabric Governance Strategy	Capacity & Cost Governance	Provides guidance on Fabric capacity planning, workspace ownership, chargeback/showback considerations, and cost management.	https://bcspca.sharepoint.com/sites/IT-HUB/RDLC%20Document%20Library/Products/Animal%20Flow-%20Foster%20Anlysis/Business/Phase1/BRD/Fabric%20Governance%20Strategy.pdf	IT Leadership, Finance, Product Owners
Foster Data Product Cost Analysis	Cost Management	Real-world analysis of Fabric storage, refresh workloads, capacity utilization, and operational cost forecasting for the Foster Analytics data product.	https://bcspca.sharepoint.com/sites/IT-HUB/RDLC%20Document%20Library/Products/Animal%20Flow-%20Foster%20Anlysis/Business/Phase1/BRD/Foster%20Data%20Product%20Cost.pdf	Managers, Directors, Product Owners



The following documents have been developed to establish a Foundation for Microsoft Fabric governance, architecture, security, and operational management.




7. Architecture and Standards

All Microsoft Fabric implementations should leverage approved standards where applicable.

Areas currently covered include:

Platform Architecture
Workspace Design
Naming Standards
Capacity Planning
Cost Governance
Security Controls
Sensitive Data Management
Monitoring and Operations

As new departments onboard to Fabric, these standards may be expanded and refined based on organizational needs.

8. Sensitive Data and Security

Certain business processes require access to confidential information such as:

Volunteer information
Employee information
Operational records
Business-sensitive data

When sensitive data is required for legitimate business purposes, it should be managed according to approved governance and security practices.

Key principles include:

Store only necessary information
Restrict access to authorized users
Follow least-privilege security principles
Apply data classification standards
Leverage Microsoft Purview where appropriate
Remove sensitive information from reporting layers when possible
Conduct periodic access reviews
9. Governance Areas for Review

The following areas are recommended for discussion and review with IT leadership and the data team before additional departments are onboarded to Microsoft Fabric.

Governance and Ownership
Define Data Owner responsibilities
Define Business Owner responsibilities
Define Product Owner responsibilities
Establish governance approval processes
Define ongoing support responsibilities
Key Question

Who is accountable for the data, the solution, and the ongoing business outcomes?

Department Onboarding
Define workspace request process
Define onboarding checklist
Define review and approval requirements
Define support expectations
Key Question

What must a department provide before receiving a Fabric workspace?

Security and Compliance
Validate sensitive data handling requirements
Confirm Microsoft Purview strategy
Review access control standards
Define periodic security reviews
Key Question

How do we ensure sensitive information remains properly governed as adoption grows?

Cost and Capacity Management
Review capacity allocation model
Clarify ownership of Fabric costs
Establish monitoring expectations
Review future growth requirements
Key Question

How will future departments consume shared Fabric capacity?

Data Product Governance
Define what qualifies as a BC SPCA Data Product
Establish ownership expectations
Define lifecycle management
Define retirement and support processes
Key Question

How will BC SPCA govern and support data products over time?

10. Conclusion

BC SPCA has begun establishing a governance foundation for Microsoft Fabric through the development of architecture, workspace, security, naming, operational, and cost management standards.

This document serves as a central index and discussion guide for those standards and provides a starting point for reviewing how Microsoft Fabric can be governed and scaled across additional departments.

The next step is to review these documents collectively with IT leadership and the data team, identify any gaps, and align on an operating model that supports both current and future Microsoft Fabric adoption across BC SPCA.