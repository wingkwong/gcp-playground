## Goal
Create Cloud Functions to automatically extract the text from an uploaded image, translate it into six different languages, and then store the results in Cloud Storage.

![image](https://user-images.githubusercontent.com/35857179/81255842-59bce300-9061-11ea-87b7-4a7c493f23b1.png)

## Overview
- Enable APIs.
- Create Cloud Storage buckets.
- Create Cloud Pub/Sub topics.
- Create and deploy Cloud Functions.
- Upload image to initiate test.
- View results.

## Enable APIs and Services
- From the main Google Cloud console navigation, choose APIs & Services > Library.
- Search for Cloud Functions API, and enable the service, if necessary.
- Return to the API Library page, search for Cloud Vision API, and enable it, if necessary.
- Return to the API Library page, search for Cloud Translation API, and enable it, if necessary.

## Create the Required Buckets
- From the main navigation, go to Storage > Browser.
- Choose Create bucket.
- In the Name field, enter a globally unique name (e.g., “image-to-text-input-” with a series of numbers at the end, like “247137").
- Set Default storage class to Regional.
- Leave the remaining values as their defaults, and click Create.
- Repeat steps 2–5 to create a bucket to hold the resulting translations. (The name could be something like “image-to-text-results-” with a series of numbers at the end.)

## Create the Required Pub/Sub Topics
- From the main console navigation, go to Pub/Sub > Topics.
- Click Create a topic.
- In the dialog, enter the name "image-to-text-results" for the topic.
- Click Create.
- Repeat steps 2–4 to create another topic with the name "image-to-text-translation".

## Retrieve Files from Repo and Configure
- Activate the Cloud Shell by clicking the icon in the top row.
- From the Cloud Shell, issue the following command to clone the repository for this course:
```
git clone https://github.com/wingkwong/gcp-playground
```

Change directory to the lab folder:
```
cd serverless/cloud-function/extract-and-translate-text
```

- Open the Cloud Shell Editor by clicking the pencil icon.
- Navigate to the cloud-functions-apis-lab folder, and open the two files there.
- In the main.py file, on line 34, change the RESULT_BUCKET value to match the name of the bucket intended to hold your translation results.
    ```py
    RESULT_BUCKET = "[YOUR-RESULTS-BUCKET]"
    ```
- Save the file.

## Create First Function
- In the console, navigate to the Cloud Functions dashboard.
- Click Create function.
- Apply the following settings:
    - Name: ocr-extract
    - Trigger: Cloud Storage
    - Bucket: Your storage bucket for incoming files
    - Source code: Inline editor
    - Runtime: Python 3.7
- From the Cloud Shell Editor, select all of the main.py code and copy it.
- In the main.py field of the function, delete the existing code and paste in the copied code.
- From the Cloud Shell Editor, open requirements.txt and copy all.
- In the requirements.txt field of the function, delete the existing code and paste in the copied code.
- In the Function to execute field, enter "process_image".
- Click Create.

## Create Second Function
- Navigate to the Cloud Functions dashboard.
- Click Create function.
- Apply the following settings:
    - Name: ocr-translate
    - Trigger: Cloud Pub/Sub
    - Topic: image-to-text-translation
    - Source code: Inline editor
    - Runtime: Python 3.7
- From the Cloud Shell Editor, select all of the main.py code and copy it.
- In the main.py field of the function, delete the existing code and paste in the copied code.
- From the Cloud Shell Editor, open requirements.txt and copy all.
- In the requirements.txt field of the function, delete the existing code and paste in the copied code.
- In the Function to execute field, enter "translate_text".
- Click Create.

## Create Third Function
- Navigate to the Cloud Functions dashboard.
- Click Create function.
- Apply the following settings:
    - Name: ocr-save
    - Trigger: Cloud Pub/Sub
    - Topic: image-to-text-results
    - Source code: Inline editor
    - Runtime: Python 3.7
- From the Cloud Shell Editor, select all of the main.py code and copy it.
- In the main.py field of the function, delete the existing code and paste in the copied code.
- From the Cloud Shell Editor, open requirements.txt and copy all.
- In the requirements.txt field of the function, delete the existing code and paste in the copied code.
- In the Function to execute field, enter "save_result".
- Click Create.
- Test in Cloud Shell and Review Results
- In the Cloud Shell, change directory:
    ```
    cd images
    ```

- List the contents:
    ```
    ls
    ```

- Copy the image1.png file:
    ```
    gsutil cp image1.png gs://<BUCKET_NAME>
    ```

- Copy the image2.jpg file:
    ```
    gsutil cp image2.jpg gs://<BUCKET_NAME>
    ```

From the console, navigate to Storage > Browser.

Open your bucket that contains the translated results.

Open any generated files.