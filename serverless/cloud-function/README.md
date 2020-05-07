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