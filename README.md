# AWSLambda
Build and Deploy Serverless Applications with Java, Basic understanding of Lambda use with AWS.

**Functions as a Service**
FaaS is a new way of building and deploying server-side software, oriented around deploying individual functions or operations.

Traditional server-side software deployment:  
![image](https://github.com/user-attachments/assets/508eb629-cd64-4580-bdad-626f7184f365)

When we deploy traditional server-side software, we start with a host instance, typically a VM instance or a container. We then deploy our application, which usually runs as an operating system process, within the host. Usually our application contains code for several different but related operations; for instance, a web service may allow both retrieval and updating resources.  

But, FaaS changes this model of deployment and ownership. We strip away both the host instance and the application process from our model. Instead, we focus on just the individual operations or functions that express our application’s logic. We upload those functions individually to a FaaS platform, which itself is the responsibility of the cloud vendor and not us.  
![image](https://github.com/user-attachments/assets/88dbef8b-ff21-42e0-b949-d788336faf1c)

the FaaS platform is configured to listen for a specific event for each operation. When that event occurs, the platform instantiates the FaaS function and then calls it, passing the triggering event. Once the function has finished executing, the FaaS platform is free to tear it down. Alternatively, as an optimization, it may keep the function around for a little while until there’s another event to be processed.

FaaS as Implemented by Lambda:   
  Lambda implements the FaaS pattern by instantiating ephemeral, managed, Linux environments to host each of our function instances. Lambda guarantees that only one event is processed per environment at a time. At the time of writing, Lambda also requires that the function completes processing of the event within 15 minutes; otherwise, the execution is aborted.  
  Lambda provides an exceptionally lightweight programming and deployment model —we just provide a function, and associated dependencies, in a ZIP or JAR file, and Lambda fully manages the runtime environment.  
  Lambda is tightly integrated with many other AWS services. This corresponds to many different types of event source that can trigger Lambda functions, and this leads to the ability to build many different types of applications using Lambda.  
Lambda is a fully serverless service, as defined by our differentiating criteria from earlier, specifically:  
_Does not require managing a long-lived host or application instance_  
_Self auto-scales and auto-provisions, dependent on load_  
_Has costs that are based on precise usage, up from and down to zero usage_   
_Has performance capabilities defined in terms other than host size/count_  
_Has implicit high availability_  

_Why Lambda?_  
  The key benefit from our perspective is how quickly you can build applications with Lambda when combined with other AWS services. We often hear of companies building brand new applications, deployed to production, in just a day or two. Being able to remove ourselves from so much of the infrastructure-related code we often write in regular applications is a huge time-saver.  

_What Does a Lambda Application Look Like?_  
  Traditional long-running server applications often have at least one of two ways of starting work for a particular stimulus—they either open up a TCP/IP socket and wait for inbound connections or have an internal scheduling mechanism that will cause them to reach out to a remote resource to check for new work. Since Lambda is fundamentally an event-oriented platform and since Lambda enforces a timeout, neither of these patterns is applicable to a Lambda application. So how do we build a Lambda application?  
The first point to consider is that at the lowest level Lambda functions can be invoked (called) in one of two ways:
• Lambda functions can be called synchronously—named RequestResponse by AWS. In this scenario, an upstream component calls the Lambda function and waits for whatever response the Lambda function generates.
• Alternatively, a Lambda function may be invoked asynchronously—named Event by AWS. This time the request from the upstream caller is responded to immediately by the Lambda platform, while the Lambda function proceeds with processing the request. No further response is returned to the caller in this scenario.  

_Web API using AWS Lambda_  
An API Gateway, to provide the HTTP protocol and routing logic that we typically have within a web service.  
![image](https://github.com/user-attachments/assets/abf687ea-064b-4dd6-ab10-c3fb8c5b46b6)  
  The above diagram shows a typical API as used by a single-page web app or by a mobile application. The user’s client makes various calls, via HTTP, to the backend to retrieve data and/or initiate requests. In our case, the component that handles the HTTP aspects of the request is Amazon API Gateway—it is an HTTP server.  
  
  We configure API Gateway with a mapping from request to handler (e.g., if a client makes a request to GET _/restaurants/123_, then we can set up API Gateway to call a Lambda function named RestaurantsFunction, passing the details of the request). API Gateway will invoke the Lambda function synchronously and will wait for the function to evaluate the request and return a response.  
  
  Since the Lambda function instance isn’t itself a remotely callable API, the API Gateway actually makes a call to the Lambda platform, specifying the Lambda function to invoke, the type of invocation (RequestResponse), and the request parameters. The Lambda platform then instantiates an instance of RestaurantsFunction and invokes that with the request parameters.  
  
  The Lambda platform does have a few limitations, like the maximum timeout we’ve already mentioned, but apart from that, it’s pretty much a standard Linux environment. In RestaurantsFunction we can, for example, make a call to a database—Amazon’s DynamoDB is a popular database to use with Lambda, partly due to the similar scaling capabilities of the two services.
  
  Once the function has finished its work, it returns a response, since it was called in a synchronous fashion. This response is passed by the Lambda platform back to API Gateway, which transforms the response into an HTTP response message, which is itself passed back to the client.  
  
  Typically a web API will satisfy multiple types of requests, mapped to different HTTP paths and verbs (like GET, PUT, POST, etc.). When developing a Lambda-backed web API, you will usually implement different types of requests as different Lambda functions, although you are not forced to use such a design—you can handle all requests as one function if you’d like and switch logic inside the function based on the original HTTP request path and verb.

_File processing with Lambda_  
A common use case for Lambda is file processing. Let’s imagine a mobile application that can upload photos to a remote server, which we then want to make available to other parts of our product suite, but at different image sizes.  
![image](https://github.com/user-attachments/assets/7c7eed88-a2d6-411e-bb88-d9310afc1f90)
Mobile applications can upload files to S3 via the AWS API, in a secure fashion. S3 is Amazon’s Simple Storage Service.



