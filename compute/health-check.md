# Health Check

Create a health check

```
gcloud compute health-checks create http autohealer-check \
    --check-interval 10 \
    --timeout 5 \
    --healthy-threshold 2 \
    --unhealthy-threshold 3 \
    --request-path "/health"
```

Create a firewall rule to allow health check probes to make HTTP requests

```
gcloud compute firewall-rules create default-allow-http-health-check \
    --network default \
    --allow tcp:80 \
    --source-ranges 130.211.0.0/22,35.191.0.0/16
```

> Pro Tip: Use separate health checks for load balancing and for autohealing. Health checks for load balancing detect unresponsive instances and direct traffic away from them. Health checks for autohealing detect and recreate failed instances, so they should be less aggressive than load balancing health checks. Using the same health check for these services would remove the distinction between unresponsive instances and failed instances, causing unnecessary latency and unavailability for your users.