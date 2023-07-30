# Exam Guide for Professional Cloud Developer


# **Section 1: Designing Highly scalable, Available, and Reliable Cloud-native applications**

## 1.1 Designing high-performing applications and APIs.

Considerations include:

### **Microservices**
---
A microservices architecture is a type of application architecture where the application is developed as a collection of services. It provides the framework to develop, deploy, and maintain microservices architecture diagrams and services independently.

Options in GCP to support Micro-Services architecture
- Deploy microservices using either the managed container service, [Google Kubernetes Engine](https://cloud.google.com/kubernetes-engine), or the fully managed serverless offering, [Cloud Run](https://cloud.google.com/run).
- Depending on the use case, [Cloud SQL](https://cloud.google.com/sql) and other Google Cloud products and services can be readily integrated to support microservices architectures.

Microservices Use cases
- **Website migration**   
A complex website that's hosted on a monolithic platform can be migrated to a cloud-based and container-based microservices platform.
- **Transactions and invoices**   
Payment processing and ordering can be separated as independent units of services so payments continue to be accepted if invoicing is not working.
- **Media content**   
Using microservices architecture, images and video assets can be stored in a scalable object storage system and served directly to web or mobile.
- **Data processing**   
A microservices platform can extend cloud support for existing modular data processing services.

Further reading: 
[What Is Microservices Architecture?  |  Google Cloud](https://cloud.google.com/learn/what-is-microservices-architecture)

### Scaling velocity characteristics/tradeoffs of IaaS (infrastructure as a service), CaaS (container as a service), PaaS (platform as a service), and FaaS (function as a service)
---
| Service | Description | Example |
| --- | --- | --- |
| Infrastructure as a service (IaaS) | For IaaS models, the service provider hosts, maintains, and updates the backend infrastructure, such as compute, storage, networking, and virtualization. You manage everything else including the operating system, middleware, data, and applications. | https://cloud.google.com/compute, https://cloud.google.com/storage |
| Platform as a service (PaaS) | Like IaaS models, for PaaS models, the service provider delivers and manages the backend infrastructure. However, PaaS models provide all the software features and tools needed for application development. You still have to write the code and manage your apps and data but do not have to worry about managing or maintaining the software development platform. | https://cloud.google.com/run, https://cloud.google.com/appengine |
| Software as a service (SaaS) | With SaaS service models, the service provider delivers the entire application stack—the complete application and all the infrastructure needed to deliver it. As a customer, all you have to do is connect to the app through the internet—the provider is responsible for everything else. | https://workspace.google.com/ |
| FaaS (function as a service) | Google Cloud Functions is a Function as a Service (FaaS) that allows engineers and developers to run code without worrying about server management. | https://cloud.google.com/functions |

![https://thecloudgirl.dev/images/vs.jpg](https://thecloudgirl.dev/images/vs.jpg)

**For Scalability**
- Serverless applications can scale quickly and you only pay for what you use (e.g. Cloud Run, Cloud Functions, App Engine).Aim for stateless microservices.
- You want to minimize start-up time for scalability.
- Create images, use containers or improve the app.
- With containers, use smaller base images (Alpine) and the builder pattern to minimize their size.
- See 5.2 GKE Autoscaling.
- GCE can autoscale managed instance groups.
- App Engine Flexible vs Standard: Standard spins up instances quicker and can have zero instances running. Flexible always has one running. Preferred standard for spiky or low traffic.
- Further Reads:    
    [Cloud Computing Products  |  Google Cloud](https://cloud.google.com/products/compute/)
    [Cloud Storage Options  |  Google Cloud](https://cloud.google.com/products/storage)
    

### **Understanding how Google Cloud services are geographically distributed (e.g., latency, regional services, zonal services)**
---
**Multi-regional, Regional and Zonal Resources**
|  | Multi-region | Regional resources | Zonal resources |
| --- | --- | --- | --- |
| Description | Multiple Google Cloud services are managed by Google to be redundant and distributed within and across regions. These services optimize availability, performance, and resource efficiency. As a result, these services require a trade-off between either latency or the consistency model. | Regional resources are resources that are redundantly deployed across multiple zones within a region | Zonal resources operate within a single zone. Zonal outages can affect some or all of the resources in that zone |
| Example | Artifact Registry, Bigtable, Cloud DLP, Cloud Healthcare API, Cloud KMS, Container Registry, Spanner, Cloud Storage, Database Migration Service, Datastore, Firestore, BigQuery | App Engine applications, Regional managed instance groups, DataFlow | Compute Engine |

**Application deployment considerations**
1. **To build highly available services and applications that can withstand zones becoming unavailable**
    Use the following:
    - [Regional resources](https://cloud.google.com/docs/geography-and-regions#regional_resources), such as App Engine applications, [regional managed instance groups](https://cloud.google.com/compute/docs/instance-groups#managed_instance_groups), or [managed multiregional resources](https://cloud.google.com/docs/geography-and-regions#multiregional_resources) such as Cloud Storage, Datastore, Firestore, or Spanner.
    - Zonal resources, such as Compute Engine virtual machines, but manage your own compute and storage redundancy across zones or across regions.

2. **To build disaster recovery capable applications that can withstand the extended loss of entire regions**
    
    For data, use one or more of the following strategies:
    
    - Use managed, multiregional storage services such as Cloud Storage, Datastore, Firestore, or Spanner.
    - Use zonal or regional resources, but snapshot data to a multiregional resource such as Cloud Storage, Datastore, Firestore, or Spanner.
    - Use zonal or regional resources, but manage your own data replication to one or more other regions.
    
    For compute, use the following strategy:
    
    - Use zonal or regional resources, such as Compute Engine or App Engine, but manually or automatically bring up your application in another region (on regional failure) referring to copies of your primary data if the data is not already in a managed, multiregional resource.
- Further reads:
    [Geography and regions  |  Documentation  |  Google Cloud](https://cloud.google.com/docs/geography-and-regions#zonal_resources)
    

### **User session management**

---

- Redis on Memorystore is generally used for user session management.
- Other Pattern is using Firestore for storing data for persistence and using Redis for speed.
- Integrate Google Sign-in with OAuth 2.0 / OpenID.

### **Caching solutions**
---

- MemoryStore
![https://thecloudgirl.dev/images/memorystore.jpg](https://thecloudgirl.dev/images/memorystore.jpg)

- Cloud CDN   
![Cloud CDN](Exam%20Guide%205c4083c3ad9f4ddc816526c2d279b732/Untitled.png)
![Cloud CDN ](Google%20Professional%20Cloud%20Developer%203bda04824918487d9efd4eaf177a78fd/Cloud%20Run%207fc77326e99147d78b43c9287d58d8db/Untitled.png)
- Cloud CDN serves cacheable content closer to the user
- Cacheable content is stored on an Edge point of presence, while dynamic content is still served by the backend service.
- CDN improves the availability and performance of the application.
- The content in the Cloud CDN store expires and the store has a limited total capacity, so content might expire earlier than expected. You enable Cloud CDN on the backend service.
- Cloud CDN has three modes that allow you to choose what content is cacheable.
    1. Cache headers: 
        - If your application responds to a web request, it needs to tell Cloud CDN using a header how long it's allowed to store the response before it needs to fetch it again.
        - This helps to control caching and expiration from the application with response headers
    2. Cache all static content: 
        - In this case, you can prevent caching by setting no cache response headers.
        - The Static content is cached by default and you can prevent caching using reponse headers
    3. Cache all for an hour: 
        - It goes without saying that you need to be very careful when enabling this mode because there is no way to override the cache from your application code. This can result in a lot of cache storage being used.
        - Cloud CDN will unconditionally cache everything in this case.
- Advantages:
    - Increased Content Availability
    - Improves website performance
    - Reduced Backend serving cost as Cloud CDN will serve more requests.

![Untitled](Google%20Professional%20Cloud%20Developer%203bda04824918487d9efd4eaf177a78fd/Cloud%20Run%207fc77326e99147d78b43c9287d58d8db/Untitled%201.png)

### **HTTP REST versus gRPC (Google Remote Procedure Call)**
---
| Areas | HTTP | gRPC |
| --- | --- | --- |
| Definitions | REST (Representational State Transfer) is an architectural style that provides guidelines for designing web APIs.It uses standard HTTP 1.1 methods like GET, POST, PUT, and DELETE to work with server-side resources. Additionally, REST APIs provide pre-defined URLs that the client must use to connect with the server. | gRPC (Remote Procedure Call) is an open-source data exchange technology developed by Google using the HTTP/2 protocol.It uses the Protocol Buffers binary format (Protobuf) for data exchange. Also, this architectural style enforces rules that a developer must follow to develop or consume web APIs. |
| Guidelines vs. Rules | REST is a set of guidelines for designing web APIs without enforcing anything. | gRPC enforces rules by defining a .protofile that must be adhered to by both client and server for data exchange. |
| Data Exchange Format | REST typically uses JSON and XML formats for data transfer. | gRPC relies on Protobuf for an exchange of data over the HTTP/2 protocol. |
| Serialization vs. Strong Typing | REST, in most cases, uses https://www.baeldung.com/java-jsonhttps://www.baeldung.com/jackson-xml-serialization-and-deserialization and conversion into the target programming language for both client and server, thereby increasing response time and the possibility of errors while parsing the request/response. | gRPC provides strongly typed messages automatically converted using the Protobuf exchange format to the chosen programming language. |
| Latency | REST utilizing HTTP 1.1 requires a TCP handshake for each request. Hence, REST APIs with HTTP 1.1 can suffer from latency issues. | gRPC relies on HTTP/2 protocol, which uses multiplexed streams. Therefore, several clients can send multiple requests simultaneously without establishing a new TCP connection for each one. Also, the server can send push notifications to clients via the established connection. |
| Browser Support | REST APIs on HTTP 1.1 have universal browser support. | gRPC has limited browser support because numerous browsers (usually the older versions) have no mature support for HTTP/2. So, it may require gRPC-web and a proxy layer to perform conversions between HTTP 1.1 and HTTP/2. Therefore, at the moment, gRPC is primarily used for internal services. |
| Code Generation Features | REST provides no built-in code generation features. However, we can use third-party tools like Swagger or Postman to produce code for API requests. | https://www.baeldung.com/grpc-introduction#1-using-protocol-buffer-compilerhttps://www.baeldung.com/grpc-introduction#1-using-protocol-buffer-compilerhttps://www.baeldung.com/grpc-introduction#1-using-protocol-buffer-compiler, comes with native code generation features, compatible with several programming languages. |
| Application areas | REST is handy in integrating microservices and third-party applications with the core systems. | IoT systems that require light-weight message transmission, mobile applications with no browser support, and applications that need multiplexed streams. |

### **Designing API services with API Gateway and Cloud Endpoints**
---
![Untitled](Exam%20Guide%205c4083c3ad9f4ddc816526c2d279b732/Untitled%201.png)

![https://storage.googleapis.com/gweb-cloudblog-publish/images/2_api_uses.png.max-900x900.jpg](https://storage.googleapis.com/gweb-cloudblog-publish/images/2_api_uses.png.max-900x900.jpg)

### **Loosely coupled asynchronous applications (e.g., Apache Kafka, Pub/Sub, Eventarc)**
---
- An *event-driven architecture* is a software design pattern in which microservices react to changes in state, called *events*.
- In an event-driven system, events are generated by **event producers**, ingested and filtered by an **event router** (or broker), and then fanned out to the appropriate **event consumers** (or sinks).

- When to use event-driven architectures   
    Consider the following usages when designing your system.
    - **To monitor and receive alerts** for any anomalies or changes to storage buckets, database tables, virtual machines, or other resources.
    - **To fan out a single event** to multiple consumers. The event router will push the event to all the appropriate consumers, without you having to write customized code. Each service can then process the event in parallel, yet differently.
    - **To provide interoperability between different technology stacks** while maintaining the independence of each stack.
    - **To coordinate systems and teams** operating in and deploying across different regions and accounts. You can easily reorganize ownership of microservices. There are fewer cross-team dependencies and you can react more quickly to changes that would otherwise be impeded by barriers to data access.

- Options In GCP for creating Loosely Coupled Asynchronous Application
    - Eventarc lets you build [event-driven architectures](https://cloud.google.com/eventarc/docs/event-driven-architectures) without having to implement, customize, or maintain the underlying infrastructure. Eventarc offers a standardized solution to manage the flow of state changes, called *events*, between decoupled microservices. Read more: [Event Arc](https://cloud.google.com/eventarc/docs/overview)
    - Pub/Sub is an asynchronous and scalable messaging service that decouples services producing messages from services processing those messages. It is a is a fully-managed real-time messaging service that allows you to send and receive messages between independent applications. Read More:
        [Pub/Sub](https://www.notion.so/Pub-Sub-76c61a90530c45f39b2793ba1aa055ec?pvs=21)

### **Instrumenting code to produce metrics, logs, and traces**
---
![patterns-diagram.png](Exam%20Guide%205c4083c3ad9f4ddc816526c2d279b732/patterns-diagram.png)

| Option | Advantage | Disadvantage |
| --- | --- | --- |
| Cloud Logging client libraries | - You can route logs directly to Cloud Logging API. - Some languages can output logs to `stdout` and `stderr` by using the library. | Application crashes break log ingestion. |
| Legacy Logging agent | - The agent uses Fluentd to collect logs. - You can retain logs in your local environment. - You might be able to recover logs from application crashes | The agent is currently supported but is not under active development. |
| Ops Agent | - The Ops Agent supports the ingestion of both logs and metrics using stable open source technologies: Fluent Bit for log collection and the OpenTelemetry Collector for metric collection. - You can collect both logs and metrics from many common applications; see https://cloud.google.com/stackdriver/docs/solutions/agents/ops-agent/third-party. - You can retain logs in your local environment.- You might be able to recover logs from application crashes. - The Ops Agent is under active development. | N/A  |
| stdout and stderr automatic log ingestion | - This process is a common way to emit logs to local environments. - You can use arbitrary logging libraries. - You might be able to recover logs from application crashes. | Not all environments automatically route logs to Logging. |

### **Graceful shutdown of applications on platform termination**
---
- Application code can perform the following tasks during "graceful shutdown"
    - **Flush monitoring data:** If you use [Cloud Trace](https://ahmet.im/blog/minimal-init-process-for-containers/) or upload metrics from your application, you can develop a signal handler and call the function that flushes out the trace spans collected before your container quits and loses these in-memory trace spans that are not uploaded.
    - **Log termination of your container:** By logging the termination event of the container, you can refer to your application logs to see when a specific container instance has started and exited, and get full visibility into the lifecycle of individual container instances.
    - **Close file descriptors or database connections:** Some abruptly quit connections can confuse the connected servers and cause them to keep connections open for a long time than gracefully disconnecting
- Service specific actions
    - **Graceful shutdowns in Cloud Run**    
        1. When a container instance is shut down on Cloud Run, a SIGTERM signal will be sent to the container and your application will have [10 seconds](https://cloud.google.com/run/docs/reference/container-contract#instance-shutdown) to exit. 
        2. If the container does not exit by then, a SIGKILL signal (which you cannot capture) will be sent to abruptly close your application. 
        3. If you choose not to write a signal handler for SIGTERM, your process is terminated instantly.

    - **Graceful shutdown in GKE**    
        Once Kubernetes has decided to terminate your pod, a series of events take place. Let's look at each step of the Kubernetes termination lifecycle.

        - **1 - Pod is set to the “Terminating” State and removed from the endpoints list of all Services**    
            At this point, the pod stops getting new traffic. Containers running in the pod will not be affected.
            
        - **2 - preStop Hook is executed**
            The [preStop Hook](https://kubernetes.io/docs/concepts/containers/container-lifecycle-hooks/#hook-details) is a special command or http request that is sent to the containers in the pod.
            If your application doesn't gracefully shut down when receiving a SIGTERM you can use this hook to trigger a graceful shutdown. Most programs gracefully shut down when receiving a SIGTERM, but if you are using third-party code or are managing a system you don't have control over, the preStop hook is a great way to trigger a graceful shutdown without modifying the application.
            
        - **3 - SIGTERM signal is sent to the pod**
            At this point, Kubernetes will send a SIGTERM signal to the containers in the pod. This signal lets the containers know that they are going to be shut down soon.         
            Your code should listen for this event and start shutting down cleanly at this point. This may include stopping any long-lived connections (like a database connection or WebSocket stream), saving the current state, or anything like that.
            Even if you are using the preStop hook, it is important that you test what happens to your application if you send it a SIGTERM signal, so you are not surprised in production!
            
        - **4 - Kubernetes waits for a grace period**
            At this point, Kubernetes waits for a specified time called the termination grace period. By default, this is 30 seconds. It's important to note that this happens in parallel to the preStop hook and the SIGTERM signal. Kubernetes does not wait for the preStop hook to finish.
            If your app finishes shutting down and exits before the terminationGracePeriod is done, Kubernetes moves to the next step immediately.
            If your pod usually takes longer than 30 seconds to shut down, make sure you increase the grace period. You can do that by setting the terminationGracePeriodSeconds option in the Pod YAML. For example, to change it to 60 seconds:
            
            ![https://storage.googleapis.com/gweb-cloudblog-publish/images/gcp-terminationGracePeriodSecondsfmw1.max-400x400.PNG](https://storage.googleapis.com/gweb-cloudblog-publish/images/gcp-terminationGracePeriodSecondsfmw1.max-400x400.PNG)
            
        - **5 - SIGKILL signal is sent to pod, and the pod is removed**
            If the containers are still running after the grace period, they are sent the SIGKILL signal and forcibly removed. At this point, all Kubernetes objects are cleaned up as well.
            
    - Cloud functions: Delete any temporary files, as these are stored in memory and can be carried over between runs.

### **Writing fault-tolerant code**
---
Some of the patterns to help write Fault-tolerant code are:
- Timeouts
- Retries
- Circuit Breaker
- Deadlines
- Rate limiters

## 1.2 Designing secure applications.
Considerations include:
### Implementing data lifecycle and residency requirements relevant to applicable regulations
---
- **Data residency**
    - Data residency describes where your data is stored at rest.
    - Data residency requirements vary based on systems design objectives, industry regulatory concerns, national law, tax implications, and even culture.
    - Controlling data residency starts with the following:
        - Understanding the type of your data and its location.
        - Determining what risks exist to your data, and what laws and regulations apply.
        - Controlling where data is or where it goes.   

- **Data lifecycle**
    - Cloud Storage:
        - A [retention policy](https://cloud.google.com/storage/docs/bucket-lock) that specifies a retention period can be [placed on a bucket](https://cloud.google.com/storage/docs/using-bucket-lock#set-policy). An object in the bucket cannot be deleted or replaced until it reaches the specified age.
        - An [object hold](https://cloud.google.com/storage/docs/object-holds)can be [placed on individual objects](https://cloud.google.com/storage/docs/holding-objects#set-object-hold)to prevent anyone from deleting or replacing the object until the hold is removed.
        - [Object Versioning](https://cloud.google.com/storage/docs/object-versioning) can be enabled on a bucket in order to retain older versions of objects. When the *live* version of an object is deleted or replaced, it becomes *noncurrent* if versioning is enabled on the bucket. If you accidentally delete a live object version, you can [restore the noncurrent version](https://cloud.google.com/storage/docs/using-versioned-objects#restore) of it back to the live version.
        - Retention policies and [Object Versioning](https://cloud.google.com/storage/docs/object-versioning) are mutually exclusive features in Cloud Storage: for a given bucket, only one of these can be enabled at a time. Any versioned objects remaining in a bucket when you apply a retention policy are also protected by the retention policy.

### Security mechanisms that protect services and resources
---
**There are three ways to filter inbound requests to a Cloud Run service.**    
**1.  Cloud Armor**
>       ⭐ Google Cloud Armor allows to use content-based rules to filter inbound requests.
![Untitled](Google%20Professional%20Cloud%20Developer%203bda04824918487d9efd4eaf177a78fd/Cloud%20Run%207fc77326e99147d78b43c9287d58d8db/Untitled%202.png)
- Google Cloud Armor filters traffic with content-based rules that inspect the properties of the incoming request.
- For example, you can add conditions based on an incoming request IP address, IP range, region code, or request headers.
- The Requests that come in through the default and custom domain of a Cloud Run service do not go through Google Cloud Armor because they are coming from the internal environment within the network your application is running in.
- Policy Rule:
    - A policy rule is the main building block of a Google Cloud Armor security policy.
    - A policy rule can be very simple or complex.
    - A simple policy rule is "deny request from a specific IP.”
    - More complex rules are the custom conditions that match up various attributes of the incoming request, such as the URL path, request method, or request header values.
    - Preconfiguration rules also exist to mitigate the following attacks: cross-site scripting, SQL injection attacks, local file inclusion attacks, remote file inclusion attacks, and remote code execution attacks.
    - Policy rules are a part of every security policy, and a security policy is attached to a backend service.
    - You can attach the policy to one or multiple backend services.
- Preconfigured rules exists to mitigate the following attacks:
    - cross site scripting,
    - SQL injection attacks,
    - local file inclusion attacks,
    - remote file inclusion attacks
    - remote code execution attacks

**2. Ingress settings**

- use the ingress to filter traffic based on its source.
- It is a course filter, not a granular one and it supports only three different source types
    - External Source
    - Resource on service project's network
    - Resource on shared VPC
- There are three modes in the setting
    - Internal Only:
        >       ⭐ Make your service private, which allows only requests from resources in the VPC of the same project.      
        - The most restrictive setting as it only allows requests that originate from resources on a virtual private cloud or VPC.
        - This mode does not allow incoming traffic from Google services such as Pub/Sub or cloud tasks, as those do not run in the VPC.
        - This mode also prevents requests from other Cloud Run services to your service, unless you use the serverless VPC access connector
    - Internal and Load Balancing
        >       ⭐ Allow requests that come through the Global HTTPS load balancer, as well as internal requests
        
        - The main use case for this setting is to make your service available for requests through the HTTPS load balancer and disable the default URL.
        - This can be useful if you've added Google Cloud Armor and wants to prevent an attacker from finding out the default domain of a service, and using that to bypass the security policy.
        - Please note that it's not possible to allow requests exclusively through the HTTPS load balancer. You must include the VPC source as well.
    - Ingress Proxy
        >       ⭐ Allow all incoming requests, enabling the default run .app URL.
        - Ingress proxy is to allow requests from all origins.
        - This is the default setting.

It's important to realize that the most manage Google Cloud services, including Cloud Run, Pub/Sub, cloud tasks, and workflows are not considered internal because they run outside of the VPC Network.

**3. IAM**

- lets you filter traffic based on the identity of the sender.
- All requests to a Cloud Run service pass through and are evaluated by IAM. To control this type of incoming request, you will need to configure IAM with an IAM policy that attaches to the Cloud Run service.
- Use Case
    - it lets you allow access to specific service accounts only.
    - you can allow unauthenticated access, which is basically making your service publicly accessible without a login being required.

![Untitled](Google%20Professional%20Cloud%20Developer%203bda04824918487d9efd4eaf177a78fd/Cloud%20Run%207fc77326e99147d78b43c9287d58d8db/Untitled%203.png)

**Filtering Outbound requests to a Cloud Run**
- By default, from a Cloud Run container, you can't connect with private resources that only have an internal IP address in the VPC.
- Solution: VPC Access Connector
    - You can create a VPC Access connector to access internal IP addresses.
    - A VPC Access connector is a resource in the VPC that acts as a router to forward traffic from Cloud Run services.
    - To use the connector, you'll need to configure your Cloud Run service to use it.
    - Note that you can only attach one connector to each service.
    - The VPC Access connector is not serverless, hence billed even when not in use.
    - To control outbound traffic from your service, you can set the VPC egress setting and route all traffic through the VPC Connector.
    - If you enable this, all outbound connections from the service are routed to your VPC network even if they have an external destination. The connections will adhere to the network's firewall, DNS, and routing rules


### **Security mechanisms that secure/scan application binaries and manifest**
---
- **Binary Authentication**   
    - Binary Authorization is a deploy-time security control that ensures only trusted container images are deployed on Google Kubernetes Engine (GKE) or Cloud Run.
    - With Binary Authorization, you can require images to be signed by trusted authorities during the development process and then enforce signature validation when deploying.
    - By enforcing validation, you can gain tighter control over your container environment by ensuring only verified images are integrated into the build-and-release process.
    - Binary Authorization integrates with the GKE and Cloud Run control plane to allow or block image deployment based on the policies that you define.
      
- **Security Command Center**
    - Built-in security and risk management solution for Google Cloud.
    - **Advantages:**
        - **Improve security posture:** Identify security misconfigurations and vulnerabilities in your Google Cloud environment and resolve them with actionable recommendations.
        - **Detect threats:** Uncover threats with specialized detectors built into the Google Cloud infrastructure to detect data exfiltration, compromised identities, cryptocurrency mining, and more.
        - **Assess and manage risk:** Use attack path simulation to discover and shut down possible pathways that adversaries can use to access and compromise cloud resources.
- Further reads
    [Security Command Center  |  Google Cloud](https://cloud.google.com/security-command-center)
    

### **Storing, accessing, and rotating application secrets and keys (e.g., Secret Manager, Cloud Key Management Service)**
---
- Secret Manager works well for storing configuration information such as database passwords, API keys, or TLS certificates needed by an application at runtime.
- A key management system, such as [Cloud KMS](https://cloud.google.com/kms), allows you to manage cryptographic keys and to use them to encrypt or decrypt data. However, you cannot view, extract, or export the key material itself.
- Key rotation needs to be performed periodically (limits exposure, shut down keys if they're exposed).

### **Authenticating to Google Cloud services (e.g., application default credentials, JSON Web Token (JWT), OAuth 2.0) | End-user account management and authentication using Identity Platform | ● IAM roles for users, groups, and service accounts | IAM roles for users, groups, and service accounts**
---
**IAM - Identity and Access Management**
>       ⭐ Permissions to resources follow IAM members

- Google account
- Service account
- Google group
- Google Workspace domain
- Cloud Identity domain ⇒ virtual ground

**Roles**
- Primitive
- Pre-defined
- Custom

**Cloud IAM API methods**
- setIAMPolicy()
- getIAMPolicy()
- testIamPermissions()

**Best Practices when using Cloud IAM**
1. Follow principle of least privilege.
2. Rotate service account keys.
3. Manage user-managed service keys.
4. Don't check in service account keys.
5. Use Cloud audit logging and export logs to Cloud Storage.
6. Set org-level IAM policies.
7. Grant roles to Google group whenever possible.

**Policy**
- An Identity and Access Management (IAM) policy specifies access controls for Google Cloud resources
- A Policy is collection of bindings.
- A binding binds one or more members or principals to a single role.    
    ```json
    {
      "version": integer,
      "bindings": [
        {
    			// object (Binding)
    		  "role": string,
    		  "members": [
    		    string
    		  ],
    		  "condition": {
    		    object (Expr)
    		  }
      ],
      "auditConfigs": [
        {
          object (AuditConfig)
        }
      ],
      "etag": string
    }
    ```
    
**Service account**
**Overview**
- Service Accounts belongs to the application or VM
- Used by the application to call the Google API or services, so users aren't directly involved
- Identified by their unique email address and are associated with a key pair
- Can have up to 10 keys associated with them to facilitate key rotation (done daily by Google)
- Enables authentication and authorization as specific IAM roles can be assigned to a service account
**Process for allowing Application to use service account**
- Create the service account
- Generate  creds file and download the file
- Set environment variable GOOGLE_APPLICATION_CREDENTIALS with file path of service account creds file
- Authenticate with code using default creds

**Order followed by Application Default Credentials (ADC) for checking credentials**
1. checks for GOOGLE_APPLICATION_CREDENTIALS env variable
2. checks for default service account of the service like Compute engine, App Engine
3. if 1 and 2 not found, throws an error.

**External Keys for use from outside GCP**   
- Can be created for use outside GCP
- User is responsible for security of the private key and other management operation like key rotation
- They are manageable through
    - IAM API
    - gcloud command line tool
    - Service Account page in the GCP Console.

**OAuth 2.0**
- It can be used to access resources on behalf of users
- Application will request access to the resources → the users will be prompted for consent → if consent is provided, the application can request creds from authorization server → Application can then use these creds on behalf of user to access the resources
- Use Case
    - Application needs to access BQ dataset that belong to users
    - Application needs to authenticate as a user to create projects on their behalf

**Identity- Aware Proxy (IAP)**
>     💡 Identity Platform is a customer identity and access management platform for adding identity and access management to applications.
- This controls access to the cloud applications running on Google Cloud
- IAP verifies a user's identity and determines whether that user should be allowed to access the application. IAP allows you to establish a central authorization layer for applications accessed by HTTPS. IAP lets you adopt an application-level access control model instead of relying on network level firewalls. Applications and resources protected by IAP can only be accessed through the proxy by users and groups with the correct Cloud IAM role.
- When you grant a user access to an application or resource by IAP, they're subject to the fine-grained access controls implemented by the product in use without requiring a VPN.
- IAP performs authentication and authorization checks when a user tries to access an IAP-secured resource.

![IAP Process flow (How it works)](Google%20Professional%20Cloud%20Developer%203bda04824918487d9efd4eaf177a78fd/Untitled.png)


**IAP Process flow (How it works)**    
![Reference Architecture](Google%20Professional%20Cloud%20Developer%203bda04824918487d9efd4eaf177a78fd/Untitled%201.png)
- Precautions while using IAP
    - Configure firewall and load balancers to protect against traffic that doesn't come from serving infrastructure.
    - Use Signed headers or AppEngine standard env Users API.
- IAP provides authentication as a service by providing:
    - Access via a set of SDKs
    - Federated login that integrates with many common providers like Google, Apple and Twitter
    - Sign-up and sign-in for your end user's application
- Firebase Authentication is similar service as IAP.
- IAP provides some advanced controls like SAML authentication to enterprise customers.

**IAP vs Firebase Authentication**
1. **Authentication (Difference)**
- Both allow you to easily sign users into your apps by providing backend services, SDKs, and UI libraries.
- But **Identity Platform** offers additional capabilities designed for enterprise customers, such as Open ID Connect in SAML authentication, multi-tenancy support, Identity-Aware Proxy integration and a 99.95% uptime SLA.
2. **Compatibility (Similarity)**
- Identity Platform is fully compatible with both Google Cloud and Firebase products. If you are currently using Firebase authentication and you upgrade to Identity Platform, your app will continue to work with existing Firebase services.
3. **SDK (Similarity)**
- Identity Platform and Firebase authentication both support a collection of client and Admin SDKs.
- To preserves backward compatibility, the SDKs occasionally use Firebase branding and naming conventions.

### **Securing service-to-service communications (e.g., service mesh, Kubernetes Network Policies, and Kubernetes namespaces)**
---
- **Service Mesh**
    - **Service mesh** is a technology pattern that can be applied to a microservice-based system to manage networked communication between services.
    - With a service mesh, the networking functionality is decoupled from the service's application logic, which means it can be managed independently.
    
- **GKE Network Policies**    
    - Network policy enforcement lets you create Kubernetes [Network Policies](https://kubernetes.io/docs/concepts/services-networking/network-policies/) in your cluster.
    - Network policies create Pod-level firewall rules that determine which Pods and Services can access one another inside your cluster.
    - Advantages     
        1. Defining network policy helps you enable things like [defense in depth](https://en.wikipedia.org/wiki/Defense_in_depth_(computing)) when your cluster is serving a multi-level application.     
            For example, you can create a network policy to ensure that a compromised front-end service in your application cannot communicate directly with a billing or accounting service several levels down.
        2. Network policy can also make it easier for your application to host data from multiple users simultaneously. 
            For example, you can provide secure multi-tenancy by defining a tenant-per-namespace model. In such a model, network policy rules can ensure that Pods and Services in a given namespace cannot access other Pods or Services in a different namespace.
            
- **Kubernetes Namespace**   
    - Kubernetes allows you to abstract a single physical cluster into multiple clusters known as 'namespaces'.     
    - Namespaces provide scope for naming resources such as Pods, Deployments, and controllers.     
    - Object names need only be unique within a namespace ,not across all namespaces.     
    - Namespaces also let you implement resource quotas across the cluster. These quotas define limits for resource consumption within a namespace.     
    - There are three initial namespaces in a cluster.     
        - **Default:** The first is a default namespace, for objects with no other namespace defined. Your workload resources will use this namespace by default.
        - **kube-system:** kube-system namespace for objects created by the Kubernetes system itself. When you use the kubectl command, by default, items in the kube-system namespace are excluded, but you can choose to view its contents explicitly.
        - **kube-public:** kube-public namespace for objects that are publicly readable to all users. kube-public is a tool for disseminating information to everything running in a cluster.
            ![Untitled](Google%20Professional%20Cloud%20Developer%203bda04824918487d9efd4eaf177a78fd/Google%20Kubernetes%20Engine%209008c6afa75b40e19cb02a5035d6b72b/Untitled.png)
            
        **Best Practice:**       
        - Use Namespace in command line kubectl command
        - You can also define in the yaml file, but it is less flexible, hence should be avoided.

### **Running services with least privileged access (e.g., Workload Identity)**
---
- Workload Identity is the recommended way for your workloads running on Google Kubernetes Engine (GKE) to access Google Cloud services in a secure and manageable way.
- Workload Identity allows workloads in your GKE clusters to impersonate Identity and Access Management (IAM) service accounts to access Google Cloud services.

### **Certificate-based authentication (e.g., SSL, mTLS)**
---
mTLS
- A two-way authentication protocol (mutual TLS).
- Possible to use mTLS as opposed to service account keys.
- Automatically done by Istio between GKE pods.

SSL
- An encryption protocol required for HTTP(S) and SSL load balancers. Can be Google or self-managed.
- [Docs: SSL certificates overview](https://cloud.google.com/load-balancing/docs/ssl-certificates).

## 1.3 Managing application data.
Considerations include:

### Defining database schemas for Google-managed databases (e.g., Firestore, Cloud Spanner, Bigtable, Cloud SQL)
---

| Database | Schema Characteristics |
| --- | --- |
| Firestore | - ‘Collections' are a group of related ‘Documents', which should be kept lightweight. - To do this you can define hierarchical subcollections, which are collections associated with a document. - https://firebase.google.com/docs/firestore/data-model. |
| Datastore | - Data objects are called entities - One entity contains following properties     - Namespace    - Entity kind     - Identifier (either a string or numeric ID)     - Ancestor Path (optional)        - Operations on one or more entities are called transactions |
| Cloud SQL | RDBMS |
| Cloud Spanner | - Extended DDL, with some extra features including table interleaving. - Create second indexes and interleave related records for faster lookups. - https://cloud.google.com/spanner/docs/schema-and-data-mode |
| Bigtable | NoSQL (wide, data fields are optional). - Broken into column families. - one key per table. You can put multiple keys into this one key and separate them with hashes for more distributed reads. - https://cloud.google.com/spanner/docs/schema-and-data-mode. |

### Defining a data storage key structure for high-write applications
---
- Cloud Storage: don't name objects similarly.
- Cloud Bigtable: one key per table. You can put multiple keys into this one key and separate them with hashes for more distributed reads.
- Cloud Spanner: Create second indexes and interleave related records for faster lookups.
- CloudSQL: Create secondary indexes or clustered indexes.

### Choosing data storage options based on use case considerations, such as:
- **Time-limited access to objects**
    - [Signed URLs](https://cloud.google.com/storage/docs/access-control/signed-urls): GCS URLs that provide temporary acccess.

- **Data retention requirements**
    - Use the GCS storage tiers and Object Lifecycle Management rules to move between them automatically.       
        - Storage classes in Cloud Storage    
            ![Untitled](Google%20Professional%20Cloud%20Developer%203bda04824918487d9efd4eaf177a78fd/Cloud%20Storage%2085f98fbda2a7481ca9d12c1e05718d37/Untitled.png)
            
    - Object holds (for retention) and policy locks (that can't be removed).

- **Structured versus unstructured data**
![https://thecloudgirl.dev/images/storageoptions.jpg](https://thecloudgirl.dev/images/storageoptions.jpg)

![Untitled](Exam%20Guide%205c4083c3ad9f4ddc816526c2d279b732/Untitled%202.png)
    
- **Strong versus eventual consistency**
    - Cloud Storage
        - Operations which are strongly consistent
            - Read-after write
            - Read-after-metadata-update
            - Read-after-delete
            - Bucket listing
            - Object listing
            - Granting access to resources
        - Operations which are eventually consistent
            - Revoking access from object
            - Accessing publicly readable cached objects
    - Strongly Consistency databases
        - Cloud Spanner
        - Cloud SQL
        - Datastore
            | Datastore API | Read of entity value | Read of index |
            | --- | --- | --- |
            | https://cloud.google.com/appengine/docs/python/datastore/queries | Eventual consistency | Eventual consistency |
            | https://cloud.google.com/appengine/docs/python/datastore/queries#keys-only_queries | N/A | Eventual consistency |
            | https://cloud.google.com/appengine/docs/python/datastore/queries#ancestor_queries | Strong consistency | Strong consistency |
            | https://cloud.google.com/appengine/docs/python/datastore/entities#Python_Retrieving_an_entity (get()) | Strong consistency | N/A |

- Data access patterns  
---
![Untitled](Exam%20Guide%205c4083c3ad9f4ddc816526c2d279b732/Untitled%203.png)
    
- Online transaction processing (OLTP) versus data warehousing

# **Section 2: Building and Testing Applications**
## 2.1 Setting up your local development environment.
Considerations include:
### **Emulating Google Cloud services for local application development**
---
**Emulator**
- Command to install and manage emulators   
    ```bash
    **gcloud beta emulators**
    ```
- The emulators enables to switch from using local emulators to GCP without changing application code
- Currently available emulators:
    - Cloud BigTable
    - Datastore
    - Firestore
    - Pub/Sub
    - Cloud Spanner

**Using the Google Cloud Console, Google Cloud SDK, and Cloud Shell tools**
-  Cloud SDK: For interacting with Google Cloud Services, there are following methods:
    1. Command Line tools
        - They provide tools to manage most of the Google Cloud Services by acting as wrapper around Cloud APIs
        - They can be used on command line or in automated Scripts
        - GCP includes three command line tools:    
            - gcloud   
            - bq
            - gsutil
    2. Client libraries
        - Provides most optimized approach to connect with Google Services
        - They are simpler to use than making direct API calls
        - It is latest and recommended ways to make request for making Cloud API requests from applications.
        - Some of the cloud Libraries can receive performance benefits by using gRPC APIs internally.

### **Using developer tooling (e.g., Cloud Code, Skaffold)**
---
#### **Cloud Code**    
**Introduction**    
    - The plugin makes it easier to create, deploy and debug cloud native applications.    
    - Available for VSCode, Jetbrains IDE and Cloud Shell Editor.    
    - Streamlines common workflows inside IDE.    
    - Integrates with secret manager to securely store sensitive data.    
    - Manages Cloud APIs and Cloud Client libraries.    
        - Can browse library specific to the programming language being used.    
        - Can copy code snippets that using cloud APIs.        
**Cloud Code for Kubernetes**    
    - Run and debug Kubernetes applications in a local cluster or on GKE.    
    - Visualize and manage Kubernetes resources using Kubernetes Explorer.    
**Cloud Code for Cloud Run**    
    - Can be used to Run and debug code locally    
    - Can deploy apps to Cloud Run from IDE    
    - Can manage Cloud Run Services with Cloud Run Explorer to manage services from IDE.    

## 2.2 Building.
Considerations include:

### **Source control management**

---

- Store code with git in Cloud Source Repositories.
- You can set up Cloud Build triggers to run when changes are pushed to certain branches.

### **Creating secure container images from code**

---

- Add security scanning to the CI process.
- You can add this as a Cloud Build step and call Container Registry on-demand, check the severity level and fail if it's too high.Use Google's managed base images, and build on top of those.
- Jib: creates optimized Docker images from Java applications

### **Developing a continuous integration pipeline using services (e.g., Cloud Build, Artifact Registry) that construct deployment artifacts**
---
![Untitled](Google%20Professional%20Cloud%20Developer%203bda04824918487d9efd4eaf177a78fd/Untitled%202.png)

Using Container Registry with Cloud Build, one can create Build pipelines that are automatically triggered when you commit code to a repository.

**Cloud build**
- A trigger type specifies whether a Build should be triggered based on commits to a particular branch in a repository or commits that contain a particular tag.
- A Build configuration file needs to be created that specifies the steps in the Build pipeline.
    - Steps are analogous to commands or scripts that you execute to Build your application.
    - Each Build step is a Docker container that's invoked by Cloud Build when the Build is executed.
    - The step name identifies a container to invoke for the Build step.
    - The images attribute contains the name of the container image to be created by this Build configuration.
    - The Build configuration can be specified in a Yaml or JSON format.
- Cloud Build enables you to specify different types of Source Repositories, tag container images to enable searches, and create Build steps that perform operations such as downloading and processing data without even creating a container image.
- Cloud Build mounts your source code into the /workspace directory of the Docker container associated with the Build step.
- In Container Registry, you can view the status and history of builds. Cloud Build also publishes Build status notifications to Pub Sub.
- You can subscribe to these notifications to take action based on Build status or other attributes.

**Container Registry**
- Spinnaker: CD for compute, containers, multi-cloud. E.g. set up a process for canary deployments.
- Tekton: CI/CD for k8s.
- Anthos Config Manager: matches the k8s environment to the config in a repo.
- Jenkins: Older kitchen sink.

### **Code and test build optimization**
---
- Metrics include: proportion of commits that trigger a build, test pass rate, build fix time, availability of feedback.
- Improvements include: increasing velocity of deployments/merges, automation, build duration (< 10 minutes), broken builds/tests, pulling in code outside the repo.

## 2.3 Testing.
Considerations include:

### **Unit testing (e.g., emulators)**
---
- Test a component in isolation.
- Find easy-to-spot errors.
- Automate your tests!

### **Integration testing**
---
- Group components together, include dependencies.

### **Performance testing**
---
- Measure latency.

### **Load testing**
---
- Place under heavy load, maybe for a long period and test scalability.

### Failure testing/chaos engineering

# **Section 3: Deploying Applications**

## 3.1 Adopting appropriate feature rollout strategies.
Considerations include:

### Deployment Strategies
---
| Deployment or testing pattern | Description | Zero downtime | Real production traffic testing | Releasing to users based on conditions | Rollback duration | Impact on hardware and cloud costs |
| --- | --- | --- | --- | --- | --- | --- |
| Recreate | Version 1 is terminated, and Version 2 is rolled out. | ❌ | ❌  | ❌  | Fast but disruptive due to downtime | No extra setup required |
| Rolling update | Version 2 is gradually rolled out and replaces Version 1.  | ✅ | ❌  | ❌  | Slow | Can require extra setup for surge upgrades |
| Blue/green | Version 2 is released alongside version 1; the traffic is switched to Version 2 after it is tested. | ✅ | ❌ | ❌ | Instant | Need to maintain blue and green environments simultaneously |
| Canary | Version 2 is released to a subset of users, followed by a full rollout. | ✅ | ✅ | ❌  | Fast | No extra setup required |
| A/B | Version 2 is released, under specific conditions, to a subset of users. | ✅ | ✅ | ✅ | Fast | No extra setup required |
| Shadow | Version 2 receives real-world traffic without impacting user requests. | ✅ | ✅ | ❌  | Does not apply | Need to maintain parallel environments in order to capture and replay user requests |

### A/B testing
---
[What is A/B Testing? A Practical Guide With Examples | VWO](https://vwo.com/ab-testing/)

### Feature flags
---

- Feature flags are a software development concept that allow you to enable or disable a feature without modifying the source code or requiring a redeploy.
- They are also commonly referred to as feature toggles, release toggles or feature flippers.
- Feature flags determine at runtime which portions of code are executed.

### Backward compatibility

---

API Backward Compatibility:

- Enforce Backward Compatibility
- Document All API Versions and Publish Changelogs
- Avoid Changing Endpoints or Response Formats
- Opt for Versioning Through the URI Path
- Publish an Up-to-Date Release Schedule

## 3.2 Deploying applications to a serverless computing environment.
Considerations include:

### Sizing and scaling serverless environments

---

- **Cloud Run**
   
    - In Cloud Run, each [revision](https://cloud.google.com/run/docs/resource-model#revisions) is automatically scaled to the number of instances needed to handle all incoming requests or events.
    - When a revision does not receive any traffic, by default it is scaled in to zero instances.
    - However, if desired, you can change this default to specify an instance to be kept idle or "warm" using the [minimum instances](https://cloud.google.com/run/docs/configuring/min-instance) setting.
    - In addition to the rate of incoming requests or events, the number of instances scheduled is impacted by:
        - The CPU utilization of existing instances when they are processing requests or events over a one minute window, targeting to keep scheduled instances to a 60% CPU utilization.
        - The current request concurrency, compared to the [maximum concurrency](https://cloud.google.com/run/docs/about-concurrency) over a one minute window.
            - To give you more control, Cloud Run provides a *maximum concurrent requests per instance* setting that specifies the maximum number of requests that can be processed simultaneously by a given instance.
            - By default each Cloud Run instance can receive up to 80 requests at the same time; you can increase this to a maximum of 1000.
    - The [maximum number of instances setting](https://cloud.google.com/run/docs/configuring/max-instances)
        
        `gcloud run services update SERVICE --max-instances MAX-VALUE`
        
    - The [minimum number of instances setting](https://cloud.google.com/run/docs/configuring/min-instances)
        
        `gcloud run services update SERVICE --min-instances MIN-VALUE`
        
    - The Cloud Run autoscaler evaluates these every 5 seconds.
- **Cloud Functions**

    `gcloud functions deploy FUNCTION_NAME --max-instances MAX_INSTANCE_LIMIT`
    
    `gcloud functions deploy FUNCTION_NAME --clear-max-instances`
    
    - **Limitation and Best Practices**
        
        - Guard against excessive scale-ups : When no `--max-instances` limit is specified, Cloud Functions is designed to favor *scaling up to meet demand* over limiting throughput
        - **Request handling when all instances are busy: Under normal circumstances, your function scales up by creating new instances to handle incoming traffic load. But when you have set a maximum instances limit, you might encounter a scenario where there are insufficient instances to meet incoming traffic load.**
        - **Max instances limits that exceed Cloud Functions scaling ability**
        - **Handling traffic spikes**
        - Deployments: When you deploy a new version of your function, Cloud Functions migrates traffic from the old version to the new one. Because maximum instances limits are set for each version of your function independently, you might temporarily exceed the specified limit during the period after deployment.

### Deploying from source code
---

- **Cloud Run**

    `gcloud run deploy SERVICE --image IMAGE_URL`
    
- **Cloud Function**
    
    ```bash
    gcloud functions deploy python-http-function \
    --gen2 \
    --runtime=python311 \
    --region=REGION \
    --source=. \
    --entry-point=hello_get \
    --trigger-http \
    --allow-unauthenticated
    ```
    

### Invocation via triggers

---

- Cloud Run
    - HTTP
    - gRPC
    - With Pub/Sub
        - Pub/Sub to *push* messages to the endpoint of your Cloud Run service, where the messages are subsequently delivered to containers as HTTP requests.
        - Make sure you set the Pub/Sub subscription acknowledgment deadline (`ackDeadlineSeconds`) to the maximum allowed 600 seconds.
    - With Scheduler
    - With Cloud Task
        - Cloud Tasks can be used to securely enqueue a task to be asynchronously processed by a Cloud Run service.
        - Typical use cases include:
            - Preserving requests through unexpected production incidents
            - Smoothing traffic spikes by delaying work that is not user-facing
            - Speeding user response time by delegating slow background operations to be handled by another service, such as database updates or batch processing
            - Limiting the call rate to backing services like databases and third-party APIs
    - With EventArc
        - You can create an Eventarc trigger so that your Cloud Run service receives notifications of a specified event or set of events.
        - By specifying filters for the trigger, you can configure the routing of the event, including the event source and the target Cloud Run service.
        - Events sent to your Cloud Run service are received in the form of HTTP requests.
        - The following event types trigger requests to your service:
            - [An audit log is created](https://cloud.google.com/eventarc/docs/reference/supported-events#using-cloud-audit-logs) that matches the trigger's filter criteria
            - [Direct events](https://cloud.google.com/eventarc/docs/reference/supported-events#directly-from-a-google-cloud-source) such as an update to a Cloud Storage bucket
            - [Messages published to a Pub/Sub topic](https://cloud.google.com/eventarc/docs/reference/supported-events#using-pubsub)
- Cloud Function
    - HTTP
    - EventArc
        - Eventarc triggers are supported only in Cloud Functions (2nd gen).
    - Cloud Pub/Sub
        
        For a function to use a Pub/Sub trigger, it must be implemented as an [event-driven function](https://cloud.google.com/functions/docs/writing/write-event-driven-functions):
        
        - If you use a [CloudEvent function](https://cloud.google.com/functions/docs/writing/write-event-driven-functions#cloudevent-functions), the Pub/Sub event data is passed to your function in the [CloudEvents format](https://cloud.google.com/eventarc/docs/cloudevents) and the CloudEvent data payload is of type `[MessagePublishedData](https://github.com/googleapis/google-cloudevents/blob/main/proto/google/events/cloud/pubsub/v1/data.proto)`.
        - If you use a [background function](https://cloud.google.com/functions/docs/writing/write-event-driven-functions#background-functions), the Pub/Sub event data payload is passed directly to your function in the `[PubsubMessage](https://github.com/googleapis/google-cloudevents/blob/main/proto/google/events/cloud/pubsub/v1/data.proto)` format.
    - Cloud Storage triggers
        
        For a function to use a Cloud Storage trigger, it must be implemented as an [event-driven function](https://cloud.google.com/functions/docs/writing/write-event-driven-functions):
        
        - If you use a [CloudEvent function](https://cloud.google.com/functions/docs/writing/write-event-driven-functions#cloudevent-functions), the Cloud Storage event data is passed to your function in the [CloudEvents format](https://cloud.google.com/eventarc/docs/cloudevents) and the CloudEvent data payload is of type `[StorageObjectData](https://github.com/googleapis/google-cloudevents/blob/main/proto/google/events/cloud/storage/v1/data.proto)`.
        - If you use a [background function](https://cloud.google.com/functions/docs/writing/write-event-driven-functions#background-functions), the Cloud Storage event data payload is passed directly to your function in the `[StorageObjectData](https://github.com/googleapis/google-cloudevents/blob/main/proto/google/events/cloud/storage/v1/data.proto)` format.
        
        | Event | Event type | Description |
        | --- | --- | --- |
        | Object finalized | - 2nd gen: google.cloud.storage.object.v1.finalized (through Eventarc)
        - 1st gen: google.storage.object.finalize | Occurs when a new object is created, or an existing object is overwritten and a new generation of that object is created. |
        | Object deleted | - 2nd gen: google.cloud.storage.object.v1.deleted (through Eventarc)
        - 1st gen: google.storage.object.delete | Occurs when an object is permanently deleted. |
        | Object archived | - 2nd gen: google.cloud.storage.object.v1.archived (through Eventarc)
        - 1st gen: google.storage.object.archive | Occurs when a live version of an object becomes a noncurrent version. See https://cloud.google.com/storage/docs/object-versioning for more information. |
        | Object metadata updated | - 2nd gen: google.cloud.storage.object.v1.metadataUpdated (through Eventarc)
        - 1st gen: google.storage.object.metadataUpdate | Occurs when the https://cloud.google.com/storage/docs/metadata of an existing object changes. |
    - Google Cloud Firestore Triggers    
        This content applies only to Cloud Functions (2nd gen). 
    - Cloud Audit Logging


### Configuring event receivers

---

![Untitled](Exam%20Guide%205c4083c3ad9f4ddc816526c2d279b732/Untitled%204.png)

- Eventarc offers a standardized solution to manage the flow of state changes, called *events*, between decoupled microservices.
- When triggered, Eventarc routes these events to various destinations (in this document, see [Event destinations](https://cloud.google.com/eventarc/docs/overview#targets)) while managing delivery, security, authorization, observability, and error-handling for you.

### Exposing and securing application APIs (e.g., API Gateway, Cloud Endpoints)

---

- **Setting the backend service address and path in the OpenAPI spec using API Gateway (Fully managed gateway for serverless workloads)**
    
    
    | Backend | x-google-backend |
    | --- | --- |
    | Cloud Functions | x-google-backend: address: https://GCP_REGION-PROJECT_ID.cloudfunctions.net/hello |
    | Cloud Run | x-google-backend:  address: https://hello-HASH.a.run.app |
    | App Engine standard environment | x-google-backend: address: https://PROJECT_ID.appspot.com |
- **Cloud Endpoint**
    
    ![Untitled](Exam%20Guide%205c4083c3ad9f4ddc816526c2d279b732/Untitled%205.png)
    

## 3.3 Deploying applications and services to Google Kubernetes Engine (GKE).

---

Considerations include:

### Deploying a containerized application to GKE
---
[Quickstart: Deploy an app in a container image to a GKE cluster  |  Google Kubernetes Engine (GKE)  |  Google Cloud](https://cloud.google.com/kubernetes-engine/docs/quickstarts/deploy-app-container-image#python)

### Integrating Kubernetes RBAC with Identity and Access Management (IAM)

---

- RBAC
 
    - [RBAC](https://kubernetes.io/docs/reference/access-authn-authz/rbac/) is a core security feature in Kubernetes that lets you create fine-grained permissions to manage what actions users and workloads can perform on resources in your clusters.
    - Kubernetes' native [role-based access control (RBAC)](https://cloud.google.com/kubernetes-engine/docs/role-based-access-control) system also manages access to resources in your cluster.
    - RBAC controls access on a cluster and namespace level, while IAM works on the project level.
    - As a platform administrator, you create RBAC *roles* and bind those roles to *subjects*, which are authenticated users such as service accounts or Groups. Kubernetes RBAC is enabled by default.
- IAM
   
    - IAM permissions work alongside [Kubernetes RBAC](https://cloud.google.com/kubernetes-engine/docs/role-based-access-control), which provides granular access controls for specific objects inside a cluster or namespace.
    - IAM has a stronger focus on permissions at the project and organization level, though it does provide several predefined roles specific to GKE.

IAM and RBAC can work together. An entity must have sufficient RBAC and IAM permissions to work with resources in your cluster.

### Configuring Kubernetes namespaces
---

- To set the namespace for a current request, use the `--namespace` flag.
- For example:
    
    `kubectl run nginx --image=nginx --namespace=<insert-namespace-name-here>`
    
    `kubectl get pods --namespace=<insert-namespace-name-here>`
    

### Defining workload specifications (e.g., resource requirements)

---

- Kubernetes provides different kinds of controller objects that correspond to different kinds of workloads you can run.
- Certain controller objects are better suited to representing specific types of workloads.
- The following sections describe some common types of workloads and the Kubernetes controller objects you can create to run them on your cluster, including:
    - **Stateless applications - Deployments**
                
        A *[stateless application](https://cloud.google.com/kubernetes-engine/docs/how-to/stateless-apps)* does not preserve its state and saves no data to persistent storage — all user and session data stays with the client.
        
        Some examples of stateless applications include web frontends like [Nginx](https://www.nginx.com/resources/wiki/), web servers like [Apache Tomcat](http://tomcat.apache.org/), and other web applications.
        
        You can create a Kubernetes [Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) to deploy a stateless application on your cluster. 
        
        Pods created by Deployments are not unique and do not preserve their state, which makes scaling and updating stateless applications easier.
        
        [Deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
        
    - **Stateful applications - StatefulSet**

        - A *[stateful application](https://cloud.google.com/kubernetes-engine/docs/how-to/stateful-apps)* requires that its state be saved or persistent. Stateful applications use persistent storage, such as [persistent volumes](https://cloud.google.com/kubernetes-engine/docs/concepts/persistent-volumes), to save data for use by the server or by other users.
        - Examples of stateful applications include databases like [MongoDB](https://www.mongodb.com/) and message queues like [Apache ZooKeeper](https://zookeeper.apache.org/).
        - You can create a Kubernetes [StatefulSet](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/) to deploy a stateful application.
        - Pods created by StatefulSets have unique identifiers and can be updated in an ordered, safe way.
        
        [StatefulSets](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/)
        
    - **Batch jobs - Jobs**
        
        - *Batch jobs* represent finite, independent, and often parallel tasks which run to their completion.
        - Some examples of batch jobs include automatic or scheduled tasks like sending emails, rendering video, and performing expensive computations.
        - You can create a Kubernetes [Job](https://kubernetes.io/docs/concepts/workloads/controllers/job/) to execute and manage a batch task on your cluster.
        - You can specify the number of Pods that should complete their tasks before the Job is complete, as well as the maximum number of Pods that should run in parallel.
        
        [Jobs](https://kubernetes.io/docs/concepts/workloads/controllers/job/)
        
    - **Daemons - DaemonSet**
 
        
        - *Daemons* perform ongoing background tasks in their assigned nodes without the need for user intervention.
        - Examples of daemons include log collectors like [Fluentd](https://www.fluentd.org/) and monitoring services.
        - You can create a Kubernetes [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) to deploy a daemon on your cluster.
        - DaemonSets create one Pod per node, and you can choose a specific node to which the DaemonSet should deploy.
        
        [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)
        

### Building a container image using Cloud Build

---

1. Sample `cloudbuild.yaml`file
    
    ```yaml
    steps:
    - name: 'gcr.io/cloud-builders/docker'
      args: [ 'build', '-t', 'us-west2-docker.pkg.dev/$PROJECT_ID/quickstart-docker-repo/quickstart-image:tag1', '.' ]
    images:
    - 'us-west2-docker.pkg.dev/$PROJECT_ID/quickstart-docker-repo/quickstart-image:tag1'
    ```
    
2. Run gcloud command to create the container image
    
    ```bash
    gcloud builds submit --region=us-west2 --config cloudbuild.yaml
    ```
    

### Configuring application accessibility to user traffic and other services

---

Services provide load-balanced access to specified Pods. 

There are three primary types of Services:

- ClusterIP: Exposes the service on an IP address that is only accessible from within this cluster. This is the default type.
- NodePort: Exposes the service on the IP address of each node in the cluster, at a specific port number.
- LoadBalancer: Exposes the service externally, using a load balancing service provided by a cloud provider.  In Google Kubernetes Engine, LoadBalancers give you access to a regional Network Load Balancing configuration by default. To get access to a global HTTP(S) Load Balancing configuration, you can use an Ingress object

[Exposing applications using services  |  Google Kubernetes Engine (GKE)  |  Google Cloud](https://cloud.google.com/kubernetes-engine/docs/how-to/exposing-apps)

### Managing container lifecycle

---

![Untitled](Exam%20Guide%205c4083c3ad9f4ddc816526c2d279b732/Untitled%206.png)

![Untitled](Exam%20Guide%205c4083c3ad9f4ddc816526c2d279b732/Untitled%207.png)

There are two hooks that are exposed to Containers:

- PostStart
    
    This hook is executed immediately after a container is created. However, there is no guarantee that the hook will execute before the container ENTRYPOINT. No parameters are passed to the handler.
    
- PreStop
    
    This hook is called immediately before a container is terminated due to an API request or management event such as a liveness/startup probe failure, preemption, resource contention and others. A call to the `PreStop` hook fails if the container is already in a terminated or completed state and the hook must complete before the TERM signal to stop the container can be sent. The Pod's termination grace period countdown begins before the `PreStop` hook is executed, so regardless of the outcome of the handler, the container will eventually terminate within the Pod's termination grace period. No parameters are passed to the handler.
    

Sample `lifecycle-events.yaml`file

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: lifecycle-demo
spec:
  containers:
  - name: lifecycle-demo-container
    image: nginx
    lifecycle:
      postStart:
        exec:
          command: ["/bin/sh", "-c", "echo Hello from the postStart handler > /usr/share/message"]
      preStop:
        exec:
          command: ["/bin/sh","-c","nginx -s quit; while killall -0 nginx; do sleep 1; done"]
```

# **Section 4: Integrating Google Cloud Services**

## 4.1 Integrating an application with data and storage services.

Considerations include:

### Managing connections to data stores (e.g., Cloud SQL, Cloud Spanner, Firestore, Bigtable, Cloud Storage)
---
*Cloud SQL**
- There are two ways to connect Cloud SQL
    1. Direct SSL/TLS connection: (Not Recommended)
    2. Cloud SQL Auth Proxy
        
        
        ![Untitled](Google%20Professional%20Cloud%20Developer%203bda04824918487d9efd4eaf177a78fd/Cloud%20Run%207fc77326e99147d78b43c9287d58d8db/Untitled%204.png)
        
        ![Untitled](Google%20Professional%20Cloud%20Developer%203bda04824918487d9efd4eaf177a78fd/Cloud%20Run%207fc77326e99147d78b43c9287d58d8db/Untitled%205.png)
        
        - First, you need to configure the Cloud Run service to connect to a Cloud SQL instance.
        - Next, from your application, connect to a UNIX domain socket. This is a special kind of file in your container in the directory/Cloud SQL. This is a standard way to connect to databases and is similar to using a TCP connection with host name and a port.
        - Finally, you'll need to make sure that the service identity of your Cloud Run service is allowed to connect to the Cloud SQL instance. The identity should have Cloud SQL client role on the Cloud SQL instance.
- There are three ways to control transaction
    1. Connection Pooling setting
    2. External Connection Pool using compute engine
    3. Max instance settings

![Untitled](Google%20Professional%20Cloud%20Developer%203bda04824918487d9efd4eaf177a78fd/Cloud%20Run%207fc77326e99147d78b43c9287d58d8db/Untitled%206.png)

**MemoryStore**   
![Untitled](Google%20Professional%20Cloud%20Developer%203bda04824918487d9efd4eaf177a78fd/Cloud%20Run%207fc77326e99147d78b43c9287d58d8db/Untitled%207.png)

**Cloud Storage, Cloud Spanner and Firestore**
- connect using client libraries

### Reading/writing data to/from various data stores
---
- Refer the client library of the databases for language specific SDK details.

### Writing an application that publishes/consumes data asynchronously (e.g., from Pub/Sub)

---
**Subscription models**
- **Pull (default subscription mode)**    
![Untitled](Exam%20Guide%205c4083c3ad9f4ddc816526c2d279b732/Untitled%208.png)
    - Pull Subscriber needs to explicitly call pull message from pub/sub using SDK.
    - Process Flow of Pull Subscription using SDK
        1. The subscriber calls pull method to request messages for delivery.
        2. Pub/Sub returns a message and acknowledgment ID.
        3. To acknowledge receipt, the subscriber invokes the acknowledged method by using the acknowledgment ID.
    - Subscriber controls the rate of delivery by modifying the acknowledgment deadline to allow more time to process messages.
    - Pull Subscription model enables batched delivery and massively parallel consumption
    - Use Pull Subscription model when need to process a **very large volume of messages with high throughput**

- **Push**
    - Push subscriber doesn't need to use any SDK to get message.
    - Process Flow of Push Subscription
        1. Pub/Sub sends each message to an HTTP endpoint.
        2. The Endpoint can be a load-balancer or any serverless app.
        3. The end point acknowledges the message by returning an HTTP status code
    - Pub/Sub dynamically adjusts the rate of push requests based on the rate at which it receives success responses.

## 4.2 Integrating an application with compute services.
Considerations include:
### Using service discovery (e.g., Service Directory)
---

- Service Directory helps reduce the complexity of management and operations by providing a single place to publish, discover, and connect services.
- It is a managed service that enhances service inventory management at scale so you don't have to.
- Service Directory provides real-time service information, whether you have a few service endpoints or thousands.
- This helps ensure that your applications only resolve the most updated information of their resources, increasing the reachability of your services.
- Good to know
    - GCE: Either use the metadata server, or hit a DNS name from a load balancer.
    - GKE
        - Endpoints
            - Connect to an IP via a connection string, as opposed to hard-coding the IP.
            - If it's a DNS name, use an `ExternalName` service.
        - Kubernetes Services
            1. `ClusterIP`: Expose an IP to the cluster.
            2. `NodePort`: Expose an IP of each node at a specific port.
            3. `LoadBalancer`: Expose externally with a cloud LB.
        - In GCP, this is a regional network LB. Use an ingress object for a global LB.
        - [Cloud Tech Video: Mapping External Services (Kubernetes)](https://youtu.be/fvpq4jqtuZ8).

### Reading instance metadata to obtain application configuration

---

- Every virtual machine (VM) instance stores its metadata on a metadata server. Your VM automatically has access to the metadata server API without any additional authorization.
- Metadata is stored as `key:value` pairs.
- There is a default set of metadata keys that are available for VMs running on Compute Engine.
- For a list of these default metadata keys, see [Default metadata values](https://cloud.google.com/compute/docs/metadata/default-metadata-values).
- You can also use your own custom metadata keys on an individual VM or project. For more information, see [Set custom metadata](https://cloud.google.com/compute/docs/metadata/setting-custom-metadata).
- Pass metadata value into start-up scripts to avoid brittle designs.

### Graceful application startup and shutdown
---
[Overview  |  Compute Engine Documentation  |  Google Cloud](https://cloud.google.com/compute/docs/instances/startup-scripts)
[Running shutdown scripts  |  Compute Engine Documentation  |  Google Cloud](https://cloud.google.com/compute/docs/shutdownscript)

## 4.3 Integrating Cloud APIs with applications.

Considerations include:

### Enabling a Cloud API
---
- `gcloud services enable` or via the Console.

### Making API calls using supported options (e.g., Cloud Client Library, REST API or gRPC, API Explorer) taking into consideration:
---
- Batching requests
- Restricting return data
- Paginating results
    - Listable collections **should** support pagination, even if results are typically small.   
    **Rationale:** If an API does not support pagination from the start, supporting it later is troublesome because adding pagination breaks the API's behavior. Clients that are unaware that the API now uses pagination could incorrectly assume that they received a complete result, when in fact they only received the first page.
    
    To support pagination (returning list results in pages) in a `List` method, the API **shall**:
    - define a `string` field `page_token` in the `List` method's request message. The client uses this field to request a specific page of the list results.
    - define an `int32` field `page_size` in the `List` method's request message. Clients use this field to specify the maximum number of results to be returned by the server. The server **may** further constrain the maximum number of results returned in a single page. If the `page_size` is `0`, the server will decide the number of results to be returned.
    - define a `string` field `next_page_token` in the `List` method's response message. This field represents the pagination token to retrieve the next page of results. If the value is `""`, it means no further results for the request.
    
    To retrieve the next page of results, client **shall** pass the value of response's `next_page_token` in the subsequent `List` method call (in the request message's `page_token` field):
    
- Caching results
- Error handling (e.g., exponential backoff)
- Using service accounts to make Cloud API calls

# **Section 5: Managing Deployed Applications**

## 5.1 Managing cloud compute services (e.g., Google Kubernetes Engine, serverless).

Considerations include:

### Analyzing lifecycle events
---
For GKE: covered in 3.3 Sub-section: Managing container lifecycle

### Using external metrics and corresponding alerts
---
- Metrics sent to Google Cloud projects with a metric type beginning `external.googleapis.com` are known as **external metrics**.
- The metrics are typically exported by open-source projects and third-party providers.
- There is presently a limit of 25,000 external metric descriptors per Google Cloud project.
- Cloud Monitoring treats external metrics the same as [custom metrics](https://cloud.google.com/monitoring/custom-metrics), with one exception.
- For external metrics, a **resource_type** of `global` is invalid and results in the metric data being discarded.

[External metrics list  |  Cloud Monitoring  |  Google Cloud](https://cloud.google.com/monitoring/api/metrics_other#externalgoogleapiscom)

### Configuring workload autoscaling
---
| Service | Option |
| --- | --- |
| Cloud Run | set min, max and maximum concurrency setting |
| Cloud Function | configure --max-instances |
| GKE |  set --enable-autoscaling at cluster level |

## 5.2 Troubleshooting applications.

Considerations include:

### Using Debugger

### **Using Cloud Logging**
---
- Dataflow, Cloud Functions (flexible and standard) , App Engine and GKE have built in support for logging
- Using Cloud logging, one can view logs and search for particular type of messages
- One can also create custom log-based metrics  and alerts based on these metrics.
- Supports
    - Platform/system/app logs
    - Log search/view/filter
    - Logs-based metrics

### **Using Cloud Monitoring**
---
- Cloud Monitoring helps increase reliability by giving users the ability to monitor Google Cloud and multi-cloud environments to identify trends and prevent issues.
- One can reduce monitoring overhead and improve your signal-to-noise ratio, allowing you to detect and fix problems faster.
- SLI
    - A service level indicator, or SLI, is a quantitative measure of some aspect of a service.
    - A service level indicator might be a predefined metric or a custom metric, such as a logs-based metric.
- SLO
    - A service level objective, or SLO, is a target value or range of values for a service level.
    - Service level objectives should include tolerance for small variations.
- Recommended 4 golden signals for dashboards
    - Latency
    - Traffic
    - Errors
    - Saturations
- It Contains following capability
    - Platform/system/app metrics
    - Uptime/health checks
    - Dashboards
    - Alert

### **Using Cloud Profiler**
---
- Cloud Profiler uses statistical techniques, and low-impact instrumentation that runs across all production application instances to provide a complete picture of an application's performance without slowing it down.
- Cloud Profiler helps you identify and eliminate potential performance issues.
- Cloud Profiler monitors CPU and heap to help identify latency and inefficiency using interactive graphical tools, to improve application bottlenecks and reduce resource consumption.
- It continuously analyzing the performance of CPU or memory-intensive functions executed across an application.
- Profiler presents the call hierarchy and resource consumption of the relevant function in an interactive flame graph that helps developers understand which paths consume the most resources and the different ways in which their code is actually called.
- Cloud Profiler allows developers to analyze applications running anywhere, including Google Cloud, other cloud platforms, or on-premises, with support for Java, Go, Node.js, and Python.
- In order to use the Cloud Profiler, profiling agent is required to be installed.

### **Using Cloud Trace**
---
- Cloud Trace is a distributed tracing system that collects latency data from your applications and displays it in the Google Cloud Console.
- One can track how requests propagate through your application, and receive detailed near real-time performance insights.
- Cloud Trace automatically analyzes all of your application's traces to generate in-depth latency reports, surfacing performance degradations.
- Cloud Trace can capture traces from all of your VMs, containers, and App Engine projects.
- Using Trace, you can inspect detailed latency information for a single request, or view aggregate latency for your entire application.
- Using the various tools and filters provided, you can quickly find where bottlenecks are occurring and more quickly identify their root causes.
- Trace continuously gathers and analyzes trace data from your project to automatically identify recent changes to your application's performance. These latency distributions, available through the Analysis Reports feature, can be compared over time or versions, and Cloud Trace will automatically alert you if it detects a significant shift in your app's latency profile.
- The Trace SDK is currently available for Java, Node.js, Ruby, and Go.
- Traces are automatically captured for projects running on App Engine

### **Using Error Reporting**