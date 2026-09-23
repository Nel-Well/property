# Property Portal — Product Feature Specification

**Status:** Evolving product specification

**Scope:** Product behavior, domain requirements, user journeys, and acceptance criteria

Engineering boundaries and technology choices live in [AGENTS.md](AGENTS.md). Local setup and user-facing project information live in the relevant project README.

## 1. Product summary

Build a property marketplace for Myanmar that supports:

- Buying property
- Selling property
- Renting property
- Multiple cities and townships, beginning with Yangon and Mandalay
- Multiple property categories, beginning with land, apartments, houses, rooms, condominiums, and commercial property
- Role-aware workflows for owners, agents, buyers/renters, and administrators/staff
- A responsive web application first, followed by a native mobile application

The product should make it easy for a visitor to discover properties, narrow results by location and property attributes, inspect trustworthy listing details, and contact the responsible owner or agent. It should also give listing operators and staff clear tools for publishing, managing, and moderating inventory.

## 2. Goals

### 2.1 Primary goals

1. Provide a fast, searchable property catalog for Yangon and Mandalay.
2. Support distinct sale and rental listing workflows.
3. Establish a trustworthy listing lifecycle with moderation and reporting.
4. Support both direct owners and professional agents.
5. Keep the service contract reusable by the web app and future mobile app.
6. Provide realistic, plentiful sample data for development, demos, QA, and UI design.
7. Make adding new cities, townships, categories, and listing types data-driven rather than code-heavy.

### 2.2 Non-goals for the first release

- Online payment, deposits, escrow, or commission settlement
- Legal conveyancing, title verification, or contract generation
- Automated property valuation
- Real-time chat as a launch requirement
- Native mobile implementation before the web experience is stable
- Public user-generated reviews of properties or agents
- Multi-country support

These may be added later without changing the core listing model.

## 3. Users and roles

The system uses role-based access control. A user may have one primary role initially; the data model should allow multiple roles later if needed.

### 3.1 Visitor

- Browse published listings
- Search and filter by transaction type, location, category, price, size, bedrooms, bathrooms, and amenities
- View listing details, media, location summary, and contact options
- Share a listing
- Register or sign in to use saved features

### 3.2 Buyer / renter

- Maintain a profile and preferred contact methods
- Save and unsave listings
- Save searches and receive optional notifications later
- Submit inquiries against a listing
- View inquiry history and statuses
- Report inaccurate, abusive, duplicate, or suspicious listings

### 3.3 Owner

- Create and manage owned listings
- Add property facts, pricing, amenities, media, and location
- Submit listings for review
- View moderation feedback
- Publish, pause, renew, or mark a listing as sold/rented when permitted
- Receive and manage inquiries

### 3.4 Agent

- Maintain an agent profile and agency information
- Create and manage listings on behalf of owners or agencies
- Associate listings with an owner/agency relationship where applicable
- Receive and manage inquiries
- View basic listing performance metrics
- Require verification before receiving a verified-agent badge

### 3.5 Admin / staff

Staff permissions should be granular even if the initial UI groups them together.

- Review, approve, reject, suspend, archive, and feature listings
- Manage users, roles, agencies, locations, categories, amenities, and reference data
- Review reports and take moderation actions
- Manage featured content and platform settings
- Inspect audit logs
- View operational metrics

Suggested staff permission groups:

- `content_moderator`
- `user_support`
- `catalog_manager`
- `administrator`

## 4. Product terminology

- **Transaction type:** `sale` or `rent`
- **Listing:** A marketable property record with a lifecycle and publishable content
- **Property category:** Land, apartment, house, condominium, room, commercial, or a future category
- **Location:** A hierarchical geographic record such as country → region/state → city → township → ward
- **Owner:** The property owner or listing principal
- **Agent:** A user or agency representing the owner
- **Inquiry:** A buyer/renter contact request connected to a listing
- **Featured listing:** A listing promoted by staff in designated placements

## 5. Release strategy

### Phase 1 — Web marketplace foundation

- Public listing discovery and detail pages
- Yangon and Mandalay location catalogs
- Sale and rental flows
- Core categories and filters
- Authentication and role-aware dashboards
- Owner/agent listing submission
- Staff moderation
- Favorites and inquiries
- Seed data and catalog management

### Phase 2 — Trust and growth

- Verified owner/agent profiles
- Saved searches and notifications
- Listing analytics
- Duplicate detection and improved moderation
- Featured listing management
- SEO improvements and structured metadata
- Better map-based browsing if a suitable map provider is selected

### Phase 3 — Mobile

- Mobile app consuming the same API
- Authentication persistence and push notifications
- Mobile-first listing creation and media upload
- Saved listings, inquiries, and search alerts
- Deep links from shared listing URLs

## 6. Core user journeys

### 6.1 Discover a property

1. Visitor opens the marketplace.
2. Visitor chooses `Buy` or `Rent`.
3. Visitor selects Yangon, Mandalay, or a township.
4. Visitor optionally chooses a category and filters.
5. The system returns paginated listings sorted by relevance or newest.
6. Visitor opens a detail page.
7. Visitor saves the listing or sends an inquiry after signing in.

### 6.2 Submit a listing

1. Owner or agent signs in.
2. User starts a listing and chooses sale/rent and category.
3. User enters title, description, price, property facts, location, amenities, and media.
4. User saves a draft or submits for review.
5. Staff reviews the listing.
6. Staff approves, rejects with feedback, or requests changes.
7. An approved listing becomes published when all required data is valid.

### 6.3 Manage an inquiry

1. Buyer/renter submits an inquiry from a listing.
2. The listing owner/agent receives the inquiry in the dashboard.
3. Recipient changes the inquiry status as it progresses.
4. Buyer/renter can see the latest status but not private staff notes.
5. Staff can inspect the inquiry for support or abuse handling.

### 6.4 Moderate a listing

1. Staff sees the moderation queue.
2. Staff reviews content, location, price, media, ownership/agent details, and reports.
3. Staff approves, rejects, suspends, or asks for changes.
4. The system records the action, actor, timestamp, and reason in an audit log.

## 7. Listing requirements

### 7.1 Required listing fields

- Title
- Description
- Transaction type: sale or rent
- Property category
- Price and currency
- Location hierarchy with city and township
- At least one contact-capable owner or agent
- At least one primary image before publication
- Status and publication dates

### 7.2 Optional listing fields

- Address text and landmark
- Latitude/longitude, if available and approved for display
- Floor area and area unit
- Land area and area unit
- Bedrooms and bathrooms
- Floor number and total floors
- Parking count
- Furnished status
- Building year
- Ownership/title notes
- Amenities
- Availability date
- Rental terms, deposit, and minimum lease duration
- Listing reference code

### 7.3 Listing statuses

Suggested lifecycle:

```text
draft → pending_review → published → paused → sold_or_rented → archived
                         ↘ rejected → draft
published → suspended → archived
```

Rules:

- Only staff-approved listings may be public.
- A rejected listing must include a staff reason.
- A suspended listing is hidden from public discovery but retained for audit.
- Sold/rented listings are hidden from default search but may remain accessible to their owner and staff.
- Archiving must not destroy inquiry or audit history.

### 7.4 Search and filtering

Initial filters:

- Transaction type
- City
- Township
- Category
- Minimum and maximum price
- Currency
- Bedrooms and bathrooms
- Minimum and maximum area
- Furnished status
- Amenities
- Listing status, restricted to authorized dashboards
- Created date / freshness

Initial sorting:

- Relevance
- Newest
- Price low to high
- Price high to low
- Largest area

Search results must return total count, page metadata, and applied-filter metadata. Search may begin with database queries and indexed columns; full-text search or a dedicated search engine is a future optimization.

## 8. Domain data requirements

The following is the minimum domain model. Field names are illustrative and should be finalized in the API data contract.

### 8.1 Identity and access

#### `User`

- `id`
- `email` and/or normalized phone number
- password hash or external-auth identity reference
- display name
- avatar URL
- primary role
- account status: active, pending, suspended, deleted
- phone/email verification timestamps
- created/updated timestamps

#### `Role` / permissions

The first release may use an enum, but authorization checks should be permission-oriented so staff roles can become granular.

Roles: `buyer_renter`, `owner`, `agent`, `staff`, `admin`.

#### `Agency`

- name, description, logo, contact details
- verification status
- primary location
- owner/admin relationship
- created/updated timestamps

#### `AgencyMember`

Associates agents/staff with agencies and records membership status and permissions.

### 8.2 Catalog and properties

#### `Location`

- hierarchical parent relationship
- location type: country, region, city, township, ward, neighborhood
- canonical name
- local-language name
- slug
- active flag
- latitude/longitude where appropriate

The initial catalog must include Yangon and Mandalay plus a useful set of townships and neighborhoods. New locations should be addable through catalog management.

#### `PropertyCategory`

- name and local-language name
- slug
- description
- active flag
- display order
- category-specific configuration if needed later

Initial categories: land, apartment, house, condominium, room, commercial, and other.

#### `Property`

Represents the underlying real-world property independently of a particular market listing where practical.

- category
- location
- address/landmark
- size and unit fields
- bedroom/bathroom fields
- structural/property facts
- coordinates and location privacy settings
- created/updated timestamps

#### `Listing`

- property reference
- owner user reference
- optional agent and agency references
- transaction type
- title, slug, description
- price, currency, and rental frequency where applicable
- status and moderation state
- featured flag and feature dates
- listing reference code
- published, expires, sold/rented, and archived timestamps
- created/updated timestamps

#### `Amenity` and `ListingAmenity`

Catalog of amenities and many-to-many listing association. Initial examples: parking, elevator, generator, security, balcony, furnished, air conditioning, water supply, garden, road access, and nearby school.

#### `MediaAsset`

- listing reference
- media type: image or video-ready placeholder
- storage key/URL
- width, height, size, alt text
- sort order
- primary flag
- moderation status
- created timestamp

The first release may use a local or configurable object-storage abstraction. The database should store references, not large binary content.

### 8.3 Engagement and operations

#### `Favorite`

Unique pair of user and listing, with created timestamp.

#### `SavedSearch`

- user
- name
- serialized validated filter criteria
- active flag
- notification preference placeholder

#### `Inquiry`

- listing, sender, recipient/owner/agent references
- message
- preferred contact method
- status: new, contacted, viewing_scheduled, closed, spam
- private staff note field or separate staff-note table
- timestamps

#### `Report`

- reporter
- listing or user target
- reason category
- free-text details
- status: open, reviewing, resolved, dismissed
- resolution and staff actor
- timestamps

#### `ModerationAction`

- target type and target ID
- actor
- action
- reason
- before/after status where useful
- timestamp

#### `AuditLog`

Immutable record of security-sensitive and business-critical actions, including actor, event type, target, request ID, and structured metadata.

#### `Notification`

In-app notification foundation for listing status changes, inquiries, moderation feedback, and future saved-search alerts.

## 9. API feature contract

Expose the product as a versioned JSON API under `/api/v1`. The exact route list may evolve during API design, but the following capabilities are required:

```text
POST   /api/v1/auth/register
POST   /api/v1/auth/login
POST   /api/v1/auth/logout
GET    /api/v1/me

GET    /api/v1/listings
POST   /api/v1/listings
GET    /api/v1/listings/:id-or-slug
PATCH  /api/v1/listings/:id
POST   /api/v1/listings/:id/submit
POST   /api/v1/listings/:id/publish
POST   /api/v1/listings/:id/pause
POST   /api/v1/listings/:id/mark-complete

GET    /api/v1/catalog/locations
GET    /api/v1/catalog/categories
GET    /api/v1/catalog/amenities

GET    /api/v1/me/favorites
POST   /api/v1/listings/:id/favorite
DELETE /api/v1/listings/:id/favorite
GET    /api/v1/me/saved-searches
POST   /api/v1/me/saved-searches

POST   /api/v1/listings/:id/inquiries
GET    /api/v1/me/inquiries
PATCH  /api/v1/inquiries/:id
POST   /api/v1/listings/:id/reports

GET    /api/v1/owner/listings
GET    /api/v1/agent/listings
GET    /api/v1/staff/moderation-queue
POST   /api/v1/staff/listings/:id/approve
POST   /api/v1/staff/listings/:id/reject
POST   /api/v1/staff/listings/:id/suspend
```

The API behavior must include:

- Consistent pagination parameters and response shape
- Consistent validation-error shape
- Consistent authentication failure behavior
- Authorization enforced per resource and action
- No client-trusted owner, agent, staff, or status fields
- Idempotent favorite operations
- Rate limits on authentication, inquiry, report, and public-search abuse vectors
- API documentation before the mobile project begins feature work

## 10. Web application requirements

### 10.1 Public pages

- Home/search landing page
- Search results with filters and sort
- Listing detail page
- Location landing pages for Yangon, Mandalay, and townships
- Category/transaction landing pages
- Sign in and registration
- Public agent/agency profile where approved

### 10.2 Authenticated pages

- Profile and account settings
- Favorites
- Saved searches
- Inquiries
- Owner/agent dashboard
- Listing creation/edit wizard
- Listing status and moderation feedback
- Staff moderation queue
- Staff catalog and user management
- Audit/activity view for authorized staff

### 10.3 UX requirements

- Responsive from small laptop widths through mobile web widths
- Keyboard navigable and accessible form controls
- Clear empty, loading, validation, and error states
- Draft autosave or explicit save-draft behavior for long listing forms
- Preserve filter state in URL query parameters
- SEO-friendly public listing and location routes
- Image gallery with mobile-friendly interaction
- Display price and area units consistently, with room for Myanmar-localized formats

## 11. Mobile application requirements

Mobile is a later phase and must consume the API rather than duplicate business logic.

Initial mobile scope:

- Browse and search listings
- View listing details and media
- Sign in and maintain a session securely
- Favorites and inquiries
- Push notification foundation
- Owner/agent listing management and media upload
- Deep-link a listing from a shared URL

The mobile project should not block web delivery. Its navigation and components may be optimized for small screens rather than attempting pixel-identical parity with the web app.

## 12. Trust, security, and privacy requirements

- Store passwords only as strong one-way hashes.
- Use short-lived access tokens and a secure refresh/session strategy.
- Protect refresh tokens and sensitive sessions from client-side leakage.
- Normalize and verify email/phone identifiers before using them for account recovery.
- Enforce role and resource ownership checks on every protected API action.
- Validate all request bodies, query parameters, path parameters, and uploaded-media metadata.
- Sanitize or safely render user-provided descriptions and messages.
- Apply rate limiting and abuse controls to login, registration, inquiry, report, and media endpoints.
- Never expose internal moderation notes, password data, audit metadata, or private contact data unintentionally.
- Record security-sensitive actions in audit logs.
- Configure CORS, security headers, request size limits, and structured server logging.

## 13. Sample data requirements

The initial development seed must feel like a populated marketplace rather than a handful of placeholder rows.

Minimum recommended seed volume:

- 2 cities: Yangon and Mandalay
- At least 20 townships/neighborhoods across the two cities
- At least 7 property categories
- At least 15 amenities
- At least 60 users across all roles
- At least 10 agents and 4 agencies
- At least 500 listings, with a mix of:
  - sale and rent
  - all initial categories
  - multiple price bands
  - draft, pending, published, paused, rejected, and sold/rented statuses
  - complete and incomplete media sets
  - different bedroom, bathroom, floor, area, and furnishing combinations
- At least 100 favorites
- At least 100 inquiries in varied statuses
- At least 20 reports and moderation actions
- At least 20 saved searches

Seed data rules:

- Use clearly fake names, phone numbers, email addresses, and media URLs.
- Do not use real people’s personal data.
- Generate deterministic data from a documented seed value so tests and demos are repeatable.
- Include edge cases: zero-bedroom land, missing optional facts, high-priced properties, long descriptions, duplicate-like listings, and listings without coordinates.
- Provide role-specific demo credentials through development documentation only; never reuse them in production.

## 14. Observability and operations requirements

- Structured logs with request IDs
- Central error handling with safe public messages
- Health endpoint for liveness and readiness
- Database migration status check
- Basic metrics: listing counts by status, inquiry counts, moderation queue size, authentication failures, and API latency
- Admin-visible operational summaries in a later dashboard phase

## 15. Testing requirements

### API

- Unit tests for validation, authorization, status transitions, and domain services
- Integration tests against a test database
- Endpoint tests for authentication, listing CRUD, search filters, favorites, inquiries, reports, and moderation
- Seed and migration verification

### Web

- Component tests for complex forms and filters
- Route/page tests for public and protected access
- End-to-end coverage for discovery, registration, listing submission, moderation, favorite, and inquiry flows
- Accessibility checks for major pages

### Mobile

- Navigation and rendering tests
- Auth/session tests
- API integration tests with mocked network boundaries
- End-to-end smoke tests after mobile launch scope is defined

## 16. Definition of done for web-first MVP

The web MVP is complete when:

1. A visitor can search and paginate through published sale and rental listings in Yangon and Mandalay.
2. Filters work for location, category, price, bedrooms, area, amenities, and transaction type.
3. A visitor can view a complete listing detail page with media and responsible contact context.
4. A buyer/renter can register, sign in, favorite listings, and send inquiries.
5. An owner or agent can create drafts, upload listing content, submit for review, and see moderation feedback.
6. Staff can approve, reject, suspend, archive, and inspect listing history.
7. Role and ownership checks prevent unauthorized access or mutation.
8. The API is documented and usable independently of the web client.
9. The seed process creates the minimum realistic dataset described above.
10. Automated tests cover the critical workflows.
11. The project can add a new city and category through catalog data without rewriting the listing architecture.

## 17. Open product decisions

These choices should be resolved during product and technical design, not by silently changing the product scope:

- Authentication provider versus first-party email/phone authentication
- Currency display and whether multiple currencies are supported at launch
- Exact Myanmar language and English localization requirements
- Media storage provider and image transformation service
- Whether map display is included in the first web release
- Public address precision and location privacy rules
- SEO rendering strategy for the web app
- Deployment environment and backup policy for the database
- Whether agent verification requires manual documents in the first release
- Whether listing expiry is mandatory and how renewal works
