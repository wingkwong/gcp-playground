# Synchronize 

```
mkdir firstlevel
mkdir ./firstlevel/secondlevel
cp setup.html firstlevel
cp setup.html firstlevel/secondlevel
```

```
gsutil rsync -r ./firstlevel gs://$BUCKET_NAME_1/firstlevel
```