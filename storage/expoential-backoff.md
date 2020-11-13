# Truncated exponential backoff

Truncated exponential backoff is a standard error handling strategy for network applications in which a client periodically retries a failed request with increasing delays between requests.

```
min(((2^n)+random_number_milliseconds), maximum_backoff),
```

where 

- n incremented by 1 for each iteration (request)

- random_number_milliseconds is a random number of milliseconds less than or equal to 1000. 

- maximum_backoff is typically 32 or 64 seconds.