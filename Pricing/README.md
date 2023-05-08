#### What exactly is serverless architecture?
Here all the application developers do is provide the code to be run and the triggers for the code. Instead of doing the normal thing of provisioning servers and memory etc. You give aws the code to run and thats it.  application developers do not package or distribute the server code to control a network socket in AWS Lambda, their applications are serverless

The triggers can be anything.
HTTP requests, Messages from a queue, Customer emails, Changes to a db record, user authentication, messages to web sockets, client-device synchroniser

The benefits of serveless architecture 
1. Its faster
2. Reduced operational costs

### Lambda Pricing

Supercharged container management services. They provide standardized execution environments to​ activate applications very quickly, and algorithms to automatically scale containers according to the workload. 
You pay for what you use. if the app isnt doing anything you dont pay for anything if suddenly you have 1 mill users lambda spins up new containers and charges you for them and then removes them when you dont need them.


In December 2019, AWS enabled users to reserve minimum capacity for Lambda functions, ensuring that there is always a certain number of processes waiting on user requests. In AWS jargon, this is called Provisioned Concurrency. With provisioned concurrency, you will also pay a fixed price for reserved instances, regardless of whether they are used or not. However, most applications won’t need to use this feature, if you design them well.


Lambda pricing allows you to release the specific features for a specific client with specific needs since you can run a seperate environment for them since it doesnt cost more than a single one.
Understanding the flow of capital on a granular level - Also great for testing, since you can create testing environments/copy that essentially are free since you are paying for requests.With Lambda, that’s now available to everyone, even a single-person team. It doesn’t cost anything more than running a single version.
its  also great in understanding your costs since its per request so you can see how much a request cost throughout its process meaning you can understand how much a single customer is consting you to serve, how much a certain feature costs etc


### Technical Constraints of AWS Lambda
1. Because AWS is in charge of scaling it can up and down VMs as it sees fit to handle requests which essentially means that you can lose state  between requests so put anything that requires state like user sessions elsewhere.
2. unknown latency - lambda prioritizes throughput which is handling many requests so that  no request waits too long which sometimes means that some requests need to wait for new lambdas to be created and some will not have to wait and will be handled immediately at all so you can't predict how long a single request will take. 
3. cold starts - when an incoming request needs to wait a few seconds for a lambda instance to be created. this has improved significantly to about hald a second
4. vpc - start up times for lambda applications connected to vpc. for those that cant avoid long initialisations you can reserve minimum capacity to reduce cold starts
5. i5 -minute execution time - lambda functions run for 3 seconds by default but can be configured to go upto 15 minutes, If a task exceeds that though Lambda will kill the VM and send back a timeout error. so long running tasks need to be split and executed in different batches or on a different service.
6. aws step functions -  instead of using Lambda to start a remote task and then waiting for it to complete, split that into two Lambdas.  Lambda 1  just sends a request to a remote service lambda 2 is kicked off by the response to the first lambda. Use AWS Step Functions to coordinate workflows that last up to one year, and invoke Lambda functions when required.
7. no direct control over processing power - you dont get to choose the processor handling your workload. or the number od cores. the only thing you get to choose is the amount of memory. 