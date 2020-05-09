# IAM

Role Types
- Primitive roles
-   - Owner, Editor & Viewer (existed prior to the introduction of Cloud IAM)
- Predefined roles
    - granular access for a specific service and ater managed by Google Cloud
- Custome roles
    - granular access for according to a user-specified list of permissions

IAM custom roles let you define a precise set of permissions
- Only can be used at the project / orgranisation levels. (Cannot be used at folder level)
- Google Group
    - Instance Operator Role
        - compute.instances.get
        - compute.instances.list
        - compute.instances.start
        - compute.instances.stop
        - etc ...

Service Accounts
- Apply on service rather than a person
- Provide an identity for carrying out **server-to-server** interactions in a project
- Used to **authenticate** from one service to another using keys
    - Google manages keys for Compute Engine and App Engine
- Used to **control privileges** used by resources
    - So that applications can perform actions on behalf of autheticated end users
- Identified with an email address
    - **PROJECT_NUMBER**-compute@developer.gserviceaccount.com
    - **PROJECT_ID**@appsport.gserviceaccount.com
- You can assign a predefined or custom IAM role to the service account
- Example:
    ```
        Identity            IAM Role                Resource
    Service Account ---- InstanceAdminRole ---- Compute Instances
    ```
- Permissions can be changed without recreating VMs

