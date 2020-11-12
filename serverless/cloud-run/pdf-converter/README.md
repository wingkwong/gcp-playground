# A serverless application with Cloud Run

Using LibreOffice to convert files to PDF format

## Architecture

![image](https://user-images.githubusercontent.com/35857179/98948445-6733b800-2531-11eb-82f9-9d0fca785849.png)

## High-Level Flow

- Upload files to Bucket
- Send a Pub/Sub notification 
- Extracts the file details from the Pub/Sub notification.
- Downloads the file from Cloud Storage to the local hard drive. This is actually not a physical disk, but a section of virtual memory that behaves like a disk.
- Converts the downloaded file to PDF.
- Uploads the PDF file to Cloud Storage. The environment variable process.env.PDF_BUCKET contains the name of the Cloud Storage bucket to write PDFs to. You will assign a value to this variable when you deploy the service below.
- Deletes the original file from Cloud Storage.


## Enable the Cloud Run API


Open the navigation menu and select APIs & Services > Library. Then in the search bar, enter in "Cloud Run" and select the Cloud Run API from the results list.

Click Enable and then hit the back button in your browser twice. Your Console should now resemble the following:

![image](https://user-images.githubusercontent.com/35857179/98948585-977b5680-2531-11eb-8749-370270329a2e.png)


Install the required packages

```
npm i
```

Use Google Cloud Build to build and deploy the REST API

```
gcloud builds submit \
  --tag gcr.io/$GOOGLE_CLOUD_PROJECT/pdf-converter
```

Go to ``Container Registry > Images`` to verify the hosted container

Deploy the application

```
gcloud beta run deploy pdf-converter \
  --image gcr.io/$GOOGLE_CLOUD_PROJECT/pdf-converter \
  --platform managed \
  --region us-central1 \
  --no-allow-unauthenticated
```

Create the environment variable $SERVICE_URL

```
SERVICE_URL=$(gcloud beta run services describe pdf-converter --platform managed --region us-central1 --format="value(status.url)")
```

Verify $SERVICE_URL

```
echo $SERVICE_URL
```

Make an anonymous POST request to your new service:

```
curl -X POST $SERVICE_URL
```

Expected Response: 

```
Your client does not have permission to get the URL
```


Invoke the service as an authorized user:
```
curl -X POST -H "Authorization: Bearer $(gcloud auth print-identity-token)" $SERVICE_URL
```


Expected Response: 

```
OK
```

## Trigger Cloud Run service when a new file is uploaded

Create a bucket for the uploaded files

```
gsutil mb gs://$GOOGLE_CLOUD_PROJECT-upload
```

one for processed files

```
gsutil mb gs://$GOOGLE_CLOUD_PROJECT-processed
```

Create a notification 

```
gsutil notification create -t new-doc -f json -e OBJECT_FINALIZE gs://$GOOGLE_CLOUD_PROJECT-upload
```

Create a new service account for Pub/Sub to trigger the Cloud Run Services

```
gcloud iam service-accounts create pubsub-cloud-run-invoker --display-name "PubSub Cloud Run Invoker"
```

Give the service account permission to invoke the service 

```
gcloud beta run services add-iam-policy-binding pdf-converter --member=serviceAccount:pubsub-cloud-run-invoker@$GOOGLE_CLOUD_PROJECT.iam.gserviceaccount.com --role=roles/run.invoker --platform managed --region us-central1
```


Look for the project number 

```
gcloud projects list
```

Create ``PROJECT_NUMBER``

```
PROJECT_NUMBER=[project number]
```

Enable your porject to create Cloud Pub/Sub authentication tokens

```
gcloud projects add-iam-policy-binding $GOOGLE_CLOUD_PROJECT --member=serviceAccount:service-$PROJECT_NUMBER@gcp-sa-pubsub.iam.gserviceaccount.com --role=roles/iam.serviceAccountTokenCreator
```

Create a Pub/Sub subscription so that the PDF converter can run whenever a message is published on the topic ``new-doc``

```
gcloud beta pubsub subscriptions create pdf-conv-sub --topic new-doc --push-endpoint=$SERVICE_URL --push-auth-service-account=pubsub-cloud-run-invoker@$GOOGLE_CLOUD_PROJECT.iam.gserviceaccount.com
```

## Testing

Copy some test files into your upload bucket:

```
gsutil -m cp gs://spls/gsp644/* gs://$GOOGLE_CLOUD_PROJECT-upload
```

Go to ``Logging`` from under the Operations section. In the log results, look or a log entry that starts with file: and click it. It shows a dump of the file data that Pub/Sub sends to your Cloud Run service when a new file is uploaded.

Go to ``Storage`` to verify the result.
