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