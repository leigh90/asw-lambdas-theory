#### What exactly is serverless architecture?
Here all the application developers do is provide the code to be run and the triggers for the code. Instead of doing the normal thing of provisioning servers and memory etc. You give AWS the code to run and that's it.  application developers do not package or distribute the server code to control a network socket in AWS Lambda, their applications are serverless.

The triggers can be anything.
HTTP requests, Messages from a queue, Customer emails, Changes to a db record, user authentication, messages to web sockets, client-device synchronisation

The benefits of serverless architecture 
1. Its faster
2. Reduced operational costs

### Lambda Pricing

Supercharged container management services. They provide standardized execution environments to activate applications very quickly, and algorithms to automatically scale containers according to the workload. 
You pay for what you use. if the app isn't doing anything you don't pay for anything if suddenly you have 1 mill users lambda spins up new containers and charges you for them and then removes them when you don't need them.


In December 2019, AWS enabled users to reserve minimum capacity for Lambda functions, ensuring that there is always a certain number of processes waiting on user requests. In AWS jargon, this is called Provisioned Concurrency. With provisioned concurrency, you will also pay a fixed price for reserved instances, regardless of whether they are used or not. However, most applications won’t need to use this feature, if you design them well.


Lambda pricing allows you to release the specific features for a specific client with specific needs since you can run a seperate environment for them since it doesn't cost more than a single one.
Understanding the flow of capital on a granular level - Also great for testing, since you can create testing environments/copy that essentially are free since you are paying for requests.With Lambda, that’s now available to everyone, even a single-person team. It doesn’t cost anything more than running a single version.
its  also great in understanding your costs since its per request so you can see how much a request cost throughout its process meaning you can understand how much a single customer is costing you to serve, how much a certain feature costs etc


### Technical Constraints of AWS Lambda
1. Because AWS is in charge of scaling it can up and down VMs as it sees fit to handle requests which essentially means that you can lose state  between requests so put anything that requires state like user sessions elsewhere.
2. unknown latency - lambda prioritizes throughput which is handling many requests so that  no request waits too long which sometimes means that some requests need to wait for new lambdas to be created and some will not have to wait and will be handled immediately at all so you can't predict how long a single request will take. 
3. cold starts - when an incoming request needs to wait a few seconds for a lambda instance to be created. this has improved significantly to about half a second
4. vpc - start up times for lambda applications connected to vpc. for those that cant avoid long initialisations you can reserve minimum capacity to reduce cold starts
5. 15 -minute execution time - lambda functions run for 3 seconds by default but can be configured to go up to 15 minutes, If a task exceeds that though Lambda will kill the VM and send back a timeout error. so long running tasks need to be split and executed in different batches or on a different service.
6. AWS step functions -  instead of using Lambda to start a remote task and then waiting for it to complete, split that into two Lambdas.  Lambda 1  just sends a request to a remote service lambda 2 is kicked off by the response to the first lambda. Use AWS Step Functions to coordinate workflows that last up to one year, and invoke Lambda functions when required.
7. no direct control over processing power - you don't get to choose the processor handling your workload. or the number of cores. the only thing you get to choose is the amount of memory. 

### When to use Lambda
1. Where throughput is critical and tasks are run in parallel. Typical web requests for dynamic content, involving access to a back-end database, or some user data manipulation usually fall into this category. Automatic email replies or chatbots for example.
2. Splittable longer-running tasks - Longer on-demand computational tasks that can execute in less than 15 minutes, or could be split into independent segments that take less than 15 minutes. for example file conversions, generating previews or thumbnails and running periodic reports
3. High availability tasks - tasks that need to be up most of the time but don't guarantee latency.  Like payment systems like paypal or stripe. these need to be handled reliably and quickly. 

###  When not to use Lambdas
1. Latency guaranteed tasks -  tasks that require guaranteed latency, such as in high-frequency trading systems or near-real-time control systems.  If a task must be handled in under 10 or 20 ms, it’s much better to create a reserved cluster and have services directly connected to a message broker.
2. Longer running tasks -  Tasks that take longer than 15 minutes. for example video transcoding for large files, Connecting to a socket and consuming a continuous data feed
3. Tasks that require high processing power - tasks that require a huge amount of processing power and coordination e.g video rendering. those would be better managed by reserved infrastructure with lots of cpu or gpus 
4. Tasks that don't require on-demand computation - like serving static files. use a CDN instead. 