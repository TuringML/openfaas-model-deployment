# Model Deployment

## Installation

Modify the following parameters in `stack.yaml`:

- `gateway`. Put the correct gateway url where OpenFaaS is deployed
- `image`. Put the correct docker-registry where you'll push the image

## Test

1. Build `faas-cli build -f stack.yaml`
2. Push `faas-cli push -f stack.yaml`
3. Deploy `faas-cli deploy -f stack.yaml -e AWS_ACCESS_KEY_ID=... -e AWS_SECRET_ACCESS_KEY=... -e MODEL_BASE_PATH=s3://bucket/path/to/model -e MODEL_NAME=model -e S3_ENDPOINT=s3.eu-west-1.amazonaws.com -e AWS_REGION=eu-west-1`
4. Call `curl -d '{"inputs": []}' -X POST http://gateway-url:8080/v1/models/model:predict`