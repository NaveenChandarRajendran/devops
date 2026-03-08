- We can cache the application data in some location. That method is called **edge location**.
- Ex: If you webserver is hosted in the usa and user from india is going to access your webserver means there will be an high latency, that cause to load our component in slow manner.
- So at normal places the aws will have edge location, in that we can cache our application . so user from india is hittinh out webserver means it will call the edge case location if the application is cached means he will recevie the content with low latency.
- TI helps **content close to customers.**
- Districution is a method of configuration for the CloudFront.
- CloudFront distribution give default domain name
- TTL - it is the value decides content validity before an edge location request the origin
- Default TTL is 244 hours
- We can customize the cache validation also.

**Lambda functions and Cloud Front Functions**

- Allow you to run the code at edge location as close as possible
- Can be manipulate request.responnse that flow through CloudFront
- Cloud Functions are great for header manipular, URL redirects and  authorization.
- Lambda@Edge greate for long running code

**Global Accelerator**

- Where CloudFront is meant for caching data at the edge
- Where the Global acc is ROuting the users to edge location , so they can immediate connect the AWS network which is very faster so by this the user can esaly reach the apps.