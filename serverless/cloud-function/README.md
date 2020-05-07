# Cloud Function

Each function
- has its own lifecycle 
- runs in its own secure execution context
- is separate from all other functions and do not share memory, global variables, file systems, or other state

Features & Benefits
- Code-only implementation
- Zero server management
- Auto scaling
- Pay only while code runs, to nearest 100 milleseconds
- Connects and extends services, on or off Google Cloud
- Code triggered directly via HTTP or in response to events

Use cases
- Serverless Backend Applications
- Real-time Data Processing
- Intelligent Applications

Costs & Quotas
- Cost = Computer Time + Invocations + Networking
    - Compute Time 
        - Measured from time request received until time completed
        - Rounded up to nearest 100ms increment
        - Fees vary depending on memory (GB/sec) and CPU (GHz/sec)
    - Invocations
        - Flat rate charge
        - Same rate for HTTP or background functions
        - Currently, charge is $.40/million, with first million free
    - Networking
        - Flat rate charge
        - Only outbound(egress) data is charge
        - Currently, charge is $.12/GB, with first 5 GB free
        - No charge
            - Incoming (ingress) data
            - Outbound data to Google APIs in same region
        
Deploying
- Python
```
gcloud function deploy test-function-1 --runtime python37 --trigger-http
```

- Nodejs8
```
gcloud function deploy test-function-1 --runtime nodejs8 --trigger-http
```

Testing
```
gcloud function describe test-function-1
```

Environment Variables
- An environment variable is a key/value pair
- Defined at deployment
    - Command line
        -  in a commas separated list
            - Create:
                ``gcloud beta functions deploy FUNCTION_NAME --set-env-vars DATABASE=DB_TEST, DB_CONN=TEST FLAGS...``
            - Update:
                ``gcloud beta functions deploy FUNCTION_NAME --update-env-vars DATABASE=DB_PROD, DB_CONN=PROD FLAGS...``
            - Delete:
                 - Remove one or more:
                ``gcloud beta functions deploy FUNCTION_NAME --remove-env-vars DATABASE FLAGS...``
                 - To clear all set environment variables
                 ``gcloud beta functions deploy FUNCTION_NAME --clear-env-vars FLAGS...``

        - YAML file
            ``gcloud beta functions deploy FUNCTION_NAME --env-vars-file .env.yaml FLAGS...``
            ```yaml
            DATABASE: DB_TEST
            DB_CONN: TEST
            ```
    - Console
    