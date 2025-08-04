# MLOps-project-5
Learning Kubernetes (k8s)

>> Distributed Computing: Distributed computing refers to a system where multiple computers (or nodes) work together to solve a large problem or process data collaboratively. The tasks are divided among the nodes, enabling parallel processing for faster and more efficient computation.

**Components of Distributed Computing**

>> Cluster: A cluster is a group of interconnected computers or servers that work together as a single system. Each computer in the cluster is called a node, and they collaborate to share workloads, provide redundancy, and improve performance.

>> Lead-Node Server: The lead-node (or master node) in a distributed system is responsible for managing the cluster. It coordinates tasks like assigning workloads to worker nodes, monitoring their health, and ensuring everything runs smoothly.

>> Communication: Communication in distributed computing refers to how nodes in the cluster exchange data and instructions. It happens through network protocols and is crucial for synchronization, task distribution, and data sharing among nodes.

>> Concurrency (Speed, Fault Tolerance): Concurrency in distributed systems allows multiple tasks to run simultaneously across nodes. This boosts speed and ensures fault tolerance—if one node fails, the workload is shifted to others, preventing disruptions.

>> Comparison with Apache Spark Internally (MapReduce): Apache Spark and Kubernetes both enable distributed computing but operate differently. Spark uses a specialized model called **MapReduce**, where tasks are divided into "map" (processing) and "reduce" (aggregation) steps. Kubernetes, on the other hand, provides a general-purpose framework for orchestrating containerized workloads and does not impose a specific computation model.



**Benefits of Distributed Computing**

>> Scalability: Distributed computing allows you to divide tasks among multiple machines, enabling the system to handle larger workloads. For example, in ML, if training a model on a single machine takes 10 hours, distributing the work across 10 machines can reduce the time significantly.

>> Fault Tolerance: If one machine fails, the distributed system can redistribute the tasks to other machines, ensuring the system keeps running. Think of it as a power grid—if one station goes offline, others take over to maintain the electricity supply.

>> Improved Performance: Tasks are processed in parallel, reducing overall latency and making applications faster. For example, web applications can serve more users simultaneously by running requests on multiple servers.

>> Cost Efficiency: Instead of investing in expensive, high-performance hardware, you can use multiple cheaper machines to achieve the same (or better) results.

>> Flexibility: You can use a mix of different types of machines, hardware, or even cloud providers. This makes it easier to build and manage systems that adapt to changing needs.



**Microservices: The Real-World Scenario**

Imagine you are building a machine learning system to recommend movies to users (like Netflix). This ML system has several components:

1. Data Ingestion: Collects and processes data from users (e.g., their watch history and ratings).  
2. Feature Engineering: Transforms raw data into meaningful inputs for your ML model.  
3. Model Training: Continuously trains and updates your recommendation algorithm.  
4. Model Serving: Hosts the trained model and responds to user requests in real time.  
5. User Interface: Provides the website or app where users can browse and see recommendations.

In the traditional **monolithic architecture**, all these components would be bundled into a single application. If you wanted to scale the system (e.g., if user requests increased), you would have to scale the entire application, even if only the **Model Serving** component needed more resources.

Now, let’s see how **microservices** solve this problem.


**Microservices in Action**

Instead of bundling everything together, microservices break down the application into smaller, independent components. Each component is responsible for a single task and can run, scale, and be updated independently.  

For our ML system:  
1. Data Ingestion Service: Runs independently, continuously collecting and processing data.  
2. Feature Engineering Service: Operates on processed data and outputs features for the model.  
3. Training Service: Triggers model retraining when new data arrives or at scheduled intervals.  
4. Model Serving Service: Hosts the model, answering API requests like, “What movies should User123 watch?”  
5. UI Service: Displays the user interface and connects to the backend services.

This separation makes it easy to scale just the **Model Serving Service** when the number of user requests spikes, without affecting the other parts of the system.



**Real-Life Analogy for Microservices**: Think of microservices as a food court

- Each food stall specializes in one type of cuisine (e.g., burgers, pizza, coffee).  
- They operate independently, so if one stall runs out of ingredients (or needs upgrades), it doesn’t affect the others.
- Customers (users) can pick and choose what they want, and the food court manager (Kubernetes) ensures all stalls have the resources (electricity, water) they need to function.




**Challenges of Distributed Computing**

>> Resource Management: Allocating resources like CPU, memory, and storage across machines is complex. How do you ensure that no machine is overloaded while others are idle?

>> Scaling: Adding or removing machines from the system (scaling up or down) requires significant effort. For instance, if traffic spikes suddenly, how do you add new machines and ensure they integrate seamlessly?

>> Communication and Networking: Machines in a distributed system need to communicate constantly. Network failures, latencies, or configuration errors can cause significant issues.

>> Fault Handling: Ensuring fault tolerance requires mechanisms to detect failures, recover lost data, and reroute tasks, all while minimizing downtime.

>> Load Balancing: Distributing tasks evenly across machines is non-trivial. Overloading one machine while others remain underutilized can degrade system performance.

>> Configuration and Deployment: Managing the deployment of software across multiple machines can be error-prone. Imagine manually configuring hundreds of machines—it's a logistical nightmare.

>> Monitoring and Debugging: Identifying issues in a distributed system is much harder than in a single system. Logs, metrics, and performance data are spread across multiple machines.

















------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### **How Kubernetes Addresses These Challenges**

Kubernetes is a **container orchestration platform** specifically designed to address the challenges of distributed computing:

1. **Automated Resource Management**  
   Kubernetes schedules workloads across machines (nodes) based on available resources, ensuring optimal usage and preventing overload.

2. **Effortless Scaling**  
   Scaling is as simple as changing a number in the deployment configuration. Kubernetes automatically adds or removes pods to match the desired state.

3. **Reliable Networking**  
   Kubernetes provides built-in networking solutions, enabling pods (smallest compute units) to communicate seamlessly, even in complex environments.

4. **Self-Healing**  
   Kubernetes continuously monitors the health of your system. If a pod crashes, Kubernetes restarts it automatically or reschedules it on another node.

5. **Load Balancing**  
   Kubernetes evenly distributes traffic across all healthy pods, ensuring no single pod is overwhelmed while others remain idle.

6. **Simplified Deployment**  
   Using declarative configuration files (YAML), Kubernetes allows you to define how applications should run. Once defined, Kubernetes takes care of deploying and managing them.

7. **Centralized Monitoring and Debugging**  
   Kubernetes integrates with monitoring tools like Prometheus and logging tools like ELK Stack, providing a unified view of the system.

---


** Kubernetes Internals **

Master Node (Control Plane): The master node is the brain of the Kubernetes cluster. It oversees the system, manages workloads, and ensures everything runs as expected. Think of it as the manager in a factory that delegates tasks and monitors operations.


Resource Manager: The resource manager in Kubernetes ensures that cluster resources like CPU, memory, and storage are allocated efficiently. It’s like a warehouse manager who ensures that raw materials (resources) are distributed to the right production lines (nodes).


API Server (For user comm): Users interact with Kubernetes through the API Server. It’s like the receptionist at an office—you send requests (like deploying an app), and the API Server routes them to the appropriate components in the cluster.


Database (etcd): `etcd` is Kubernetes' central database where all cluster data is stored, including the current state of the system and desired configurations. Imagine it as a library catalog—every time you check out or return a book, the catalog is updated to reflect the changes.


Worker Node: Worker nodes are the muscle of the cluster. They run your applications and handle the tasks assigned by the master node. Think of them as factory workers who execute tasks given by the manager (master node).


Kubelet: The kubelet is an agent running on each worker node that ensures containers (your applications) are running as expected. It’s like a shift supervisor in a factory who ensures each machine is operating correctly.


Kube-Proxy: The kube-proxy manages network traffic for pods, ensuring they can communicate with each other and the outside world. It’s like a traffic cop at a busy intersection, directing cars (data packets) to the right destinations.


Pods: A pod is the smallest deployable unit in Kubernetes and typically wraps one or more containers. Think of it as a container ship holding one or more goods (containers) and transporting them across a logistics network.


SharedDB (Volumes): Volumes are shared storage spaces in Kubernetes that allow pods to save and share data. It’s like a shared locker room where workers (pods) can access tools or leave notes for each other.


Kube-Manifest (YAML): Kube manifests are configuration files written in YAML that define what you want Kubernetes to do, such as deploying an app or creating a service. Think of it as the blueprint for building a house—Kubernetes reads it to know exactly what to construct.


Service: A service in Kubernetes provides a stable network endpoint for accessing a set of pods. Even if the pods are replaced or moved, the service ensures they can still be reached. It’s like a restaurant hotline—no matter who answers the phone (which server pod), your order is taken.


Namespace: Namespaces are virtual clusters within a Kubernetes cluster that help organize and isolate resources. It’s like different departments in a large office building—HR, Sales, and IT each work in separate areas but share the same infrastructure.


Scheduler: Decides which worker node will run a new pod based on resource availability. It's like a dispatcher allocating tasks to the most available worker.


ReplicaSets: Ensure that a specified number of identical pods are always running. It’s like a backup generator ensuring there’s always power even if one generator fails.


***** Real-Life Analogy: Distributed Computing Without and With Kubernetes *****

- Without Kubernetes:  
   Imagine running a massive restaurant chain where you have to manage chefs, waiters, supplies, and customer orders manually for each branch. If one branch runs out of ingredients or if a waiter quits, everything falls apart.

- With Kubernetes:  
   Now, imagine a central management system that monitors every branch in real time, restocks ingredients automatically, hires temporary staff when someone quits, and redirects customers to the least crowded branches. That’s Kubernetes for your distributed computing!

---

**Summary**
- Distributed Computing Benefits: Scalability, fault tolerance, improved performance, cost efficiency, and flexibility.  
- Challenges: Resource management, scaling, communication, fault handling, load balancing, deployment, and monitoring.  
- Kubernetes: A robust solution that automates, simplifies, and optimizes the management of distributed systems, making it the backbone for modern applications, especially in MLOps.

---

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

---

**Connecting Microservices to Docker and Kubernetes**

**Docker: Packaging the Microservices**

Each microservice runs inside its own Docker container. Think of Docker containers as "takeout boxes" for food stalls:
- Every box contains all the ingredients (dependencies) needed for that service.
- It’s lightweight, portable, and ensures the service runs the same way everywhere—whether on your laptop, a server, or the cloud.

For example:
- The Data Ingestion Service is packaged in one container.  
- The Model Serving Service is packaged in another.  

**Kubernetes: Orchestrating the Microservices**

Kubernetes acts like the food court manager who ensures all stalls (containers) operate efficiently:
1. Scaling: If the burger stall has a long line, the manager deploys more burger chefs (replicas) to handle the load. Similarly, Kubernetes can spin up more pods for a service.  
2. Networking: The manager ensures customers can easily navigate the food court. Similarly, Kubernetes handles communication between containers and ensures users can access the services.  
3. Load Balancing: Kubernetes distributes traffic among multiple instances of a service (e.g., multiple replicas of the Model Serving Service).  

---

**Advantages of Microservices for ML in Docker-Kubernetes**

1. Independent Scaling: If user traffic spikes, you can scale just the **Model Serving Service** without wasting resources on the **Training Service**.  
2. Resilience: If one microservice fails (e.g., the **Training Service** crashes), the others keep running.  
3. Flexibility: You can use different languages or tools for each service. For example, the **Model Serving Service** might run Python with TensorFlow, while the **UI Service** might use JavaScript with React.

---

Summary
- **Microservices** break ML workflows into smaller, independent services that can be deployed, scaled, and updated separately.  
- **Docker** packages each service into a container, ensuring it runs consistently across environments.  
- **Kubernetes** manages and orchestrates these containers, ensuring they communicate effectively, scale as needed, and stay resilient.

By breaking down a complex ML system into microservices and running it on Docker-Kubernetes, you can handle increasing workloads, deploy faster, and maintain a more resilient architecture.

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------



### **1. How to Incorporate Kubernetes in a CI/CD Workflow**

In your GitHub Actions + AWS project, Kubernetes can replace or complement the deployment process. Here's how:

#### **Current Workflow Recap**
- On pushing to the main branch, GitHub Actions builds the latest Docker image, pushes it to Amazon Elastic Container Registry (ECR), and deploys it on an EC2 server.

#### **Modified Workflow with Kubernetes**
1. **Push Docker Image to ECR** (same as before):
   - GitHub Actions still builds and pushes the image to ECR.

2. **Deploy to Kubernetes:**
   - Instead of directly deploying the image on an EC2 instance, you deploy it to a Kubernetes cluster running on **AWS (EKS)** or any other managed Kubernetes service.
   - Add a step in your CI/CD pipeline to:
     - Use the `kubectl` CLI or GitHub's Kubernetes Action to apply a `Deployment` manifest referencing the latest image from ECR.
     - Example:
       ```yaml
       - name: Deploy to Kubernetes
         run: |
           kubectl apply -f deployment.yaml
       ```

3. **Advantages:**
   - Scalability: Kubernetes automatically handles scaling based on traffic.
   - Fault Tolerance: Pods are automatically restarted if they fail.
   - Rolling Updates: Kubernetes manages zero-downtime updates by gradually replacing old Pods with new ones.

4. **GitHub Actions Example Workflow:**
   ```yaml
   name: CI/CD Pipeline with Kubernetes
   on:
     push:
       branches:
         - main

   jobs:
     deploy:
       runs-on: ubuntu-latest
       steps:
         - name: Checkout Code
           uses: actions/checkout@v3

         - name: Log in to ECR
           run: aws ecr get-login-password --region <region> | docker login --username AWS --password-stdin <account-id>.dkr.ecr.<region>.amazonaws.com

         - name: Build and Push Docker Image
           run: |
             docker build -t <image-name>:latest .
             docker tag <image-name>:latest <account-id>.dkr.ecr.<region>.amazonaws.com/<image-name>:latest
             docker push <account-id>.dkr.ecr.<region>.amazonaws.com/<image-name>:latest

         - name: Deploy to Kubernetes
           uses: azure/k8s-deploy@v4
           with:
             manifests: |
               deployment.yaml
               service.yaml
   ```

This approach integrates Kubernetes seamlessly into your CI/CD workflow.

---

### **2. Where Do People Create Kubernetes Clusters in Real Life?**

In real-world deployments, clusters are typically created on **cloud platforms** or **on-premise servers**, depending on organizational needs. Here's a breakdown:

#### **On-Premise Clusters:**
- **When Used:**
  - Organizations that manage their own data centers (e.g., banks, government institutions) and cannot rely on public clouds for security or compliance reasons.
- **How:**
  - Use tools like **kubeadm** or **Rancher** to set up Kubernetes on physical or virtual servers.
- **Challenges:**
  - You must manage the control plane, nodes, networking, and updates manually.

#### **Cloud-Based Clusters:**
- **When Used:**
  - Most companies prefer this approach because it reduces operational overhead.
- **How:**
  - Use managed Kubernetes services like:
    - **Amazon EKS (Elastic Kubernetes Service)**
    - **Azure AKS (Azure Kubernetes Service)**
    - **Google GKE (Google Kubernetes Engine)**

- **Benefits:**
  - The cloud provider handles the control plane, networking, and scaling.
  - You focus on deploying and managing workloads.
  - Easily integrate with other cloud services (e.g., storage, logging, monitoring).

#### **Why Minikube Is Not Used in Production**
- **Minikube** is a lightweight tool meant for local development and testing only.
- In production, you need a robust setup with scalability, monitoring, and high availability — something **Minikube cannot provide**.
- Instead, companies use **cloud-managed Kubernetes services** or set up clusters manually with tools like **kubeadm** or Infrastructure as Code (IaC) tools like **Terraform**.

---

### **3. How Do Managed Kubernetes Services (EKS/AKS/GKE) Help?**

Managed Kubernetes services make deploying and managing Kubernetes clusters easier by handling many of the complexities for you. Here's how they help:

#### **Features of Managed Kubernetes Services:**
1. **Cluster Management:**
   - The control plane (master node) is managed by the cloud provider. You don't have to worry about installing, maintaining, or scaling it.

2. **Networking:**
   - The provider handles networking between nodes and integration with their cloud network (VPCs, subnets, etc.).

3. **Integration with Cloud Services:**
   - **AWS (EKS):**
     - Integrates with IAM for secure role-based access.
     - Supports **CloudWatch** for logs and metrics.
   - **Azure (AKS):**
     - Integrates with Azure Monitor for performance insights.
     - Azure AD for secure identity management.
   - **Google (GKE):**
     - Tight integration with Google Cloud's operations suite (logging, monitoring).
     - Pre-configured auto-scaling for workloads.

4. **Scaling:**
   - Easily scale clusters vertically (more CPU/RAM per node) or horizontally (more nodes).

5. **Ease of Updates:**
   - Managed services let you update the Kubernetes version with minimal effort and downtime.

6. **High Availability:**
   - Providers ensure the control plane is replicated across availability zones, so it's always available.

---

### **Key Differences Between EKS/AKS/GKE**
| Feature                  | AWS EKS                             | Azure AKS                          | Google GKE                        |
|--------------------------|--------------------------------------|-------------------------------------|-----------------------------------|
| **Ease of Use**          | Medium                              | Beginner-Friendly                  | Beginner-Friendly                 |
| **Best For**             | Enterprise-scale applications       | Seamless Azure integration          | AI/ML workloads and experimentation|
| **Pricing**              | Control plane billed separately     | Free control plane, pay for nodes  | Free control plane, pay for nodes |
| **Unique Advantage**     | Tight integration with AWS services | Azure AD for secure access         | Advanced auto-scaling algorithms  |

---

### **Conclusion**
For real-life Kubernetes deployments, most organizations prefer managed services like EKS/AKS/GKE for their ease of use, scalability, and integration with cloud ecosystems.


----------------------------------------------------------



### **Managed Kubernetes Services Overview**
Managing Kubernetes on your own (self-hosted) requires handling everything: setting up clusters, managing networking, scaling, and upgrading Kubernetes versions. Managed Kubernetes services simplify this by handling much of the heavy lifting for you, letting you focus on deploying and managing your applications.

Each cloud provider offers a managed Kubernetes service:
- **AWS**: Elastic Kubernetes Service (EKS)  
- **Azure**: Azure Kubernetes Service (AKS)  
- **GCP**: Google Kubernetes Engine (GKE)

---

### **1. AWS Elastic Kubernetes Service (EKS)**

#### **What It Feels Like**  
AWS EKS provides a reliable way to run Kubernetes on AWS infrastructure. You don’t need to worry about setting up the Kubernetes control plane—it’s fully managed by AWS. You only handle the worker nodes.

#### **Key Features**  
1. **Integration with AWS Ecosystem**  
   - Seamless integration with AWS services like **CloudWatch (monitoring)**, **IAM (identity management)**, **Elastic Load Balancers**, and **Amazon VPC (networking)**.  
   - For example, EKS makes it easy to connect your Kubernetes pods to an S3 bucket for storing ML model outputs.

2. **High Availability and Scalability**  
   - AWS runs the control plane across multiple availability zones, ensuring high availability.  
   - You can easily scale worker nodes using **Auto Scaling Groups**.

3. **Custom Networking Options**  
   - AWS gives advanced networking flexibility, such as **VPC CNI Plugin**, to provide custom IP addresses for every pod.

#### **Why Use It?**  
- Perfect for teams already using AWS services for their applications or ML pipelines.  
- Supports highly complex workloads with custom networking and storage needs.

---

### **2. Azure Kubernetes Service (AKS)**

#### **What It Feels Like**  
AKS offers one of the most beginner-friendly Kubernetes experiences. It automates tasks like cluster setup, node upgrades, and security patching, making it ideal for developers who are new to Kubernetes.

#### **Key Features**  
1. **Seamless Integration with Azure Ecosystem**  
   - Built-in integration with Azure services like **Azure Monitor**, **Azure Active Directory (AAD)** for RBAC, and **Blob Storage** for persistent storage.  
   - AKS simplifies linking Azure ML with Kubernetes for deploying and managing ML models.

2. **Serverless Kubernetes with Virtual Nodes**  
   - Azure’s **Virtual Nodes** allow you to run containers on-demand using **Azure Container Instances (ACI)**. This is great for running temporary or bursty workloads without managing additional worker nodes.

3. **Simplified Upgrades**  
   - AKS automates cluster upgrades and patches, reducing downtime and ensuring a secure Kubernetes environment.

#### **Why Use It?**  
- Ideal for teams using Microsoft tools like Azure ML, Power BI, or Office 365.  
- Great for running **hybrid workloads**, as Azure has strong on-prem and cloud integration options.

---

### **3. Google Kubernetes Engine (GKE)**

#### **What It Feels Like**  
GKE, built by Google (the creators of Kubernetes), offers a polished Kubernetes experience with cutting-edge features and tight integration with Google Cloud services. It’s highly optimized for AI/ML workloads.

#### **Key Features**  
1. **Smart Cluster Management**  
   - GKE automatically manages the control plane, node pools, and cluster scaling.  
   - **GKE Autopilot**: A fully managed mode where even the worker nodes are abstracted. You just deploy your workloads.

2. **Optimized for AI/ML Workloads**  
   - Tight integration with **Vertex AI**, Google’s AI/ML platform. You can deploy and manage ML models directly in GKE.  
   - Supports GPUs and TPUs (Tensor Processing Units) for high-performance ML workloads.

3. **Built-In Reliability**  
   - Google handles the control plane with multi-zone and regional cluster setups for high availability.  
   - **Pod Autoscaling**: Automatically scales pods based on workload demand.

4. **Advanced Monitoring and Logging**  
   - Out-of-the-box integration with **Cloud Logging** and **Cloud Monitoring** makes it easy to track the health and performance of your clusters.

#### **Why Use It?**  
- Best for teams focused on AI/ML projects due to GPU/TPU support.  
- Great if you want the latest Kubernetes features (GKE often gets them first).  
- Ideal for startups and enterprises looking for a cost-effective managed Kubernetes solution.

---

### **Comparison of Managed Kubernetes Services**

| Feature                 | **AWS EKS**                          | **Azure AKS**                      | **Google GKE**                     |
|-------------------------|--------------------------------------|------------------------------------|------------------------------------|
| **Ease of Use**         | Advanced, more manual setup required | Beginner-friendly, highly automated| Polished and cutting-edge          |
| **Integration**         | AWS ecosystem (S3, IAM, VPC)         | Azure tools (AAD, Blob Storage)    | GCP tools (Vertex AI, BigQuery)    |
| **Autoscaling**         | Manual scaling or Auto Scaling Groups| Simplified autoscaling             | Pod and cluster autoscaling        |
| **AI/ML Support**       | Good, with support for SageMaker     | Moderate                           | Best (GPU/TPU and Vertex AI)       |
| **Networking**          | Flexible (VPC CNI plugin)            | Straightforward                    | Seamless and intuitive             |
| **Cost**                | Pay-as-you-go, slightly higher       | Competitive                        | Cost-effective, Autopilot mode     |

---

### **Real-Life Analogy: Managed Kubernetes as Renting a Car**

- **AWS EKS**: Like renting a car with all the bells and whistles but requiring some setup—setting the GPS, understanding the controls, and driving yourself. Great if you know what you’re doing and need flexibility.  
- **Azure AKS**: Like renting an easy-to-drive car with an automatic transmission. It’s perfect for beginners and offers additional safety features to make your ride smooth.  
- **GCP GKE**: Like renting a self-driving car. You sit back and let the car (GKE) handle most of the complexities while you focus on your destination (deploying and managing workloads).

---

### **Key Takeaway**
Managed Kubernetes services make Kubernetes accessible and scalable by handling the hardest parts of running Kubernetes. The choice between **EKS**, **AKS**, and **GKE** depends on your familiarity with the provider, existing ecosystem usage, and workload type (general apps vs. ML-heavy workloads).


------------------------------------------------------------------



# Kubernetes Tutorial Projectflow

## Prerequisites

1. **Docker Desktop**: Ensure Docker Desktop is installed and running.
2. **Docker Hub**: Sign in to Docker Hub.
3. **Minikube**: Install Minikube (instructions provided below).

---

## Steps to Execute the Project

### **1. Build the Application**
1. Create or use an existing application.
2. Test it locally to ensure it works as expected.

---

### **2. Prepare and Build the Docker Image**

1. Create a `Dockerfile` in your project directory.
2. Build the Docker image:
   ```bash
   docker build -t kubernetes-test-app:latest .
   ```
3. Verify the image:
   ```bash
   docker images
   ```
4. Test the image locally:
   ```bash
   docker run -p 5000:5000 kubernetes-test-app:latest
   ```

---

### **3. Create the Deployment YAML File**
Define a Kubernetes deployment in a file named `deployment.yaml`.

---

### **4. Install Minikube**

1. Visit the Minikube installation page: [Minikube Installation Guide](https://minikube.sigs.k8s.io/docs/start/?arch=%2Fwindows%2Fx86-64%2Fstable%2F.exe+download).
2. Download the `.exe` file and execute the installation via PowerShell as an administrator.
3. Restart your terminal after the installation (a system reboot may be required).

Start Minikube:
```bash
minikube start
# or
minikube start --embed-certs
```

---

### **5. Troubleshooting Minikube**

#### **Common Error**

```text
Failing to connect to https://registry.k8s.io/ from both inside the minikube container and host machine
To pull new external images, you may need to configure a proxy: https://minikube.sigs.k8s.io/docs/reference/networking/proxy/
```

#### **Solutions**
1. If using a proxy, configure it for Minikube:
   ```bash
   minikube start --docker-env HTTP_PROXY=http://your-proxy:port --docker-env HTTPS_PROXY=https://your-proxy:port
   ```

2. If no proxy is being used, unset these environment variables:
   ```bash
   unset HTTP_PROXY
   unset HTTPS_PROXY
   unset NO_PROXY
   ```

3. Test connectivity to `registry.k8s.io`:
   ```bash
   nslookup registry.k8s.io
   ```
   Example output:
   ```text
   Server:  dlinkrouter.local
   Address:  192.168.0.1

   Non-authoritative answer:
   Name:    registry.k8s.io
   Addresses:  2600:1901:0:bbc4::
             34.96.108.209
   ```

4. Clean up and restart Minikube:
   ```bash
   minikube stop
   minikube delete --all
   ```

---

### **6. Verify Minikube Status**

Check the status of the Minikube cluster:
```bash
minikube status
```

Verify Kubernetes resources:
```bash
kubectl get all -A
kubectl get pods -A
kubectl get nodes -A
```

---

### **7. Adding Nodes**

To add a new node to the cluster:
```bash
minikube start --nodes=2
# or
minikube start --nodes=2 --embed-certs
```

---

### **8. Load Docker Image to Minikube**

1. Verify the Docker images:
   ```bash
   minikube image list
   docker images
   ```
2. Load the Docker image into Minikube:
   ```bash
   minikube image load kubernetes-test-app:latest
   ```

---

### **9. Apply Deployment**

Deploy the application:
```bash
kubectl apply -f deployment.yaml
```

To delete the deployment (if needed):
```bash
kubectl delete deployment kubernetes-test-app
```

---

### **10. Test the Application**

1. Check Pods and Nodes:
   ```bash
   kubectl get pods -A
   kubectl get nodes -A
   ```

2. Delete a specific Pod (optional):
   ```bash
   kubectl delete pod <pod-name/id>
   ```

3. Access the application service:
   ```bash
   minikube service kubernetes-test-app
   ```

4. Open the Minikube dashboard:
   ```bash
   minikube dashboard
   ```

---

### **11. Debugging and Logs**

- View Pod logs:
  ```bash
  kubectl logs -f <pod-id>
  ```

- Check Endpoints and Services:
  ```bash
  kubectl get endpoints
  kubectl get service
  ```

---

### **12. Load Testing with Postman**

1. Use Postman to run performance tests:
   - Go to **Collection > Runs > Performance > Run Performance Test**.
   - Example: Fixed configuration with 10 users for 1 minute.

---

### **13. Stop Minikube**

Stop the Minikube cluster:
```bash
minikube stop
```

---

### **14. Handling ImagePullBackOff Issue**

If you encounter the `ImagePullBackOff` error:

1. Tag the Docker image with your Docker Hub username:
   ```bash
   docker tag kubernetes-test-app:latest <your-dockerhub-username>/kubernetes-test-app:latest
   ```
2. Push the image to Docker Hub:
   ```bash
   docker push <your-dockerhub-username>/kubernetes-test-app:latest
   ```
