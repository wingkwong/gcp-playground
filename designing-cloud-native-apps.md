# Designing Cloud Native Apps

## Selecting the appropriate Cloud Service Model

- Infrastructure-as-a-service (IaaS) model
    - large degree of fliexibility in implementation
    - requries a significant amount of labor

- Software-as-a-service (SaaS) model
    - higher velocity for delivery of services while maintaining a fair amount of flexibility
    - at the expense of flexibility 

- Iaas -> CaaS -> PaaS -> Faas -> Saas (from low fliexibility to high velocity)

## Protability and Design Consideration

- Protable Languages 
- Platform considerations
- Platform specific designs are targeted for a specific environment
- Vendor lock in

## Evaluating Different Service Technologies

- Google App Engine
    - you want to focus on writing code, never wants to touch a server, cluster or infrastructure 
    - you want to build a highly reliable and scalable serving app or component without doing it all yourself
    - you value developer velocity over infrastructure control
    - Minimize operational overhead
- Google Kubernetes Engine
    - you want to increase velocity and improve operability dramatically by separating the app from the OS
    - you need a secure, scalable way to manage containers in production
    - you don't have dependencies on a specific operating system
- Google Compute Engine 
    - you need complete control over your infrastructure and direct access to high-performance hardware such as GPUs and local SSDs
    - you need to make OS-level changes, such as providing your own network or graphic drivers, to squeeze out the last drop of performance
    - You want to move your application from your own colo or datacenter to the cloud without rewriting it
    - You need to run a software package that can't easily be containerized or your want to use existing VM images

## Operating System Considerations

- CentOS
- Container-optimized OS from Google
- CoreOS
- Debian
- Red Hat Enterprise Linux (RHEL)
- SUSE Enterprise Linux
- SLEX for SAP
- Ubuntu
- Windows Server

## Location of Your Service Components

- to cut down on latency and provide better services to your end users

## Microservice Architectures

- Separated into independent constituent parts, with each part having its own realm of responisbility
- Refactored from monolithic apps that have very tight coupling to a micro-service based architectures
- Advantages:
    - the code base becomes more modular and easier to manage
    - it becomes much easier to reuse services for other applications
    - it is much easier to scale and tune individules services

## Defining Key Structures

- avoid monotonically increasing keys
- instead, migrate to keys that use random numbers, such as UUID

## Session Management

- keep a session cache 
- Cloud Spanner
    - a limit of 10K sessions per database per node
    - a client can delete a session
    - The Cloud Spanner database service can delete a session when the session is idle for more than 1 hour


## API Management Consideration

- Apps should be designed to have loosely coupled components
- Pub/Sub model enables event-driven architectures & asyn parallel processing 
    - Pulisher---(publish event)---> Event Channel --(Fire Event)---> Subscriber
    - Pulisher---(publish event)---> Event Channel <--(Subscribe)---- Subscriber

## Health Checks

- Cloud Load Balancing
    - Interal Load Balancing
    - TCP Proxy Load Balancing
    - SSL Proxy Load Balancing
    - HTTP(s) Load Balancing
- Stackdriver Monitoring --- uptime check ---> storage/database/network
- Example:
    ```
    gcloud compute health-checks create [PROTOCOl]
    [HEALTH_CHECK_NAME] \
    --description=[DESCRIPTION] \
    --check-interval=[CHECK_INTERVAL] \
    --timeout=[TIMEOUT] \
    --healthy-threshold=[HEALTHY_THRESHOLD] \
    --unhealthy-threshold=[UNHEALTHY_THRESHOLD] \
    ...additional flags
    ```

