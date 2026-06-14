# AppFolio GraphQL Schema

## Overview

This conceptual GraphQL schema models the AppFolio property management platform domain. AppFolio provides cloud-based property management software covering residential, commercial, HOA, student housing, single-family, and affordable housing segments. Integrations are available through the AppFolio Stack API (partner-gated, OAuth 2.0) at developer.appfolio.com.

This schema is derived from AppFolio's publicly documented capabilities, REST API surface, and partner integration documentation. It is intended for conceptual alignment and API design reference.

## Schema Source

- Developer Portal: https://developer.appfolio.com/
- Stack API: https://www.appfolio.com/stack/partners/api
- Help Center: https://helpcenter.appfolio.com/

## Domain Coverage

The schema covers the following AppFolio platform domains:

**Property and Unit Management**
- Properties with full address, type, and status metadata
- Units with rent, occupancy, and availability tracking
- Vacancy management and listing details

**Leasing**
- Leases with full term, status, and tenant associations
- Co-tenant and guarantor relationships
- Move-in/move-out workflows

**Tenant and Owner Management**
- Tenant profiles with contact, payment, and communication history
- Owner records with portfolio associations
- Owner statements and reporting

**Maintenance**
- Maintenance requests with category, priority, and status tracking
- Work orders with vendor assignment and scheduling
- Vendor management and service records

**Financial**
- Invoices with line items, approval status, and payment tracking
- Payments with method, status, and reconciliation support
- Deposits, transactions, and bank account reconciliation

**Applications and Screening**
- Rental applications with applicant details and status
- Background and credit screening results
- Document management for application packets

**Documents**
- Document storage and association with leases, tenants, properties
- Lease document generation and e-signature tracking

**Access and Integrations**
- API key management
- OAuth token lifecycle
- Webhooks and event subscriptions (AppFolio Stack)

## Key Types (65+)

| Type | Description |
|---|---|
| Property | Top-level property record |
| PropertyDetails | Extended metadata for a property |
| PropertyType | Enumeration of property classifications |
| PropertyAddress | Structured address for a property |
| Unit | Individual rentable unit within a property |
| UnitDetails | Extended unit metadata |
| UnitType | Unit classification (studio, 1BR, etc.) |
| UnitStatus | Occupancy/availability status |
| UnitRent | Rent schedule and market rate for a unit |
| Lease | Active or historical lease record |
| LeaseDetails | Extended lease metadata |
| LeaseStatus | Lease lifecycle state |
| LeaseType | Lease classification (residential, commercial, etc.) |
| LeaseTerms | Term length, renewal, and rent escalation |
| Tenant | Tenant record linked to a lease |
| TenantDetails | Extended tenant profile data |
| TenantProfile | Tenant contact and communication preferences |
| CoTenant | Secondary tenant on a lease |
| Owner | Property owner record |
| OwnerDetails | Extended owner metadata |
| OwnerStatement | Periodic financial statement for an owner |
| MaintenanceRequest | Tenant-submitted maintenance issue |
| MaintenanceDetails | Extended maintenance request metadata |
| MaintenanceStatus | Lifecycle status of a maintenance request |
| MaintenanceCategory | Category classification for maintenance work |
| WorkOrder | Assigned work order for a maintenance request |
| WorkOrderDetails | Extended work order metadata |
| WorkOrderStatus | Status of a work order |
| Vendor | Vendor/contractor record |
| VendorDetails | Extended vendor metadata |
| VendorType | Vendor classification |
| Invoice | Financial invoice record |
| InvoiceDetails | Extended invoice metadata |
| InvoiceStatus | Invoice approval and payment status |
| InvoiceLineItem | Line item within an invoice |
| Payment | Payment transaction record |
| PaymentDetails | Extended payment metadata |
| PaymentStatus | Payment processing status |
| PaymentMethod | Payment method (ACH, check, card) |
| Deposit | Security or other deposit record |
| DepositDetails | Extended deposit metadata |
| Transaction | General ledger transaction |
| TransactionDetails | Extended transaction metadata |
| BankAccount | Bank account linked to property or owner |
| Reconciliation | Bank reconciliation record |
| ApplicationDetails | Rental application details |
| Applicant | Individual applicant on a rental application |
| ApplicationStatus | Status of a rental application |
| Screening | Background/credit screening result |
| DocumentDetails | Metadata for a stored document |
| Document | Document binary and content reference |
| LeaseDocument | Document specifically associated with a lease |
| Move | Move-in or move-out record |
| MoveDetails | Extended move metadata |
| Vacancy | Vacant unit record with listing details |
| VacancyDetails | Extended vacancy and listing metadata |
| Report | Generated management or financial report |
| ReportDetails | Extended report metadata and filters |
| APIKey | API credential for Stack API access |
| Token | OAuth 2.0 access token record |
| Webhook | Registered webhook endpoint |
| WebhookEvent | Event payload delivered to a webhook |
| Portfolio | Collection of properties under common management |
| PortfolioDetails | Extended portfolio metadata |
| Contact | Generic contact record |
| Note | Annotation attached to a record |
| Attachment | File attachment on a record |

## Query Examples

```graphql
# Fetch all properties with vacancy status
query {
  properties(filter: { status: VACANT }) {
    id
    name
    address {
      street
      city
      state
      zip
    }
    units {
      id
      unitType
      status
      rent {
        marketRent
        scheduledRent
      }
    }
  }
}

# Fetch open maintenance requests for a property
query {
  maintenanceRequests(propertyId: "prop_123", status: OPEN) {
    id
    category
    priority
    description
    submittedAt
    tenant {
      id
      name
    }
    workOrder {
      id
      vendor {
        name
        phone
      }
      scheduledDate
      status
    }
  }
}

# Fetch owner statement
query {
  ownerStatement(ownerId: "own_456", periodStart: "2026-05-01", periodEnd: "2026-05-31") {
    owner {
      id
      name
    }
    totalIncome
    totalExpenses
    netOperatingIncome
    transactions {
      date
      description
      amount
      type
    }
  }
}
```

## Notes

- AppFolio's Stack API requires partner approval and OAuth 2.0 authentication
- Sandbox environment is available to approved partners
- Webhooks are supported for real-time event delivery
- This schema is conceptual and does not represent an officially published AppFolio GraphQL endpoint
