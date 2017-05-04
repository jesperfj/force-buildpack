# Force.com DX Buildpack

Use this buildpack to run your Force.com DX project in a Heroku Dyno, either interactively or as part of Heroku CI and Heroku Continuous Delivery.

## Development

The location of the CLI installer is published in a manifest. You can get the latest one with:

```
curl -s https://developer.salesforce.com/media/salesforce-cli/manifest.json | jq -r '.builds["linux-amd64"].url'
```

(assuming you have [jq](https://stedolan.github.io/jq/) installed.)


## S3 public read

Enable public read on S3 bucket:

    BUCKET_NAME=$(heroku config:get BYODEMO_BUCKET_NAME)
    sed -e s/%%BUCKET_NAME%%/$BUCKET_NAME/ s3/public-read.json > tmp
    aws --profile dev s3api put-bucket-policy --bucket $BUCKET_NAME --policy file://tmp
    rm tmp