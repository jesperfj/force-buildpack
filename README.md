# Force.com DX Buildpack

Use this buildpack to run your Force.com DX project in a Heroku Dyno, either interactively or as part of Heroku CI and Heroku Continuous Delivery.

## Development

The location of the CLI installer is published in a manifest. You can get the latest one with:

```
curl -s https://developer.salesforce.com/media/salesforce-cli/manifest.json | jq -r '.builds["linux-amd64"].url'
```

(assuming you have [jq](https://stedolan.github.io/jq/) installed.)


## S3 public read

The S3 bucket provisioned by byodemo isn't publicly readable. You'll need to find some way of changing that.
`s3/public-read.json` contains a policy snippet for turning on public read but you'll need to append it to 
the existing policy for the s3 bucket somehow. Here's how to sub out the bucket name:

    BUCKET_NAME=$(heroku config:get BYODEMO_BUCKET_NAME)
    sed -e s/%%BUCKET_NAME%%/$BUCKET_NAME/ s3/public-read.json > tmp

You'll then need to combine the policy in the tmp file with current policy and set the end result with:

    aws s3api put-bucket-policy --bucket $BUCKET_NAME --policy file://merged_policy
    rm tmp merged_policy
