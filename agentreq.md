# Agent Financial Operations & Compliance Platform
## Production Requirements Specification (Markdown)

**Document Type:** Product / System Requirements Specification  
**Target:** Production-grade U.S./Canada cash-collection agent platform for remittance/MSB operations  
**Version:** 1.0  
**Status:** Draft for Architecture & Implementation  
**Primary Domains:** Agent Management, Cash/Vault, Ledger, Settlement, Reconciliation, Compliance, Regulatory Reporting, Audit

---

## 1. Purpose

This document defines the functional, financial, compliance, operational, security, data, integration, and reporting requirements for a production-grade platform used by a remittance/MSB principal to operate and monitor cash-collection agents in the United States and Canada.

The platform shall support:

- Agent onboarding and lifecycle management
- Agent/location/teller management
- Customer and beneficiary processing
- Cash collection and physical cash custody
- Vault and teller-drawer management
- Double-entry financial ledger
- Balance management
- Agent settlement and commissions
- Bank, transaction, cash, agent, and payout reconciliation
- AML/KYC/KYB and transaction monitoring
- Compliance case management
- U.S. and Canadian regulatory reporting
- Immutable auditability
- Maker-checker controls
- Liquidity and cash forecasting
- Multi-tenant operation
- Bank, PSP, payout-provider, KYC and regulatory integrations

> Regulatory implementation must be validated against the current applicable federal, state, provincial and territorial requirements by qualified compliance/legal professionals. Jurisdictional rules must be configurable rather than hard-coded.

---

# 2. Product Principles

## 2.1 Financial Source of Truth

The double-entry ledger shall be the authoritative source of financial balances.

Operational modules shall not independently mutate financial balances.

```text
Business Event
      ↓
Accounting Event
      ↓
Double-Entry Journal
      ↓
Ledger
      ↓
Derived Balances
      ↓
Settlement / Reconciliation / Reporting
```

## 2.2 Physical Cash vs Financial Ledger

The system shall distinguish:

- Physical cash custody
- Financial ownership/liability
- Cash in transit
- Bank cash
- Agent receivables/payables
- Customer funds liabilities
- Settlement obligations

## 2.3 Immutable Financial History

Posted journal entries and material financial events shall never be edited or deleted.

Corrections shall use reversal/correction entries.

## 2.4 Auditability

Every material action shall be traceable to:

- Actor
- Timestamp
- Entity
- Previous state
- New state
- Reason
- Approval
- Correlation ID
- Evidence where applicable

## 2.5 Maker-Checker

Material financial, compliance, risk, settlement and configuration actions shall support maker-checker approval.

---

# 3. Scope

## 3.1 In Scope

### Organization
- Legal entities
- Tenants
- Jurisdictions
- Regulatory registrations

### Agent
- Applications
- KYB/KYA
- UBOs
- Contracts
- Services
- Locations
- Staff
- Training
- Agent risk
- Agent status

### Transaction
- Customer
- Beneficiary
- Remittance
- Fees
- FX
- Cash funding
- Payout
- Refund
- Cancellation

### Cash
- Vault
- Teller drawer
- Cash accounts
- Denominations
- Cash movement
- Cash count
- Cash variance
- Cash deposit
- Cash withdrawal
- Cash in transit

### Ledger
- Chart of accounts
- Ledger accounts
- Journal entries
- Journal lines
- Accounting events
- Balance snapshots
- Reversals

### Settlement
- Agent settlement
- Agent payable/receivable
- Commissions
- Settlement batches
- Bank settlement
- Failed/returned settlements

### Reconciliation
- Transaction reconciliation
- Bank reconciliation
- Cash reconciliation
- Agent reconciliation
- Settlement reconciliation
- Payout reconciliation
- File import
- Matching
- Exceptions
- Resolution

### Compliance
- KYC/KYB
- UBO
- Sanctions screening
- PEP screening
- Risk scoring
- Transaction monitoring
- Agent monitoring
- Alerts
- Cases
- SAR/STR/CTR/LCTR workflows

### Reporting
- Operational
- Financial
- Agent
- Cash
- Settlement
- Compliance
- Regulatory
- Audit

### Security
- IAM
- RBAC/ABAC
- MFA
- Encryption
- Secrets
- Audit
- PII protection

---

# 4. Out of Scope

Unless explicitly added later:

- Consumer banking
- Deposit-taking banking
- Card issuing
- Lending
- Investment services
- Cryptocurrency custody
- Automated legal interpretation
- Autonomous regulatory filing without human approval

---

# 5. User Roles

## 5.1 Platform Roles

- PLATFORM_ADMIN
- PLATFORM_SUPPORT
- PLATFORM_AUDITOR

## 5.2 MTO Roles

- MTO_ADMIN
- OPERATIONS_MANAGER
- FINANCE_MANAGER
- TREASURY_MANAGER
- COMPLIANCE_OFFICER
- AML_ANALYST
- RECON_ANALYST
- SETTLEMENT_OFFICER
- REPORTING_OFFICER
- AUDITOR
- SUPPORT_AGENT

## 5.3 Agent Roles

- AGENT_ADMIN
- LOCATION_MANAGER
- TELLER
- VAULT_CUSTODIAN
- AGENT_COMPLIANCE
- AGENT_FINANCE

---

# 6. Agent Management Requirements

## FR-AGT-001 Agent Application

The system shall allow creation of an agent application containing:

- Legal name
- DBA/trade name
- Entity type
- Registration number
- Tax identifier
- Business address
- Contact information
- Owners
- Beneficial owners
- Control person
- Services requested
- Expected transaction volume
- Expected cash volume
- Bank information
- Geographic coverage
- Compliance questionnaire
- Supporting documents

## FR-AGT-002 Agent Lifecycle

The system shall support:

```text
DRAFT
APPLICATION_SUBMITTED
KYB_REVIEW
UBO_REVIEW
SCREENING
COMPLIANCE_REVIEW
APPROVED
CONTRACT_PENDING
TRAINING_PENDING
ACTIVE
RESTRICTED
SUSPENDED
TERMINATED
```

## FR-AGT-003 Agent Approval

Agent activation shall require configured approval rules.

## FR-AGT-004 Agent Suspension

Suspension shall support:

- Immediate transaction blocking
- Cash-operation restrictions
- Settlement restrictions
- Compliance hold
- Reason
- Effective time
- Approver
- Audit record

## FR-AGT-005 Agent Monitoring

The platform shall continuously monitor agent activity including:

- Transaction volume
- Cash volume
- Transaction velocity
- Average transaction size
- Customer concentration
- Beneficiary concentration
- Cash variance
- Reconciliation exceptions
- Settlement failures
- Compliance alerts
- Complaints
- Geographic anomalies

---

# 7. Location Requirements

Each agent may have multiple locations.

Each location shall have:

- Unique location code
- Address
- State/province
- Country
- Time zone
- Operating hours
- Services
- Cash limit
- Transaction limit
- Vault
- Teller drawers
- Staff
- Risk status

Location status:

```text
DRAFT
PENDING_APPROVAL
ACTIVE
RESTRICTED
SUSPENDED
CLOSED
```

---

# 8. Teller Requirements

A teller shall have:

- User identity
- Employee number
- Agent
- Location
- Role
- Assigned drawer
- Status

Teller lifecycle:

```text
UNASSIGNED
ASSIGNED
OPEN
CLOSED
SUSPENDED
```

Teller opening shall record physical cash.

Teller closing shall require physical cash count.

---

# 9. Vault Requirements

Each physical location may contain one or more vaults.

Vault shall maintain:

- Maximum cash capacity
- Minimum required cash
- Current physical cash
- Available cash
- Reserved cash
- Cash in transit
- Restricted cash
- Denomination inventory
- Movement history

Vault states:

```text
AVAILABLE
COUNTING
RECONCILIATION_HOLD
COMPLIANCE_HOLD
FROZEN
CLOSED
```

---

# 10. Cash Management Requirements

## FR-CASH-001 Cash Accounts

The platform shall create cash accounts for:

- Teller drawers
- Vaults
- Cash in transit
- Bank cash where appropriate

## FR-CASH-002 Cash Movement

Every physical cash movement shall have:

- Source
- Destination
- Amount
- Currency
- Type
- Initiator
- Approver where required
- Status
- Reference
- Timestamp
- Reason
- Idempotency key

Supported movements:

```text
CUSTOMER_TO_TELLER
TELLER_TO_VAULT
VAULT_TO_TELLER
VAULT_TO_BANK
BANK_TO_VAULT
VAULT_TO_COURIER
COURIER_TO_VAULT
TELLER_TO_BANK
```

## FR-CASH-003 Denomination

The platform shall support denomination-level counts.

## FR-CASH-004 Cash Variance

The system shall calculate:

```text
Expected Cash
vs
Physical Cash
```

Variance shall require:

- Reason
- Evidence where applicable
- Maker
- Checker
- Audit trail

---

# 11. Balance Requirements

Balances shall distinguish:

- Ledger balance
- Available balance
- Reserved balance
- Pending balance
- In-transit balance
- Restricted balance
- Settled balance

The platform shall never use an arbitrary mutable `balance` field as the financial source of truth.

---

# 12. Transaction Requirements

A remittance transaction shall contain:

- Transaction reference
- Customer
- Beneficiary
- Agent
- Location
- Teller
- Source currency
- Source amount
- Fee
- FX rate
- Payout currency
- Payout amount
- Compliance status
- Transaction status
- Settlement status
- Timestamps

Transaction lifecycle:

```text
CREATED
KYC_PENDING
SCREENING
APPROVED
CASH_PENDING
CASH_RECEIVED
FUNDS_CONFIRMED
PROCESSING
PAYOUT_PENDING
PAID
SETTLED
```

Exception states:

```text
REJECTED
CANCELLED
REFUNDED
FAILED
COMPLIANCE_HOLD
```

---

# 13. Double-Entry Ledger Requirements

## FR-LEDGER-001

The ledger shall support:

- Chart of accounts
- Ledger accounts
- Journal entries
- Journal lines
- Accounting events
- Reversals
- Balance snapshots

## FR-LEDGER-002

Every journal entry shall satisfy:

```text
Total Debits = Total Credits
```

for the applicable accounting currency.

## FR-LEDGER-003

Posted entries shall be immutable.

## FR-LEDGER-004

Every financial event shall have an idempotency key.

## FR-LEDGER-005

Every ledger event shall reference its originating business event.

---

# 14. Chart of Accounts

## Assets — 1xxx

```text
1000 Cash & Cash Equivalents

1100 Corporate Bank Accounts
1110 Operating Bank - USD
1120 Operating Bank - CAD
1130 Settlement Bank - USD
1140 Settlement Bank - CAD

1200 Agent Cash
1210 Agent Vault Cash
1220 Agent Teller Cash
1230 Cash In Transit

1300 Receivables
1310 Agent Receivables
1320 Processor Receivables
1330 Payout Partner Receivables

1400 Restricted Assets
1410 Regulatory Reserve
1420 Settlement Reserve
```

## Liabilities — 2xxx

```text
2000 Liabilities

2100 Customer Funds Liabilities
2110 Remittance Customer Funds
2120 Pending Customer Funds

2200 Agent Liabilities
2210 Agent Payable
2220 Agent Commission Payable

2300 Settlement Liabilities
2310 Payout Partner Payable
2320 Bank Settlement Payable

2400 Refund Liabilities
```

## Equity — 3xxx

```text
3000 Equity
3100 Paid-in Capital
3200 Retained Earnings
3300 Current Period Earnings
```

## Revenue — 4xxx

```text
4000 Revenue
4100 Remittance Fees
4200 FX Revenue
4300 Agent Service Revenue
4400 Other Service Revenue
```

## Expenses — 5xxx

```text
5000 Expenses
5100 Agent Commissions
5200 Bank Fees
5300 Processor Fees
5400 Payout Partner Fees
5500 Cash Handling Costs
5600 Courier / Armored Transport
5700 Compliance Costs
5800 Operating Expenses
5900 Losses / Write-offs
```

---

# 15. Accounting Rules

## Customer funds received

Illustrative accounting:

```text
DR Agent/Teller Cash Asset
CR Customer Funds Liability
```

## Teller to vault

```text
DR Vault Cash
CR Teller Cash
```

## Vault to cash-in-transit

```text
DR Cash In Transit
CR Vault Cash
```

## Confirmed bank deposit

```text
DR Bank Cash
CR Cash In Transit
```

## Remittance fee

```text
DR Cash
CR Fee Revenue
```

where the applicable accounting arrangement recognizes the fee as revenue.

## Agent commission

```text
DR Commission Expense
CR Agent Commission Payable
```

Exact revenue recognition and custody treatment shall be validated with the organization's accounting policy and legal structure.

---

# 16. Settlement Requirements

Settlement shall calculate:

```text
Gross Transactions
- Fees
- Commissions
- Adjustments
- Refunds
+/- Other Settlement Items
=
Net Settlement
```

Settlement lifecycle:

```text
CALCULATING
CALCULATED
REVIEW
APPROVED
SUBMITTED
BANK_PENDING
SETTLED
```

Exception states:

```text
FAILED
REJECTED
RETURNED
DISPUTED
```

---

# 17. Reconciliation Requirements

The platform shall support:

1. Transaction reconciliation
2. Bank reconciliation
3. Cash reconciliation
4. Agent reconciliation
5. Settlement reconciliation
6. Payout reconciliation
7. Multi-way reconciliation

## Multi-way reconciliation

```text
Transaction
    ↕
Ledger
    ↕
Agent Cash
    ↕
Bank
    ↕
Payout Provider
```

---

# 18. Reconciliation Pipeline

```text
UPLOAD / API
      ↓
SECURITY SCAN
      ↓
FILE VALIDATION
      ↓
SCHEMA VALIDATION
      ↓
NORMALIZATION
      ↓
DEDUPLICATION
      ↓
MATCHING
      ↓
CONFIDENCE SCORING
      ↓
EXCEPTION CREATION
      ↓
INVESTIGATION
      ↓
MAKER/CHECKER
      ↓
LEDGER ADJUSTMENT
      ↓
CLOSE
```

---

# 19. Reconciliation Match Types

The engine shall support:

```text
EXACT_ID
EXACT_REFERENCE
AMOUNT_DATE
COMPOSITE
FUZZY
MANUAL
NO_MATCH
DUPLICATE
```

Matching criteria may include:

- Transaction ID
- Reference
- Amount
- Currency
- Date
- Value date
- Customer reference
- Bank reference
- Provider reference

Matching rules shall be configurable and versioned.

---

# 20. Reconciliation Exceptions

Exception types:

```text
MISSING_INTERNAL
MISSING_EXTERNAL
AMOUNT_MISMATCH
CURRENCY_MISMATCH
DUPLICATE
TIMING_DIFFERENCE
UNKNOWN
CASH_VARIANCE
SETTLEMENT_VARIANCE
```

Exception states:

```text
OPEN
INVESTIGATING
PENDING_AGENT
PENDING_BANK
PENDING_PROVIDER
PENDING_COMPLIANCE
RESOLVED
REJECTED
WRITTEN_OFF
```

Resolution shall require:

- Root cause
- Explanation
- Evidence
- Proposed adjustment
- Maker
- Checker
- Ledger reference

---

# 21. Bank Reconciliation

The platform shall support:

- Bank account setup
- Statement import
- API ingestion
- CSV/XLSX
- MT940
- CAMT.053
- Statement normalization
- Duplicate detection
- Transaction matching
- Outstanding items
- Adjustments
- Reconciliation close

---

# 22. Compliance Requirements

The compliance platform shall support:

- Customer KYC
- Agent KYB/KYA
- Beneficial ownership
- Sanctions screening
- PEP screening
- Risk scoring
- Transaction monitoring
- Agent monitoring
- Alerts
- Cases
- Evidence
- Regulatory reporting
- Compliance audit trail

---

# 23. Agent Risk Requirements

Risk factors shall include configurable weights for:

- KYB
- UBO
- Geography
- Transaction activity
- Cash activity
- Velocity
- Customer concentration
- Beneficiary concentration
- Complaints
- Previous compliance events
- Reconciliation variance
- Settlement failures
- Operational risk

Risk levels:

```text
LOW
MEDIUM
HIGH
CRITICAL
```

Thresholds shall be configurable by jurisdiction/policy.

---

# 24. Transaction Monitoring

Rules shall support:

- High-value activity
- Velocity
- Structuring indicators
- Multiple customers to same beneficiary
- Multiple agents to same beneficiary
- Same customer across agents
- Sudden volume changes
- Geographic anomalies
- High-risk corridors
- Unusual cash patterns
- Agent behavioral anomalies

Each rule shall have:

- Rule ID
- Version
- Effective date
- Jurisdiction
- Threshold
- Window
- Status
- Severity
- Explanation

---

# 25. Compliance Case Management

A case shall contain:

- Case number
- Case type
- Subject
- Related customer
- Related agent
- Related transactions
- Alerts
- Documents
- Notes
- Investigator
- Decisions
- Regulatory reporting
- Audit history

Case states:

```text
OPEN
INVESTIGATING
PENDING_INFORMATION
ESCALATED
CLEARED
RESTRICTED
REJECTED
REPORTED
CLOSED
```

---

# 26. Regulatory Reporting

The system shall provide a configurable regulatory reporting framework.

U.S. examples:

```text
SAR
CTR
Agent information
Funds-transfer records
State reporting
```

Canada examples:

```text
STR
Large Cash Transaction reporting
Electronic Funds Transfer reporting
Agent/service-agreement records
Other applicable FINTRAC reporting
```

Exact thresholds, fields, aggregation periods and filing workflows shall be configuration-driven and versioned.

---

# 27. Regulatory Data Model

The system shall maintain:

```text
Jurisdiction
Regulatory Rule
Rule Version
Report Type
Report Template
Report Instance
Report Snapshot
Submission
Submission Receipt
```

Each generated report shall record:

- Reporting period
- Rule version
- Data snapshot
- Record count
- Generation timestamp
- Generated by
- Approved by
- Submission ID
- Submission receipt
- Hash

---

# 28. Audit Requirements

Audit events shall be generated for:

- Agent activation
- Agent suspension
- Agent termination
- Location activation
- Bank account change
- Limit change
- Cash adjustment
- Vault transfer
- Teller opening/closing
- Ledger posting
- Ledger reversal
- Settlement approval
- Reconciliation resolution
- Compliance case decision
- Regulatory report generation
- Regulatory report submission
- User permission change

Audit records shall be append-only.

---

# 29. Document Management

Documents shall support:

- Upload
- Versioning
- SHA-256 hash
- MIME type
- Size
- Virus scan
- Encryption
- Retention policy
- Access logging
- Entity association

Sensitive documents shall be stored in encrypted object storage.

---

# 30. Security Requirements

Minimum controls:

- OAuth2/OIDC
- Keycloak-compatible IAM
- MFA
- RBAC
- ABAC where necessary
- Tenant isolation
- Encryption at rest
- TLS
- Secrets management
- PII masking
- Rate limiting
- API authentication
- Privileged-access logging
- Audit trail
- Session controls
- Device/IP controls where required

---

# 31. Tenant Isolation

Every business entity shall be associated with:

```text
tenant_id
```

The backend shall derive tenant identity from authenticated context.

Frontend-supplied tenant IDs shall never be trusted.

PostgreSQL Row Level Security should be considered for high-risk multi-tenant tables.

---

# 32. API Requirements

API conventions:

```text
/api/v1/agents
/api/v1/locations
/api/v1/staff
/api/v1/customers
/api/v1/beneficiaries
/api/v1/remittances

/api/v1/cash/vaults
/api/v1/cash/drawers
/api/v1/cash/movements
/api/v1/cash/counts

/api/v1/ledger/accounts
/api/v1/ledger/journals
/api/v1/balances

/api/v1/settlements

/api/v1/reconciliation/jobs
/api/v1/reconciliation/exceptions

/api/v1/compliance/alerts
/api/v1/compliance/cases

/api/v1/reports
```

All money-moving APIs shall support idempotency.

---

# 33. Idempotency Requirements

Every financial mutation shall accept an idempotency key.

Example:

```text
POST /cash/movements
Idempotency-Key: unique-client-key
```

A retry with the same key shall not create another financial event.

---

# 34. Event Architecture

The platform shall publish domain events including:

```text
AgentApproved
AgentActivated
AgentSuspended
LocationActivated
TellerOpened
TellerClosed
CashReceived
CashMoved
CashCountCompleted
CashVarianceCreated
RemittanceCreated
RemittanceApproved
FundsReceived
PayoutCompleted
SettlementCreated
SettlementApproved
SettlementCompleted
ReconciliationMatched
ReconciliationExceptionCreated
ComplianceAlertCreated
ComplianceCaseOpened
RegulatoryReportGenerated
```

---

# 35. Event Envelope

Every event shall contain:

```json
{
  "eventId": "UUID",
  "eventType": "CashReceived",
  "tenantId": "UUID",
  "aggregateType": "CashAccount",
  "aggregateId": "UUID",
  "occurredAt": "timestamp",
  "actorId": "UUID",
  "correlationId": "UUID",
  "causationId": "UUID",
  "version": 1
}
```

---

# 36. Transactional Outbox

Services producing financial events shall use the transactional outbox pattern.

Within one transaction:

```text
Business Record
Accounting Event
Outbox Event
```

shall commit atomically.

The outbox publisher shall then publish to Kafka.

---

# 37. Integration Requirements

External integration boundary shall support:

- Banks
- ACH/wire providers
- Payment processors
- Payout providers
- KYC/KYB providers
- Sanctions providers
- FX providers
- SFTP
- File uploads
- Regulatory systems

External providers shall not directly modify the ledger.

Provider responses shall enter through controlled integration workflows.

---

# 38. Liquidity Management

The system shall calculate:

```text
Current Cash
+ Expected Incoming
- Expected Outgoing
- Required Reserve
=
Projected Liquidity
```

Liquidity status:

```text
NORMAL
LOW
CRITICAL
```

The system shall support:

- Minimum cash
- Maximum cash
- Replenishment threshold
- Cash concentration
- Agent liquidity
- Location liquidity
- Forecasts
- Replenishment requests

---

# 39. Cash Forecasting

Forecast records shall include:

- Location
- Date
- Expected inflow
- Expected outflow
- Required minimum
- Projected balance
- Model/rule version

Initial implementation may be rules/statistics-based; advanced forecasting can be added later.

---

# 40. Limits Engine

Limits shall support:

```text
Global
Country
State/Province
Agent
Location
Teller
Customer
Transaction
```

Limit types:

```text
Single transaction
Daily
Weekly
Monthly
Cash on hand
Vault
Teller
Settlement
```

Limits shall be versioned and auditable.

---

# 41. Daily Location Close

The system shall support:

```text
Stop/lock appropriate operations
Complete pending teller operations
Count teller cash
Reconcile teller
Reconcile vault
Record variances
Approve variances
Reconcile bank deposits
Close location
Generate daily statement
```

---

# 42. Daily Enterprise Close

```text
All Locations Closed
        ↓
Agent Reconciliation
        ↓
Bank Reconciliation
        ↓
Payout Reconciliation
        ↓
Ledger Trial Balance
        ↓
Settlement Reconciliation
        ↓
Compliance Batch
        ↓
Daily Regulatory Snapshot
```

---

# 43. Statements

The platform shall generate:

### Agent statement

```text
Opening balance
Cash collection
Settlement
Commission
Adjustments
Closing balance
```

### Cash statement

```text
Opening cash
Cash received
Cash transferred
Cash deposited
Cash paid
Closing cash
```

### Ledger statement

```text
Date
Journal
Account
Debit
Credit
Running balance
Reference
```

---

# 44. Reporting

Operational reports:

- Agent activity
- Location activity
- Teller activity
- Cash position
- Cash variance
- Transaction volume
- Settlement
- Reconciliation
- Exceptions

Financial reports:

- Trial balance
- General ledger
- Agent receivable/payable
- Revenue
- Commission
- Bank position
- Cash position

Compliance reports:

- Alerts
- Cases
- Agent risk
- Transaction monitoring
- Screening
- Regulatory submissions

---

# 45. Data Retention

Retention shall be policy/configuration-driven by:

- Jurisdiction
- Record type
- Regulatory requirement
- Legal hold
- Contractual requirement

The system shall support:

- Retention policies
- Legal holds
- Immutable archives
- Controlled disposal
- Deletion approval where legally permitted

---

# 46. Privacy

The system shall support:

- Data minimization
- Field-level protection for sensitive data
- PII masking
- Access logging
- Purpose-based access
- Export controls
- Data subject workflows where applicable
- Retention/deletion controls subject to legal/regulatory requirements

---

# 47. Observability

Every request/event should carry:

```text
trace_id
correlation_id
tenant_id
agent_id
location_id
transaction_id
journal_entry_id
```

Monitoring shall include:

- API latency
- Error rate
- Event lag
- Failed jobs
- Ledger failures
- Reconciliation failures
- Integration failures
- Settlement failures
- Queue depth
- Database health
- Security events

---

# 48. Disaster Recovery

Target objectives shall be defined by business criticality.

Recommended initial targets:

```text
RPO ≤ 5 minutes
RTO ≤ 30 minutes
```

Subject to infrastructure and business validation.

Controls:

- PostgreSQL PITR
- Multi-zone deployment
- Backup encryption
- Immutable backups
- Object storage versioning
- Kafka retention
- Disaster recovery environment
- Recovery testing

---

# 49. Financial Invariants

Automated controls shall continuously verify:

```text
Every journal entry balances.

Posted journals are immutable.

Every financial event has an accounting event.

Every cash movement has source and destination.

Every cash movement has corresponding ledger impact.

No duplicate idempotency key creates duplicate posting.

Every settlement has supporting transactions.

Every reconciliation adjustment has approval.

Every report is reproducible from a defined snapshot.

Every tenant is isolated.
```

---

# 50. Core Database Domains

Recommended PostgreSQL logical schemas:

```text
identity
organization

agent
location
staff

customer
beneficiary
transaction

cash
vault
settlement

ledger
accounting

reconciliation

compliance
monitoring
case_management

regulatory

document
audit

reporting
```

---

# 51. Core Tables

## Organization

```text
tenants
legal_entities
jurisdictions
regulatory_registrations
```

## Agent

```text
agents
agent_owners
agent_services
agent_contracts
agent_documents
agent_bank_accounts
agent_limits
agent_risk_profiles
agent_training
agent_regulatory_profiles
```

## Location

```text
locations
location_services
location_hours
```

## Staff

```text
staff
staff_roles
staff_assignments
```

## Customer

```text
customers
customer_identifiers
customer_addresses
customer_risk_profiles
```

## Beneficiary

```text
beneficiaries
beneficiary_accounts
```

## Transaction

```text
remittance_transactions
transaction_events
transaction_fees
transaction_fx
transaction_payouts
transaction_status_history
```

## Cash

```text
cash_accounts
cash_drawers
vaults
cash_denominations
cash_movements
cash_counts
cash_adjustments
cash_deposits
cash_withdrawals
```

## Ledger

```text
ledger_accounts
journal_entries
journal_lines
accounting_events
balance_snapshots
ledger_reversals
```

## Settlement

```text
settlement_batches
settlement_items
agent_payables
agent_receivables
settlement_adjustments
```

## Reconciliation

```text
reconciliation_jobs
reconciliation_files
reconciliation_records
reconciliation_items
reconciliation_exceptions
reconciliation_resolutions
match_rules
match_results
```

## Compliance

```text
kyc_cases
kyb_cases
screening_requests
screening_results
risk_profiles
monitoring_rules
monitoring_rule_versions
alerts
compliance_cases
case_notes
case_evidence
regulatory_reports
regulatory_report_snapshots
submissions
```

## Audit

```text
audit_events
approval_requests
approval_actions
```

---

# 52. State Machine Requirements

All important lifecycle states shall be explicit state machines.

Required state machines:

1. Agent
2. Location
3. Teller
4. Vault
5. Cash movement
6. Remittance
7. Settlement
8. Reconciliation job
9. Reconciliation exception
10. Compliance alert
11. Compliance case
12. Regulatory report
13. Bank statement import

State transitions shall:

- Be validated by the backend
- Be permission controlled
- Be auditable
- Emit domain events
- Reject invalid transitions

---

# 53. Agent State Machine

```text
DRAFT
 → APPLICATION_SUBMITTED
 → KYB_REVIEW
 → UBO_REVIEW
 → SCREENING
 → COMPLIANCE_REVIEW
 → APPROVED
 → CONTRACT_PENDING
 → TRAINING_PENDING
 → ACTIVE
```

From ACTIVE:

```text
ACTIVE → RESTRICTED
ACTIVE → SUSPENDED
ACTIVE → TERMINATED
```

---

# 54. Cash Movement State Machine

```text
CREATED
 → PENDING_APPROVAL
 → APPROVED
 → IN_TRANSIT
 → RECEIVED
 → COMPLETED
```

Exceptions:

```text
REJECTED
CANCELLED
FAILED
DISPUTED
```

---

# 55. Reconciliation Job State Machine

```text
CREATED
 → VALIDATING
 → NORMALIZING
 → MATCHING
 → REVIEW
 → COMPLETED
```

Exceptions:

```text
FAILED
CANCELLED
```

---

# 56. Compliance Alert State Machine

```text
CREATED
 → ASSIGNED
 → INVESTIGATING
 → ESCALATED
 → DECISION
 → CLOSED
```

Possible decisions:

```text
CLEAR
RESTRICT
REJECT
ESCALATE
REPORT
```

---

# 57. RBAC Requirements

Permissions shall be granular.

Examples:

```text
agent.read
agent.create
agent.update
agent.approve
agent.suspend

location.read
location.create
location.approve

cash.read
cash.receive
cash.transfer
cash.count
cash.adjust
cash.approve_adjustment

ledger.read
ledger.post
ledger.reverse

settlement.read
settlement.create
settlement.approve
settlement.execute

reconciliation.read
reconciliation.create
reconciliation.resolve
reconciliation.approve

compliance.read
compliance.investigate
compliance.close
compliance.report

report.read
report.generate
report.approve
report.submit

audit.read
```

---

# 58. Separation of Duties

Examples:

```text
Teller:
    Receive cash
    Cannot approve own adjustment

Maker:
    Create adjustment
    Cannot approve own adjustment

Checker:
    Approve adjustment
    Cannot modify maker record

Settlement Maker:
    Prepare settlement

Settlement Checker:
    Approve settlement

Compliance Analyst:
    Investigate

Compliance Approver:
    Approve final disposition
```

---

# 59. API Security

Every API request shall enforce:

```text
Authentication
Tenant
Role
Permission
Entity scope
State
Limit
Approval policy
```

---

# 60. Performance Requirements

Initial production target:

```text
P95 read API latency < 300ms
P95 write API latency < 500ms
```

Financial writes may exceed these targets where transaction integrity requires synchronous processing.

The system shall support horizontal scaling for:

- Transaction service
- Compliance monitoring
- Reconciliation workers
- Notification workers
- Integration workers

---

# 61. Availability

Financial transaction APIs should target:

```text
99.9%+
```

Actual SLA shall be determined by production infrastructure and contractual requirements.

Ledger availability shall be treated as a critical dependency.

---

# 62. Concurrency

The system shall protect against:

- Double cash posting
- Concurrent drawer closing
- Double settlement
- Duplicate payout
- Duplicate reconciliation adjustment
- Concurrent balance mutation
- Concurrent approval

Use appropriate:

- Database transactions
- Optimistic locking
- Pessimistic locking where necessary
- Unique constraints
- Idempotency
- State transition validation

---

# 63. Database Constraints

Examples:

```text
journal entry must balance
transaction reference unique
idempotency key unique by scope
account code unique per tenant
location code unique per agent
drawer cannot have multiple active sessions
settlement batch cannot be approved twice
posted journal cannot be deleted
```

---

# 64. File Security

Every uploaded reconciliation/document file shall pass:

```text
MIME validation
Extension validation
Size validation
Virus/malware scan
Hashing
Schema validation
Duplicate detection
```

No uploaded file shall directly modify financial balances.

---

# 65. Reconciliation Adjustment Control

Required workflow:

```text
Mismatch
 ↓
Exception
 ↓
Investigation
 ↓
Evidence
 ↓
Proposed Adjustment
 ↓
Maker
 ↓
Checker
 ↓
Journal Entry
 ↓
Exception Resolved
```

---

# 66. Agent Cash Reconciliation Formula

```text
Opening Cash
+ Cash Received
+ Transfers In
- Cash Paid
- Transfers Out
- Bank Deposits
+ Bank Returns
=
Expected Closing Cash
```

Then:

```text
Expected Closing Cash
vs
Physical Cash
=
Variance
```

---

# 67. Location Reconciliation

```text
Σ Teller Cash
+
Vault Cash
+
Cash In Transit
=
Location Cash Position
```

This shall be reconciled against the ledger representation.

---

# 68. Agent Reconciliation

```text
Σ Locations
+
Agent Cash In Transit
+
Agent Bank Cash
+
Receivables
-
Payables
=
Agent Financial Position
```

Exact components depend on the contractual/accounting model.

---

# 69. Bank Reconciliation

```text
Ledger Bank Balance
vs
Bank Statement Balance
```

Outstanding items shall include:

```text
Deposits in transit
Outstanding withdrawals
Bank fees
Unknown deposits
Unknown withdrawals
Timing differences
Duplicates
```

---

# 70. Payout Reconciliation

```text
Internal payout
vs
Provider payout
```

Statuses:

```text
MATCHED
MISSING_PROVIDER
MISSING_INTERNAL
AMOUNT_MISMATCH
STATUS_MISMATCH
DUPLICATE
```

---

# 71. Multi-Currency

Every monetary record shall contain:

```text
currency
amount
```

FX transactions shall additionally contain:

```text
source_currency
source_amount
rate
payout_currency
payout_amount
rate_source
rate_timestamp
```

Never infer currency from account name.

---

# 72. Money Precision

Use fixed precision numeric types.

Recommended:

```text
NUMERIC(20,4)
```

for monetary amounts where the business requires four decimal places.

Do not use floating-point types for money.

---

# 73. Time

Store timestamps in UTC.

Store location timezone separately.

Business-day calculations shall use the applicable location/jurisdiction timezone.

---

# 74. API Versioning

Use:

```text
/api/v1
```

Breaking changes shall use a new major API version.

---

# 75. API Response Standards

Standard response metadata:

```json
{
  "requestId": "UUID",
  "timestamp": "ISO-8601",
  "data": {},
  "errors": []
}
```

Errors shall include stable error codes.

Example:

```text
CASH_ACCOUNT_NOT_FOUND
INSUFFICIENT_AVAILABLE_CASH
INVALID_STATE_TRANSITION
DUPLICATE_IDEMPOTENCY_KEY
APPROVAL_REQUIRED
LIMIT_EXCEEDED
COMPLIANCE_HOLD
```

---

# 76. Production Acceptance Requirements

The system shall not be considered production-ready until:

## Financial

- All ledger entries balance
- Ledger is immutable
- Reversal works
- Idempotency works
- Cash movement is ledger-integrated
- Settlement is ledger-integrated
- Multi-currency is tested

## Cash

- Teller open/close works
- Vault transfers work
- Denomination counting works
- Cash variance works
- Cash-in-transit works
- Physical vs ledger reconciliation works

## Reconciliation

- Bank imports work
- Duplicate detection works
- Matching works
- Exceptions work
- Resolution requires approval
- Adjustments create journal entries

## Compliance

- KYC/KYB workflows work
- Screening works
- Monitoring rules work
- Alerts work
- Cases work
- Regulatory reports are reproducible
- Sensitive regulatory information is properly access controlled

## Security

- MFA works
- RBAC works
- Tenant isolation works
- Privileged actions are audited
- PII is protected
- Logs do not expose sensitive data

## Reliability

- Duplicate requests do not duplicate money
- Failed events recover
- Outbox retries
- Database recovery works
- Disaster recovery is tested

---

# 77. Recommended Production Architecture

```text
                         WEB / MOBILE
                              │
                         API GATEWAY
                              │
                           KEYCLOAK
                              │
       ┌──────────────────────┼──────────────────────┐
       │                      │                      │
       ▼                      ▼                      ▼
 AGENT SERVICES        TRANSACTION SERVICES     COMPLIANCE
       │                      │                      │
       └──────────────────────┼──────────────────────┘
                              │
                       ACCOUNTING EVENTS
                              │
                              ▼
                       ┌─────────────┐
                       │ LEDGER CORE │
                       └──────┬──────┘
                              │
             ┌────────────────┼────────────────┐
             ▼                ▼                ▼
          CASH/Vault       Settlement       Balance
             │                │                │
             └────────────────┼────────────────┘
                              ▼
                       RECONCILIATION
                              │
             ┌────────────────┼────────────────┐
             ▼                ▼                ▼
           BANK             PAYOUT           AGENT
           RECON             RECON            RECON
             │                │                │
             └────────────────┼────────────────┘
                              ▼
                         DATA PLATFORM
                              │
               ┌──────────────┼───────────────┐
               ▼              ▼               ▼
          Regulatory      Analytics        Reporting
```

---

# 78. Implementation Priority

## Phase 1 — Financial Core

```text
Agent
Location
Teller
Vault
Cash Accounts
Ledger
Journal
Balances
Cash Movement
```

## Phase 2 — Remittance

```text
Customer
Beneficiary
Transaction
Fees
FX
Payout
Settlement
```

## Phase 3 — Reconciliation

```text
Bank Import
Transaction Matching
Cash Reconciliation
Settlement Reconciliation
Agent Reconciliation
Exceptions
Maker/Checker
```

## Phase 4 — Compliance

```text
KYC/KYB
UBO
Screening
Risk
Monitoring
Alerts
Cases
Regulatory Reporting
```

## Phase 5 — Treasury

```text
Liquidity
Cash Forecast
Replenishment
Agent Limits
Cash Concentration
```

## Phase 6 — Advanced

```text
Automated Reconciliation
Anomaly Detection
Predictive Liquidity
Agent Risk Intelligence
Network Analytics
```

---

# 79. Key Architectural Decisions

The implementation shall follow these principles:

1. Ledger is the financial source of truth.
2. Physical cash is modeled separately from financial ownership.
3. No direct balance mutation.
4. Every financial operation is idempotent.
5. Posted journals are immutable.
6. Corrections use reversals.
7. Reconciliation never directly edits balances.
8. Reconciliation adjustments create ledger entries.
9. Compliance is a first-class domain.
10. Regulatory rules are configuration-driven.
11. Agent monitoring is continuous.
12. Maker-checker applies to material actions.
13. Every material action is auditable.
14. Tenant isolation is enforced server-side.
15. External providers cannot directly write financial balances.
16. Domain events use transactional outbox.
17. Sensitive data is encrypted and access controlled.
18. Reports are generated from reproducible snapshots.
19. State machines prevent invalid financial transitions.
20. All production controls must be testable automatically.

---

# 80. Deliverables for the Implementation-Level SRS

The next implementation artifact shall contain:

### A. PostgreSQL
- Complete DDL
- PK/FK
- Indexes
- Unique constraints
- Check constraints
- Enums/status strategy
- RLS strategy
- Audit fields
- Optimistic locking
- Seed data

### B. ERD
- All core entities
- Relationships
- Cardinality
- Financial references
- Domain boundaries

### C. Microservices
- Spring Boot service boundaries
- Responsibilities
- Dependencies
- Database ownership
- APIs
- Events
- Failure handling

### D. Kafka
- Topic catalog
- Event schemas
- Partitions
- Keys
- Ordering requirements
- Retry/DLQ strategy
- Consumer groups
- Idempotency

### E. REST APIs
- Endpoints
- Request/response schemas
- Validation
- Error codes
- Authorization
- Idempotency
- Pagination
- Filtering
- Versioning

### F. State Machines
- States
- Events
- Valid transitions
- Guards
- Side effects
- Permissions

### G. Ledger
- Chart of accounts
- Posting rules
- Journal templates
- Reversal rules
- Multi-currency
- Balance derivation
- Accounting invariants

### H. Reconciliation
- Matching algorithms
- Exact matching
- Composite matching
- Fuzzy matching
- Tolerance rules
- Exceptions
- Resolution
- Adjustments

### I. RBAC
- Roles
- Permissions
- Resource scope
- Agent/location/teller scope
- Maker-checker

### J. Production Testing
- Unit tests
- Integration tests
- Contract tests
- Ledger invariant tests
- Concurrency tests
- Idempotency tests
- Reconciliation tests
- Compliance tests
- Security tests
- Disaster recovery tests
- End-to-end acceptance tests

---

# 81. Definition of Done

The Agent Platform is production-ready only when:

```text
✓ Financial transactions are double-entry accounted
✓ No duplicate financial posting is possible
✓ Physical cash is fully traceable
✓ Vault/teller balances reconcile
✓ Agent balances derive from the ledger
✓ Bank statements reconcile
✓ Payouts reconcile
✓ Settlement is auditable
✓ Reconciliation adjustments are controlled
✓ Agent activity is monitored
✓ Compliance cases are auditable
✓ Regulatory reports are reproducible
✓ Maker/checker is enforced
✓ RBAC is enforced server-side
✓ Tenant isolation is verified
✓ PII is protected
✓ All material events are auditable
✓ Recovery procedures have been tested
✓ Production acceptance tests pass
```

---

# 82. Regulatory Reference Basis

The design should be implemented against the current requirements of the applicable regulator and jurisdiction.

For U.S. MSB operations, relevant FinCEN areas include AML programs, SAR, CTR, funds-transfer recordkeeping and agent information/monitoring.

For Canadian MSB operations, relevant FINTRAC areas include registration, agent eligibility, large cash transaction reporting, electronic funds transfer reporting, suspicious transaction reporting and recordkeeping.

The implementation team must maintain a regulatory rule register with source, effective date, jurisdiction and version so regulatory changes can be incorporated without rewriting core transaction logic.
