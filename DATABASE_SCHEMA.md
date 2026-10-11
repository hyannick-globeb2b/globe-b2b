# GLOBE B2B — DATABASE SCHEMA

## SOURCE. GROW. SCALE.

This document defines the data foundation for the Globe B2B operating system.

The database must support the complete business journey:

BUYER → REQUEST → SOURCING → SUPPLIER DISCOVERY → SUPPLIER VERIFICATION → QUOTATION → COMPARISON → NEGOTIATION → PURCHASE → LOGISTICS → DELIVERY → CLIENT ACCEPTANCE

---

# 1. USERS

The users table represents authenticated people using Globe B2B.

Core fields:

- id
- full_name
- email
- phone
- password_hash
- role
- account_status
- country
- city
- profile_photo
- created_at
- updated_at
- last_login_at

Possible roles:

- buyer
- supplier
- partner
- procurement_officer
- sales_agent
- logistics_officer
- finance_officer
- administrator
- super_admin

---

# 2. BUYERS

The buyers table stores organizations or individuals purchasing through Globe B2B.

Core fields:

- id
- user_id
- organization_name
- organization_type
- registration_number
- country
- city
- address
- business_description
- industry
- verification_status
- created_at
- updated_at

---

# 3. SUPPLIERS

The suppliers table stores businesses capable of supplying products or services.

Core fields:

- id
- user_id
- company_name
- registration_number
- country
- city
- address
- website
- industry
- supplier_type
- years_in_business
- verification_status
- verification_score
- created_at
- updated_at

---

# 4. PARTNERS

The partners table stores strategic partners working with Globe B2B.

Core fields:

- id
- user_id
- organization_name
- partner_type
- country
- city
- address
- services
- partnership_status
- created_at
- updated_at

---

# 5. PRODUCTS

The products table stores products that can be sourced or supplied.

Core fields:

- id
- supplier_id
- product_name
- category
- description
- specifications
- unit
- minimum_order_quantity
- estimated_price
- currency
- country_of_origin
- availability_status
- created_at
- updated_at

---

# 6. PRODUCT REQUESTS

The product_requests table stores buyer requests.

Core fields:

- id
- buyer_id
- request_title
- product_description
- category
- quantity
- unit
- target_budget
- currency
- destination_country
- destination_city
- required_delivery_date
- specifications
- request_status
- created_at
- updated_at

Possible request statuses:

- draft
- submitted
- under_review
- sourcing
- quotations_received
- negotiating
- approved
- purchased
- shipped
- delivered
- completed
- cancelled

---

# 7. SUPPLIER VERIFICATION

The supplier_verifications table stores supplier due-diligence information.

Core fields:

- id
- supplier_id
- verification_type
- registration_verified
- identity_verified
- address_verified
- business_verified
- product_verified
- references_checked
- documents_verified
- verification_score
- verification_status
- verified_by
- verified_at
- notes

Possible statuses:

- pending
- in_review
- verified
- rejected
- expired

---

# 8. QUOTATIONS

The quotations table stores supplier quotations against buyer requests.

Core fields:

- id
- request_id
- supplier_id
- quotation_number
- unit_price
- quantity
- subtotal
- shipping_cost
- taxes
- other_costs
- total_price
- currency
- lead_time
- quotation_valid_until
- payment_terms
- delivery_terms
- quotation_status
- created_at
- updated_at

---

# 9. QUOTATION ITEMS

The quotation_items table stores individual products or services contained in a quotation.

Core fields:

- id
- quotation_id
- product_id
- description
- quantity
- unit_price
- subtotal

---

# 10. QUOTE COMPARISONS

The quote_comparisons table allows Globe B2B to compare supplier quotations.

Core fields:

- id
- request_id
- quotation_id
- price_score
- supplier_score
- quality_score
- delivery_score
- payment_terms_score
- overall_score
- recommendation
- created_at

---

# 11. NEGOTIATIONS

The negotiations table records commercial negotiation activity.

Core fields:

- id
- request_id
- quotation_id
- buyer_id
- supplier_id
- initiated_by
- negotiation_status
- original_price
- negotiated_price
- currency
- agreed_quantity
- agreed_terms
- final_agreement
- created_at
- updated_at

---

# 12. PURCHASE ORDERS

The purchase_orders table records approved purchases.

Core fields:

- id
- buyer_id
- supplier_id
- request_id
- quotation_id
- purchase_order_number
- total_amount
- currency
- payment_terms
- delivery_terms
- order_status
- approved_at
- created_at
- updated_at

Possible statuses:

- draft
- pending_approval
- approved
- processing
- partially_fulfilled
- fulfilled
- cancelled

---

# 13. PAYMENTS

The payments table records financial transactions.

Core fields:

- id
- buyer_id
- supplier_id
- purchase_order_id
- payment_reference
- amount
- currency
- payment_method
- payment_type
- payment_status
- transaction_date
- created_at
- updated_at

Possible statuses:

- pending
- processing
- successful
- failed
- refunded
- cancelled

---

# 14. SHIPMENTS

The shipments table manages logistics.

Core fields:

- id
- purchase_order_id
- supplier_id
- buyer_id
- shipment_reference
- carrier
- origin_country
- origin_city
- destination_country
- destination_city
- shipping_method
- tracking_number
- estimated_departure
- estimated_arrival
- actual_departure
- actual_arrival
- shipment_status
- created_at
- updated_at

Possible statuses:

- preparing
- ready_for_pickup
- picked_up
- in_transit
- customs
- out_for_delivery
- delivered
- delayed
- cancelled

---

# 15. DELIVERIES

The deliveries table records delivery confirmation.

Core fields:

- id
- shipment_id
- purchase_order_id
- delivery_date
- received_by
- delivery_location
- quantity_received
- condition
- delivery_status
- proof_of_delivery
- notes
- created_at

---

# 16. CLIENT ACCEPTANCE

The client_acceptance table records final buyer acceptance.

Core fields:

- id
- buyer_id
- purchase_order_id
- delivery_id
- acceptance_status
- accepted_quantity
- rejected_quantity
- rejection_reason
- client_comments
- accepted_by
- accepted_at
- created_at

Possible statuses:

- pending
- accepted
- partially_accepted
- rejected
- disputed

---

# 17. DOCUMENTS

The documents table stores business documents associated with transactions.

Core fields:

- id
- owner_type
- owner_id
- document_type
- file_name
- file_url
- document_number
- verification_status
- uploaded_by
- created_at
- updated_at

Possible document types:

- business_registration
- identification
- supplier_certificate
- quotation
- purchase_order
- invoice
- contract
- shipping_document
- customs_document
- delivery_proof
- other

---

# 18. MESSAGES

The messages table supports communication between platform users.

Core fields:

- id
- sender_id
- receiver_id
- request_id
- subject
- message
- attachment_url
- read_at
- created_at

---

# 19. NOTIFICATIONS

The notifications table stores system notifications.

Core fields:

- id
- user_id
- notification_type
- title
- message
- related_entity_type
- related_entity_id
- is_read
- created_at

---

# 20. AUDIT LOGS

The audit_logs table records important system actions.

Core fields:

- id
- user_id
- action
- entity_type
- entity_id
- old_value
- new_value
- ip_address
- created_at

This table is critical for accountability, security and business traceability.

---

# 21. RELATIONSHIP MODEL

Core relationships:

USER
→ BUYER

USER
→ SUPPLIER

USER
→ PARTNER

BUYER
→ PRODUCT REQUESTS

PRODUCT REQUEST
→ QUOTATIONS

SUPPLIER
→ QUOTATIONS

SUPPLIER
→ PRODUCTS

SUPPLIER
→ SUPPLIER VERIFICATION

REQUEST
→ QUOTATIONS
→ NEGOTIATIONS
→ PURCHASE ORDER

PURCHASE ORDER
→ PAYMENTS
→ SHIPMENTS
→ DELIVERIES
→ CLIENT ACCEPTANCE

ALL IMPORTANT ENTITIES
→ DOCUMENTS
→ AUDIT LOGS

---

# 22. SECURITY PRINCIPLES

The database must enforce:

- Authentication
- Role-based access control
- Authorization
- Password hashing
- Secure sessions
- Data validation
- Input sanitization
- Audit logging
- Restricted access to sensitive documents
- Protection of personal information
- Protection of financial information

Users must only access information they are authorized to see.

---

# 23. BUSINESS RULES

1. A buyer can create multiple product requests.

2. A supplier can submit multiple quotations.

3. A product request can receive multiple quotations.

4. A quotation must belong to a valid supplier and request.

5. A purchase order must be connected to an approved quotation.

6. A shipment must be connected to a purchase order.

7. A delivery must be connected to a shipment.

8. Client acceptance happens after delivery.

9. Supplier verification must be completed before a supplier receives verified status.

10. Financial transactions must maintain permanent audit records.

11. Critical business actions must be logged.

12. Deleted or changed sensitive records must remain traceable through audit mechanisms.

---

# 24. FUTURE EXTENSIONS

The schema is designed to support future modules including:

- Supplier marketplace
- Buyer marketplace
- RFQ marketplace
- Escrow
- Multi-currency payments
- Automated supplier matching
- Logistics integrations
- Customs coordination
- AI procurement assistant
- AI supplier risk scoring
- Automated quotation comparison
- Procurement analytics
- Business intelligence dashboards
- Mobile applications
- API integrations
- Blockchain-based document verification

---

# 25. ARCHITECTURE PRINCIPLE

The Globe B2B database must remain modular, secure, scalable and auditable.

The database is the foundation of the Globe B2B operating system.

SOURCE. GROW. SCALE.
