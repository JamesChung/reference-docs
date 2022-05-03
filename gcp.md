# Google Cloud Platform Reference

## Identity and Access Management (IAM)

There are three types of roles

### Primitive Roles

IAM primitive roles offer fixed, coarse-grained levels of access

- Owner
  - Invite members
  - Remove members
  - Delete projects
  - etc...
- Editor
  - Deploy applications
  - Modify code
  - Configure services
  - etc...
- Viewer
  - Read-only access
- Billing administrator
  - Manage billing
  - Add and remove administrators

A project can have multiple owners, editors, viewers, and billing administrators.

### Predefined Roles

IAM __predefined__ roles apply to a particular GCP service in a project.

IAM predefined roles offer more fine-grained permissions on particular services.

### Custom Roles

IAM custom roles let you define a precise set of permissions.

Good to make least-privilege custom roles based on your organization's security policy needs.

### Service Accounts

Service Accounts control server-to-serer interactions

- Provide an identity for carrying out __server-to-server__ interactions in a project
- Used to __authenticate__ from one service to another
- Used to __control privileges__ used by resources
  - So that applications can perform actions on behalf of authenticated end users
- Identified with an __email__ address:
  - __PROJECT_NUMBER__-compute@developer.gserviceaccount.com
  - __PROJECT_ID__@appspot.gserviceaccount.com
- Service accounts authenticate using keys
  - Google manages keys for Compute Engine and App Engine.
- You can assign a predefined or custom IAM role to the service account.
