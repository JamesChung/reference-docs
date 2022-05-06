# Google Cloud Platform Reference

## `gcloud`

> If you have an editor or owner primitive role you can ssh via `gcloud`

```sh-session
gcloud compute ssh [instance name] --zone=[zone name]
```

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
- Three types of Service Accounts:
  1. User-created (custom)
  2. Built-in
      - Compute Engine and App Engine default service accounts
  3. Google APIs Service Accounts
- Scopes:
  - Access scopes specify APIs instances have authorization to access, and define level of access instances have with those services
    - Each service has its own scopes
  - Default Compute Engine service account automatically enabled with the following access scopes:
    - Read-only access to Google Cloud Storage
    - Write access to the Compute Engine log

## Use APIs Explorer to help you write your code

- The APIs Explorer is an interactive tool that lets you easily try Google APIs using a browser.
- With the APIs Explorer, you can:
  - Browse quickly through available APIs and versions.
  - See methods available for each API and what parameters they support along with inline documentation.
  - Execute requests for any method and see responses in real time.
  - Easily make authenticated and authorized API calls.

## Creating snapshots

- Unmount the disk or prevent writes
- `sudo umount</mount/point>
- For a boot disk the best practice is to shutdown the server
- If you can't unmount the disk use __sync__ and __fsfreeze__

## Network fundamentals

- Network resources are *global* resources.
  - They can be visible to all resources in a project.
- The FQDN pattern is [HOSTNAME].c.[PROJECT_ID].internal to communicate with internal resources
- Firewall rule tags
  - Use tags to manage how rules are applied to instances. Makes management easier as your rule set get's large.
  - Example: If you create a firewall rule to allow port 21, if the tag associated with the rule is `FTP-SERVER` then if you create an instance with the same tag that instance will adopt the matching firewall rule.

## VPCs

- Google Cloud VPC networks are global
- Subnets are regional

## Cloud Load Balancing

- Users get a single, global anycast IP address
- Traffic goes over the Google backbone from the closest point-of-presence to the user
- Backends are selected based on load
- Only healthy backends receive traffic
- No pre-warming is required

## Metadata Service

- Base URLs
  - The full URL:
    - `http://metadata.google.internal/computeMetadata/v1/`
  - The short URL is:
    - `http://metadata/computeMetadata/v1/`
  - IP address:
    - `http://169.254.169.254/computeMetadata/v1/`
