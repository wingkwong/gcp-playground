# Lifecycle Policy File

Delete the object after 31 days.

```json
{
  "rule":
  [
    {
      "action": {"type": "Delete"},
      "condition": {"age": 31}
    }
  ]
}
```

Set the policy 

```
gsutil lifecycle set life.json gs://$BUCKET_NAME_1
```

Get the policy 

```
gsutil lifecycle get gs://$BUCKET_NAME_1
```