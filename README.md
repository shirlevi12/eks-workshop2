# Kubernetes WordPress & MariaDB Deployment on EKS

This repository contains Kubernetes configuration files to deploy a WordPress application and a MariaDB database on an Amazon EKS cluster. Below is an explanation of the key components involved in the deployment.

## Prerequisites

- **Amazon EKS Cluster**: A running Kubernetes cluster on AWS using EKS.
- **kubectl**: Command line tool for interacting with the Kubernetes cluster.
- **AWS CLI**: Configured with necessary permissions to interact with AWS services.
- **Helm**: A tool used for managing Kubernetes applications (optional).
- **Nginx Ingress Controller**.
- **docker**.
  

## Key Components

### 1. **StorageClass**
A `StorageClass` defines how storage resources are provisioned in Kubernetes. In this case, we use AWS EBS (Elastic Block Store) to create persistent volumes. The `StorageClass` ensures that storage is only bound to a pod when it is scheduled, making the provisioning process more efficient.

### 2. **StatefulSet for MariaDB**
A `StatefulSet` is a Kubernetes resource that helps manage stateful applications, such as databases. MariaDB is deployed as a StatefulSet to ensure it has stable network identities and persistent storage. This allows MariaDB to maintain its state even if the pod is rescheduled or restarted.

### 3. **PersistentVolumeClaim (PVC) for MariaDB**
The PVC is used to request storage from the `StorageClass` defined earlier. It specifies the amount of storage (e.g., 5Gi) and the access mode (e.g., ReadWriteOnce), which ensures that MariaDB's data is persisted and not lost when the pod is terminated or restarted.

### 4. **Deployment for WordPress**
A `Deployment` in Kubernetes is used to manage the WordPress application. This defines the number of replicas (pods) for WordPress, the container image to use, and the environment variables needed for WordPress to connect to the MariaDB database. It also specifies the volume mount for persistent storage, ensuring that WordPress's data is stored persistently.

### 5. **PersistentVolumeClaim (PVC) for WordPress**
Similar to MariaDB, WordPress also uses a PVC to request storage. This ensures that any data created by WordPress (such as uploaded images, content, etc.) is stored persistently using the same storage class.

### 6. **Services for MariaDB and WordPress**
In Kubernetes, a `Service` is used to expose an application running in a pod to other pods within the cluster or externally. The MariaDB and WordPress services allow communication between the two containers and ensure that external traffic can reach the WordPress application.

### 7. **Ingress Configuration**
An `Ingress` is a Kubernetes resource that manages HTTP and HTTPS traffic to services within the cluster. In this deployment, an Ingress is used to route traffic from a domain name to the WordPress and Grafana services based on URL paths. For example, traffic to `/grafana` is routed to the Grafana service, while other traffic is routed to the WordPress service.

## Deployment Steps

1. **Set up your EKS Cluster**: Create and configure an EKS cluster in AWS.
2. **Configure kubectl**: Ensure kubectl is connected to your EKS cluster.
3. **Apply the StorageClass**: This defines how persistent volumes are created using AWS EBS.
4. **Create PersistentVolumeClaims**: These requests storage for both MariaDB and WordPress.
5. **Deploy MariaDB**: Deploy MariaDB as a StatefulSet to ensure persistence and stable identity.
6. **Deploy WordPress**: Deploy the WordPress application and connect it to the MariaDB database.
7. **Expose Services**: Create Kubernetes services to expose MariaDB and WordPress internally in the cluster.
8. **Set up Ingress**: Configure an Ingress resource to allow external access to WordPress and Grafana via a domain.

## Accessing the Application

Once deployed, you can access the WordPress application and Grafana through the Ingress resource. The DNS name provided by AWS (ELB) will route traffic to the respective services based on the defined paths in the Ingress configuration.

---

This deployment provides a simple and scalable setup for running a WordPress application backed by MariaDB on AWS using Kubernetes.
