# Getting Started - Requirements Definition

## Document Development

### Preparation

|Action|Name|Title|Date|
| :---: | :---: | :---: |:---: |
| Prepared By: | Tim Sun | Technical Consultant | 18/02/2020 |
| Reviewed By: |   |   |   |

### Release

|Version|Date Released|Sections Affected|Remarks|Updated By|Email|
| :---: | :---: | :---: | :---: | :---: | :---: |
|1.0|18/02/2020|   | First Release| Tim Sun|Tim.Sun@team.telstra.com|

## Document Control

### Distribution list

| Name | Title | Organization |
| :---: | :---: | :---: |
| Ipspace Instructor Team |   |   |
| Course Attendees |   |   |

## 1 Introduction

The purpose of this document is to state the requirements for Cloudia project of Company ABC (ABC). It is intended to provide the top-level requirements specification to enable the most appropriate solution to be determined.

Cloudia project will enable ABC to utilize Amazon Web Services (AWS) to host company&#39;s web site.

### 1.1 Document Purpose

The Requirements Definition document captures what needs to be delivered to satisfy the needs of the project, identifying key criteria for the success of the project, phasing and criticality of solution components. This will influence subsequent phasing and delivery schedules of the project.

As such it achieves the following:

- It provides visibility and exposure of stakeholder requirements for review
- It documents the stakeholder team workshop responses, in terms of criticality, and provides a summary rating of each specification
- It categorizes the requirements according to their functional block
- It provides the level of detail necessary to effectively guide the selection of potential deployment solutions
- It provides a basis for assessment of the network solution once implemented

### 1.2 Document Audience

The primary audience for this document consists of the ipspace team such areas as:

- Instructors teams
- Training attendees

This document is of primary importance and warrants input and acceptance from these teams.

### 1.3 Document Scope

This Requirements Definition focuses on the identification and recommendation of solution requirements for the Cloudia project.

### 1.4 Inclusions

The following general areas are considered in scope within this document:

- Functional requirements
- Non-functional requirements

### 1.5 Exclusion

The following areas are specifically out of scope of this document:

- Enterprise solution architecture, including high level network design or architectural discussion
- Information Security strategy, including procedure or design
- Detailed design of the physical layer or network specifics
- Implementation or activation planning
- Migration strategy

### 1.6 Presentation Format

The project requirements will be presented in tables with the following format:

| Identifier | Description | Classification | Priority |
| :---: | :---: | :---: | :---: |
| R.Example.1 | The System MUST have requirements. | **Mandatory** | 1 |
| R.Example.2 | The System MAY have 40 requirements. | **Desirable** | 2 |
  
Where:

- **Identifier** is a unique identifier for the requirement, allowing the requirements to be explicitly referenced in other documents
- **Description** is a prose description of the requirement. It should be specific and verifiable and use terms detailed in Section 
- **Classification** defines the importance of fulfilling the requirement in terms of solution appropriateness. The order of importance is Optional, Desirable and Mandatory
- **Priority** is referenced where time constraints are imposed on the delivery of the program of works. Critical path requirements or components will be delivered before non-critical components

#### 1.6.1 Nomenclature

The following statements have specific meaning when detailing requirements:

| Term | Description |
| :---: | :---: |
| **MUST**| Requirements described using the word MUST are mandatory items. Failing to satisfy these requirements will result in a system that does not fulfil its ultimate purpose.These requirements will have a classification of M, or Mandatory.E.g.: The car MUST have seatbelts. |
| **MUST NOT** | Requirements described using the phrase MUST NOT are mandatory items. Failing to satisfy these requirements will result in a system that does not fulfil its ultimate purpose. This is the opposite of MUST.These requirements will have a classification of M, or Mandatory.E.g.: The car MUST NOT explode when driven at 60 kph. |
| **SHOULD** | Requirements described using the word SHOULD are desirable items. Failing to satisfy these requirements will result in a system that lacks certain features but will still fulfil its ultimate purpose.These requirements will have a classification of D, or Desirable.E.g.: The car SHOULD have air-conditioning. |
| **SHOULD NOT** | Requirements described using the phrase SHOULD NOT are desirable items. Failing to satisfy these requirements will result in a system that has certain undesirable features but will still fulfil its ultimate purpose. This is the opposite of SHOULD.These requirements will have a classification of D, or Desirable.E.g.: The car SHOULD NOT be blue. |
| **MAY** | Requirements described using the word MAY are optional items. Failing to satisfy these requirements will result in a system that fulfils the majority of its purpose but lacks certain &#39;nice to have&#39; features.These requirements will have a classification of O, or Optional.E.g.: The car MAY have GPS navigation. |
| **MAY NOT** | Requirements described using the phrase MAY NOT are optional items. Failing to satisfy these requirements will result in a system that fulfils the majority of its purpose but lacks certain &#39;nice to have&#39; features. This phrase is also used to capture items that were discussed but evaluated as unnecessary.These requirements will have a classification of O, or Optional.E.g.: The car MAY NOT be red. |

## 2 Project Scope

Project scope defines the boundaries of what will be included in the delivery of this project.

### 2.1 Inclusions

The following general areas are considered in scope for this phase of the project:

- **GEN** is an identifier for General requirements
- **CON** is an identifier for Connectivity requirements
- **CAP** is an identifier for Capacity requirements
- **AVA** is an identifier for Availability requirements
- **SEC** is an identifier for Security requirements
- **MGT** is an identifier for Management requirements

| **Identifier** | **Description** | **Classification** | **Priority** |
| :---: | :---: | :---: | :---: |
| R.GEN.1 | The project MUST deliver a solution on AWS | **Mandatory** | 1 |
| R.GEN.2 | The project MUST deploy a web site | **Mandatory** | 1 |
| R.GEN.3 | The project MUST deploy a backend database to host the content of the web site | **Mandatory** | 1 |
| R.GEN.4 | The project SHOULD deploy frontend web servers on Linux servers | **Desirable** | 2 |
| R.GEN.5 | The project SHOULD deploy the web site through automation tools in a repeatable way | **Desirable** | 2 |
|   |   |   |   |
| R.CON.1 | The web site MUST be accessible from the Internet | **Mandatory** | 1 |
| R.CON.2 | The web site MUST be accessible from the corporate network | **Mandatory** | 1 |
| R.CON.3 | Users from the corporate network MUST access the web site via a dedicated network link | **Mandatory** | 1 |
| R.CON.4 | The web site SHOULD provide access with 2 URLs, one for the access from the Internet and the other one for the access from the corporate network | **Mandatory** | 1 |
|   |   |   |   |
|   |   |   |   |
| R.CAP.1 | The web site MUST be capable of handling 1000 connections per second | **Mandatory** | 1 |
| R.CAP.2 | The web site MUST have 100Mbps throughput | **Mandatory** | 1 |
| R.CAP.3 | The dedicated link between the corporate network and the web site MUST have 50Mbps throughput | **Mandatory** | 1 |
|   |   |   |   |
| R.AVA.1 | The project MUST deploy 3 web servers for redundancy | **Mandatory** | 1 |
| R.AVA.2 | The project MUST deploy a load balancer for the web site | **Mandatory** | 1 |
| R.AVA.3 | The web servers SHOULD deployed in different availability zones | **Desirable** | 2 |
|   |   |   |   |
| R.SEC.1 | The project MUST NOT allow users to access the web site through ports other than TCP 443 / HTTPS | **Mandatory** | 1 |
| R.SEC.2 | The project MUST place the web servers and database in different security zones | **Mandatory** | 1 |
| R.SEC.3 | The traffic from the corporate network MUST be encrypted | **Mandatory** | 1 |
| R.SEC.4 | The IP addresses of users SHOULD be logged | **Desirable** | 2 |
|   |   |   |   |
| R.MGT.1 | Admins MUST be able to manage the web servers from the corporate network | **Mandatory** | 1 |
| R.MGT.2 | Admins MUST NOT be able to manage the web site from the Internet | **Mandatory** | 1 |
| R.MGT.3 | Admins MUST be able to manage the database from the corporate network | **Mandatory** | 1 |
| R.MGT.4 | Admins MUST NOT be able to manage the database site from the Internet | **Mandatory** | 1 |
| R.MGT.5 | Admins MUST be able to manage the load balancer from the corporate network | **Mandatory** | 1 |
| R.MGT.6 | Admins MUST NOT be able to manage the load balancer from the Internet | **Mandatory** | 1 |
| R.MGT.7 | Last 10 times of changes MUST be archived | **Mandatory** | 1 |
| R.MGT.8 | Admins MUST NOT share the management account | **Mandatory** | 2 |
| R.MGT.9 | IP addresses of admins SHOULD be logged | **Desirable** | 2 |
| R.MGT.10 | Changes made by admins SHOULD be logged | **Desirable** | 2 |
| R.MGT.11 | Admins SHOULD be able to manage the web site with the corporate network accounts | **Desirable** | 3 |

### 2.2 Exclusions

The following areas are specifically out of scope for this phase of the project:

| **ID** | **Description** |
| :---: | :---: |
| OOS.1 | The content of the web site |
| OOS.2 | AWS accounts creation |

### 2.3 Dependencies

The following are major dependencies for the delivery:

| ID | Description |
| :---: | :---: |
| DEP.1 | Corporate network has centralized account management |
| DEP.2 | Corporate network admins allocate non-duplicate private subnets |
| DEP.3 | ABC company has exsiting ASW accounts |
