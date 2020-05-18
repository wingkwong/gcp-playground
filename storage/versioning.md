# Versioning 

View the current versioning

```
gsutil versioning get gs://$BUCKET_NAME_1
```

Enable Versioning

```
gsutil versioning set on gs://$BUCKET_NAME_1
```

Store the version value in the environment variable [VERSION_NAME]

```
export VERSION_NAME=<Enter VERSION name here>
```

Example Result

```
gs://BUCKET_NAME_1/setup.html#1584457872853517
```
