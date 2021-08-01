# EP_CompanyManagement

## TOC
1. [About](#About)
2. [Company Management](#Company_Management)
3. [Project Structure](#Project_Structure)

### About
The following platform provides a solution for managing aspects of your
business.

### Company_Management
The following services will be offered:

1. Contractor account management
    1. Onboarding
    2. Contractor pay
    3. Contractor scheduling 
2. Job management
    1. Incoming jobs
    2. Completed jobs
3. Client account management
    1. Onboarding
    2. Accounting

### Project_Structure
1. [User Types](#User_Types)
2. [Front End](#Front_End)
    1. [Account Creation](#Account_Creation)
        - Contractors
        - Clients
        - Admin
    2. [Routes](#Routes)
3. [Back End](#Back_End)

#### User_Types
- [Contractors](#Contractors)
- [Clients](#Clients)
- [Admins](#Admins)

##### Contractors
Contractor user types perform the work for the business. Onboarding for these
accounts is handled by the Stripe Express onboarding flow. See the following
link for details: 
[Stripe Express](https://stripe.com/docs/connect/express-accounts)

##### Clients
Client user types are the entities that require work to be performed. These
can be business or individual accounts. Clients will be billed by each job and 
each job will be tracked by job id.
See the following link for details:
[Charge types](https://stripe.com/docs/connect/charges)

When a client has submitted a job request, an estimate will be issued to 
the client. At this point, the client may choose to accept, modify, or reject
the estimate. If the client accepts the estimate, they will place a deposit
equal to 35% of the project cost.

#### Front_End
The front-end will provide a gateway for the account types to manage aspects
of their user type.

Each gateway will allow the user types to log into their account or create one
if they are new users. 

Log into account:
User will log into their account with their credentials or OAuth2.0 flow

Create an account:
User will first create their platform account using local credentials or the 
OAuth2.0 flow. Once the platform account has been created, the user will be
permitted to start the onboarding process.

The onboarding process will be handled via Stripe [Express](https://stripe.com/docs/connect/express-accounts).

When the user has completed the Connect onboarding flow, they will be redirected
to their platform-provided dashboard. See the following link for details:
[User Dashboard](https://stripe.com/docs/connect/express-dashboard)

##### Account_Creation
Contractors: 
The contractor user type will first create a platform account. Once the
platform account has been created, the user will be able to begin the 
onboarding process. This process will enable the user to accept payments,
handle their tax information and view their account information via their 
dashboard.

Clients: 
The client user type will create an account after their proposal has been 
reviewed and accepted. 

Admins: 
Admin user types will need to initiate and admin account request to recieve an 
admin access link. This requeste action will initiate an email to the primary
platform account owner that an admin request has been made. New admins will be
manually added within Stripe and an account access link will be provided to
the user. The access link will expire at a set time (7days) and the user must
finish their account creation within that time period.

##### Routes
Root Route:
- /
    - provides access to the company management system for all user types.

Contractor and Admin Routes:
- /dashboard
    - users can view recent and upcoming payouts and perform other relevant tasks.
- /dashboard/jobs
    - users can select either inbound, in-review, or completed jobs.
- /dashboard/jobs/inbound
    - users can view inbound jobs yet to be completed
- /dashboard/jobs/review/[id]
    - users will submit a review that will notify the corresponding client that 
    the job has been completed. If completed, the client will accept the work as finalized. 
    If the client does not accept the work, an audit will be triggered until
    the job is accepted by the client. If the client does not accept the work
    and the work is in digital format, they will not receive ownership of the
    completed work and will not be charged. If the work is done for a physical
    item or task, the client may elect to not accept the work but will lose the
    specified deposit. The client will not lose their deposit if they elect to
    have the work completed by the platform.
- /dashboard/jobs/completed
    - lists the jobs that have been completed by that particular contractor. If 
    an account manager is logged in, they will be able to see all completed
    jobs for all contractors.

Client Routes:
- /dashboard
    - clients can view their posted jobs. A progress diagram will be displayed
    that reflects the current stage of the job id.
- /dashboard/review
    - client will be able to review the jobs that are ready to be marked for
    completion. They may choose to approve or request an audit of the work. The
    audit request will trigger an action that will notify the contractor that
    the job has yet to be completed to satisfaction.
- /dashboard/completed
    - client will be able to see their completed job history



#### Back_End
