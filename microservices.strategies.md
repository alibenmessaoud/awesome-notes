##### Right Strategies for Microservices Deployment

Multiple Service Instances per Host (Physical or VM)

> Benefits: efficient resource usage; Deployment of a service instance is fast;
>
> Challenges: limit the resources each instance utilizes; isolation if run in the same process; risks of errors while deployment

Service Instance Per Host (Physical or VM)

> Benefits: uses limited memory; no famine; cannot steal resources; take advantage load balancing and auto-scaling; deployment is simpler and reliable; 
>
> Challenges: efficient resource utilization; versions upgrade is slow;  

Service Instance per Container

> Benefits: works in isolation; allows to track how many resources are being used; lightweight and very fast to build
>
> Challenges: Service Instance per Container Pattern is still behind the VMs infrastructure; responsible for all the heavy lifting of administering the container images; Cost;

Server-less Deployment

> Benefits: Pricing; frees from any aspect of the IT infrastructure such as VMs, containers, etc.
>
> Challenges: cannot be used for long-running services; requests have to be completed within 300 seconds; to be stateless since the Lambda function might run a different instance for each request; 

