# Storage

- Use cases:
    - website content
    - storing data for archiving and disaster recovery
        - distributing large data objects to users via direct download
- Scalable to exabytes
- Time to first byte in milliseconds
- Very high availability across all storage classes
- Single API across storage classes

## Signed URLs
- Valet key access to buckets and objects via ticket:
    - crytographically signed URL
    - time-limited
    - operations specfied in ticket: HTTP, GET, PUT, DELETE (not POST)
    - any user with URL can invoke permitted operations
- Example:
    - ``gsutil signurl -d 10m path/to/privatekey.p12 gs://bucket/object``

## Strong Global Consistency

- read-after-write
- read-after-metadata-update
- read-after-delete
- bucket-listing
- object-listing
- granting access to resources

## Choose among Cloud Storage classes

![image](https://user-images.githubusercontent.com/35857179/81492736-cc7dc680-92cc-11ea-8ab9-a7bb81458f74.png)

![image](https://user-images.githubusercontent.com/35857179/82185697-3a08a300-991c-11ea-8392-51a6e9529070.png)


## Cloud Bigtable

- Fully managed NoSQL, wide-column database service for terabyte applications
- Accessed using HBased API
- Native compatibility with big data Hadoop ecosystems
- Managed, scalable storage
- Data encryption in-flight and at rest
- Control access with IAM
- Bigtable drives major applications such as Google Analytics and Gmail

## Cloud SQL

- managed RDBMS  
- offers MYSQL and PostgreSQLBeta databases as a service
- automatic replication
- managed backups
- vertical scaling (read & write)
- horizontal scaling (read)
- google security

## Cloud Spanner

- horizontally scalable RDBMS
- strong global consistency
- managed instances with high availablity 
- SQL queries
    - ANSI 2011 with extensions
- automatic replication

## Cloud Datastore

- designed for application backends
- supports transactions 
- includes a free daily quota

# Comparing Storage Options

![image](https://user-images.githubusercontent.com/35857179/81492881-46627f80-92ce-11ea-9646-da99bba5421d.png)

![image](https://user-images.githubusercontent.com/35857179/81492896-7578f100-92ce-11ea-9f55-cd04c55ac36b.png)

# Decision Flowchart 

![image](https://user-images.githubusercontent.com/35857179/81492949-d56f9780-92ce-11ea-861c-5e1faa2e678c.png)
