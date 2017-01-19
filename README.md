# docker-akamai-purge
## DESCRIPTION
This Docker image uses the httpie-edgegrid python library to code-sign the requests for the Akamai API.

## BUILD IMAGE
Before you build the image go to your Akamai account and create your API credentials [https://developer.akamai.com/introduction/Prov_Creds.html]().

Place them in the docker/.edgerc file.
```
# Change into the docker directory then run:
docker build -t biegl/akamai-purge .
```

## USAGE
This example uses the API in version v3. Currently, there is only a closed beta vor v3. v3 enables fast purge (~ 5 sec.).
Even though this example uses the V3 API, the request is internally redirected to V2. As soon as V3 is available to everybody
this request will use the correct API version. Until then, the purge request is queued, which takes longer.

The EdgeGrid plugin relies on a .edgerc credentials file that is mounted in your home directory and organized by [section] following the format below. Each [section] can contain a different credentials set allowing you to store all of your credentials in a single .edgerc file.
```
[default]
client_secret = xxxx
host = xxxx # Note, don't include the https:// here
access_token = xxxx
client_token = xxxx

[section1]
client_secret = xxxx
host = xxxx # Note, don't include the https:// here
access_token = xxxx
client_token = xxxx
````

The different sections can be accessed via the -a flag, like in the example below.

```
docker run --rm -ti --volume $(pwd):/data --workdir /data biegl/akamai-purge \
    http --auth-type edgegrid -a default: POST :/ccu/v3/invalidate/url \
    hostname=HOST objects:='["ASSETS..."]'
```
If everything work the request returns a 201 and returns a JSON object that looks something like this:
```
{
    "detail": "Request accepted.",
    "estimatedSeconds": 240,
    "httpStatus": 201,
    "pingAfterSeconds": 240,
    "progressUri": "/ccu/v2/purges/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "purgeId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "supportId": "xxxxxxxxxxxxx-xxxxxxxxx"
}
```
## TROUBLESHOOTING
"INVALID TIMESTAMP": In this case the docker container did not pick up the time from your host. Restart the docker daemon in order to get the right time.