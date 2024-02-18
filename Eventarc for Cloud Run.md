# ``` Eventarc for Cloud Run ```
## Task 1
``` bash
gcloud config set project <PROJECT-ID>
gcloud config set run/region <REGION>
gcloud config set run/platform managed
gcloud config set eventarc/location <LOCATION>
```
## Task 2
```bash
export PROJECT_NUMBER="$(gcloud projects list \
  --filter=$(gcloud config get-value project) \
  --format='value(PROJECT_NUMBER)')"
gcloud projects add-iam-policy-binding $(gcloud config get-value project) \
  --member=serviceAccount:${PROJECT_NUMBER}-compute@developer.gserviceaccount.com \
  --role='roles/eventarc.admin'
```
## Task 3
```bash
gcloud beta eventarc attributes types list
gcloud beta eventarc attributes types describe \
  google.cloud.pubsub.topic.v1.messagePublished
```
## Task 4
```bash
export SERVICE_NAME=event-display
export IMAGE_NAME="gcr.io/cloudrun/hello"
gcloud run deploy ${SERVICE_NAME} \
  --image ${IMAGE_NAME} \
  --allow-unauthenticated \
  --max-instances=3
```
## Task 5
```bash
gcloud beta eventarc attributes types describe \
  google.cloud.pubsub.topic.v1.messagePublished
gcloud beta eventarc triggers create trigger-pubsub \
  --destination-run-service=${SERVICE_NAME} \
  --matching-criteria="type=google.cloud.pubsub.topic.v1.messagePublished"
export TOPIC_ID=$(gcloud eventarc triggers describe trigger-pubsub \
  --format='value(transport.pubsub.topic)')
gcloud eventarc triggers list
gcloud pubsub topics publish ${TOPIC_ID} --message="Hello there"
```
###### Check Create a Cloud Pub/Sub event trigger lab progress
## Task 6
```bash
gcloud eventarc triggers delete trigger-pubsub
export BUCKET_NAME=$(gcloud config get-value project)-cr-bucket
gsutil mb -p $(gcloud config get-value project) \
  -l $(gcloud config get-value run/region) \
  gs://${BUCKET_NAME}/
```
###### From the Navigation menu, select IAM & Admin > Audit Logs.Search Google Cloud Storage and check the box
###### On the right hand side, click the LOG TYPE tab. Admin Write is selected by default, make sure you also select Admin Read, Data Read, Data Write and then click Save
```bash
echo "Hello World" > random.txt
gsutil cp random.txt gs://${BUCKET_NAME}/random.txt
```
###### In the Cloud Console, go to Navigation menu > Logging > Logs Explorer.Under Resource, choose GCS Bucket > [Bucket Name] > Location then choose your bucket and its location. Click Apply. Then run query

```bash
gcloud beta eventarc attributes types describe google.cloud.audit.log.v1.written
gcloud beta eventarc triggers create trigger-auditlog \
--destination-run-service=${SERVICE_NAME} \
--matching-criteria="type=google.cloud.audit.log.v1.written" \
--matching-criteria="serviceName=storage.googleapis.com" \
--matching-criteria="methodName=storage.objects.create" \
--service-account=${PROJECT_NUMBER}-compute@developer.gserviceaccount.com
gcloud eventarc triggers list
gsutil cp random.txt gs://${BUCKET_NAME}/random.txt
```


