# ``` Google Cloud Speech-to-Text API: Qwik Start ```
## Task 1
To create an API key, click Navigation menu > APIs & services > Credentials.
Then click Create credentials, select API key.

In the Navigation menu, select Compute Engine. You should see a 'linux-instance' listed in the VM instances window.
Click SSH.

Replace <YOUR_API_KEY> with API key which is created now.

```bash
export API_KEY=<YOUR_API_KEY> 
```
```bash 
touch request.json
```
```bash 
nano request.json
```
Add the following to your request.json file
```bash {
  "config": {
      "encoding":"FLAC",
      "languageCode": "en-US"
  },
  "audio": {
      "uri":"gs://cloud-samples-tests/speech/brooklyn.flac"
  }
}
```
```bash
curl -s -X POST -H "Content-Type: application/json" --data-binary @request.json \
"https://speech.googleapis.com/v1/speech:recognize?key=${API_KEY}"
```
```bash
curl -s -X POST -H "Content-Type: application/json" --data-binary @request.json \
"https://speech.googleapis.com/v1/speech:recognize?key=${API_KEY}" > result.json
```
