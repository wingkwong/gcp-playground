# SCP command

gcloud compute scp - copy files to and from Google Compute Engine virtual machines via scp

### To specify the project, zone, and recurse all together, run:

```
gcloud compute scp --project "your-gcp-project" --zone "us-east1-b" --recurse ~/local-directory/ gcp-instance-name:~/server-directory/
```