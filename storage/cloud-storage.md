# Cloud Storage

Create a bucket and set a lifecycle rule

```
gsutil mb -c regional -l us-east1 gs://wingkwong/
gsutil lifecycle set lifecycle.json gs://wingkwong/
```

Sample lifecycle.json 
```json
{
    "lifecycle": {
        "rule": [
            {
                "action": {
                "type": "SetStorageClass",
                "storageClass": "NEARLINE"
                },
                "condition": {
                "age": 365,
                "matchesStorageClass": ["MULTI_REGIONAL", "STANDARD", "DURABLE_REDUCED_AVAILABILITY"]
                }
            },
            {
                "action": {
                "type": "SetStorageClass",
                "storageClass": "COLDLINE"
                },
                "condition": {
                "age": 1095,
                "matchesStorageClass": ["NEARLINE"]
                }
            }
        ]
    }
}
```

Upload an item from the CLI

```
gsutil cp test.txt gs://wingkwong/
```

Move the object to another bucket

```
gsutil mv gs://wingkwong/test.txt gs://wingkwong2/
```

Make an object public

```
gsutil acl ch -u AllUsers:R gs://wingkwong/test.txt
```

Use signed URLs

```
gsutil signurl -d 5m foo.json gs://wingkwong/test.txt
```
