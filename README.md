# Microsoft Entra B2B External Contractor Identity Lifecycle

## Overview

This lab demonstrates the design, implementation, testing, and offboarding of a temporary external contractor identity using Microsoft Entra ID B2B collaboration.

The objective was to provide an external cloud security contractor with access to a specific business application while following least-privilege principles and preventing access to unrelated organizational resources.

Rather than assigning application access directly to the contractor, access was managed through a dedicated security group. This created a repeatable authorization model that could support future contractor onboarding and offboarding.

## Scenario

A fictional organization hired **Alex Morgan**, an external Cloud Security Contractor, for a 30-day security assessment.

### Business Requirement

Alex required access to the organization's **Cloud Security Assessment Portal** but did not require:

- Microsoft Entra administrative privileges
- Access to unrelated applications
- Membership in internal employee groups
- Persistent access after the engagement ended

The identity lifecycle implemented in this lab was:

**Invite → Authenticate → Authorize → Validate → Review → Revoke**

## Access Architecture

The authorization model used group-based access rather than direct user assignment:

External Contractor  
↓  
Microsoft Entra B2B Guest  
↓  
SG-External-CloudSecurity-Contractors  
↓  
Cloud Security Assessment Portal

This design separates the identity from the application entitlement and allows access to be granted or revoked through security-group membership.

## Implementation

### 1. External Identity Provisioning

A Microsoft Entra B2B guest identity was created for the external contractor.

The identity was configured with:

- **User type:** Guest
- **Job title:** Cloud Security Contractor
- **Department:** Information Security
- **Employee type:** Contractor

The invitation was redeemed using the contractor's external identity, establishing the B2B guest relationship with the tenant.

### 2. Group-Based Authorization

A dedicated security group was created:

`SG-External-CloudSecurity-Contractors`

Rather than assigning application access directly to the contractor, the contractor was added to this security group. This allows access to be managed based on business function instead of individual user assignments.

### 3. Enterprise Application Access

A non-gallery enterprise application named:

`Cloud Security Assessment Portal`

was created to represent the business resource required during the contractor's engagement.

The contractor security group was assigned to the enterprise application, creating the following authorization path:

**Alex Morgan (B2B Guest) → SG-External-CloudSecurity-Contractors → Cloud Security Assessment Portal**
