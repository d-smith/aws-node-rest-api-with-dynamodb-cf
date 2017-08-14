# API Gateway as a Cloud Front Origin

One annoyance with the current way the AWS Gateway in implemented is how it is 
wrapped in a cloud front distribution that defeats the ability to set up a multi-region
active-standby topology where clients use a single DNS API to access the API.

By using the pattern presented here, however, you can avoid this.

The pattern can be summarized as follows:

* Create you API gateway applications in multiple regions
* Create a cloud front distribution that contains your primary regions
API gateway url as an origin
* Associate an SSL certificate in ACM with a wildcard hostname for your api domain
name with the Cloud Front distribution embedding the API url as an origin
* Associate a CNAME in the cloud front distribution that is part of the domain
indicated in the certificate. This will be the API endpoint that clients will use.
* Create a route 53 alias using the CNAME that points to the cloud front distribution.

## Create the API Gateway Apps

This sample provides a serverless application - create the DynamoDB table in the regions you with to deploy to, then deploy the serverless app via `serverless deploy`

## Create the Cloud Front Distribution

Use the provided CDN cloud formation template. This template currently uses two origins - one is an s3 bucket configured to host a web site, the other an API gateway endpoint. For
troubleshooting purposes it is useful to have content in an S3 bucket available to 
through the cloud front distribution.

For inputs, use the API gateway stage endpoint as the APIEndpoint parameter value, and 
the bucket domain as the S3SiteEndpoint arg. The API stage is also an important input.




# TODO

* API Key Synchronization between regions