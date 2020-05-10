# Compute

## Compute Engine 
- Offers managed virtual machines
    - No upfront investment
    - Fast and consitent performance
    - Create VMs with GCP Console or **gcloud**
    - Run images of Linux or Windows Server
    - Pick memory & CPU: use predefined types, or make a custom VM
    - Pick GPUs if you need them
    - Pick persistent disks: standard or SSD
    - Pick local SSD for scratch space too if you need it
    - Pick a boot image: Linux or Windows Server
    - Define a startup script if you like
    - Take disk snapshots as backups or as migration tools

- Offers innovative pricing
    - Per-second billing, sustained use discounts
    - Preemptible instances
    - High throughput to storage at no exta cost
    - Custom machine types: Only pay for the hardware you need

- Scales up or scale outs
    - Use big VMs for memory- and compute-intensive applications
    - Use Autoscaling for resilient, scalable applications


## App Engine

- a PaaS for building scalable applications
- makes deployment, maintenance, and scalability easy so you can focus on innovation
- esp suited for building scalable web applications and mobile backends
- standard environment:
    - easily deploy your applications
    - autoscale workloads
    - free daily quota 
    - usage based pricing 
    - SDKs for development, testing and deployment
    - sandbox constraints:
        - no writing to local files
        - all requests time out at 60s
        - limits on third-party software
- flexible environment:
    - build and deploy containerized applications with a click
    - no sandbox constraints 
    - can access App Engine resources
- Comparing the App Engine environments
    ![image](https://user-images.githubusercontent.com/35857179/81493125-65faa780-92d0-11ea-8526-e2146d69d8d9.png)

![image](https://user-images.githubusercontent.com/35857179/81493136-7a3ea480-92d0-11ea-8e33-a70af55fb857.png)
