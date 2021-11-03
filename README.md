# hello-world-nodejs

## Description

A simple AWS Lambda function written and tested with Node.js 12.x and 14.x.  The function is wrapped with the SignalFX Lambda wrapper to send telemetry to the Splunk Observability cloud.

## Deployment

1. Create a new AWS Lambda function specifying Node.js 12.x or 14.x. 

2. Once the function is created, and the `signalfx-lambda-nodejs-wrapper` layer to the function.  This is done by specifying the ARN of the SignalFX Lambda Node.js Wrapper layer which can be found by visiting https://github.com/signalfx/lambda-layer-versions/blob/master/node/NODE.md.

    NOTE: Be sure to select the wrapper that matches the region your Function is deployed in.

3. Under the **Configuration** tab for your function, add the following **Environment variables**:

    * `SIGNALFX_ACCESS_TOKEN`: `<YOUR_SPLUNK_ACCESS_TOKEN>`
    * `SIGNALFX_ENDPOINT_URL`: `https://ingest.<YOUR_SPLUNK_REALM>.signalfx.com/v2/trace`

4. Under the **Test** tab, specify a event with the JSON payload as:

    ```json
    {
      "data": "This is the data",
      "options": "https://splunk.com"
    }
    ```

## Execution

Click the **Test** button.  The Log output should like something like this:

```json
START RequestId: d38ca0c5-1da8-4752-974d-e93fe235abc3 Version: $LATEST
2021-11-03T17:09:23.155Z	d38ca0c5-1da8-4752-974d-e93fe235abc3	INFO	Status: 301
2021-11-03T17:09:23.192Z	d38ca0c5-1da8-4752-974d-e93fe235abc3	INFO	Headers: {"date":"Wed, 03 Nov 2021 17:04:44 GMT","server":"Apache/2.4.37 (Red Hat Enterprise Linux) OpenSSL/1.1.1","strict-transport-security":"max-age=63072000; includeSubDomains; preload","x-frame-options":"DENY","x-content-type-options":"nosniff","location":"https://www.splunk.com/","content-length":"231","connection":"close","content-type":"text/html; charset=iso-8859-1"}
2021-11-03T17:09:23.195Z	d38ca0c5-1da8-4752-974d-e93fe235abc3	INFO	Successfully processed HTTPS response
2021-11-03T17:09:23.388Z	d38ca0c5-1da8-4752-974d-e93fe235abc3	ERROR	error: Failed to send datapoint: 404 404 page not found
 
END RequestId: d38ca0c5-1da8-4752-974d-e93fe235abc3
```
