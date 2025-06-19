# AWSLambda
Lambda is Amazon’s FaaS platform

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
_Does not require managing a long-lived host or application instance  
Self auto-scales and auto-provisions, dependent on load  
Has costs that are based on precise usage, up from and down to zero usage  
Has performance capabilities defined in terms other than host size/count  
Has implicit high availability  _

