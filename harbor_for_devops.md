### Harbor Overview and Use Cases

**Harbor** is an open-source container image registry that secures artifacts with policies and role-based access control, scans images for vulnerabilities, and signs images as trusted. It extends the Docker Distribution by adding features typically required by enterprises, such as security, identity management, and management of container images. 

### Use Cases

1. **Secure and Scalable Image Storage**: Harbor can store a large number of container images securely and efficiently.
2. **Vulnerability Scanning**: It scans images for vulnerabilities using tools like Clair or Trivy.
3. **Role-Based Access Control**: Harbor allows for fine-grained access control over container images.
4. **Image Signing and Validation**: Ensures that images are from trusted sources.
5. **Replication**: Replicates images across multiple registries for high availability and disaster recovery.
6. **Compliance**: Helps in enforcing security and compliance policies by scanning and signing images.

### Installation and Configuration

#### Prerequisites

- Docker (version 18.06.0+)
- Docker Compose (version 1.18.0+)
- A server or virtual machine with at least 4GB RAM and 2 CPUs

#### Normal Installation

1. **Download Harbor**

   Download the latest version of Harbor from the [Harbor GitHub repository](https://github.com/goharbor/harbor/releases).

   ```bash
   wget https://github.com/goharbor/harbor/releases/download/v2.6.0/harbor-online-installer-v2.6.0.tgz
   tar xvf harbor-online-installer-v2.6.0.tgz
   cd harbor
   ```

2. **Configuration**

   Edit the `harbor.yml` file to configure Harbor according to your requirements. The key configurations include:

   ```yaml
   hostname: your.hostname.com
   http:
     port: 80
   https:
     port: 443
     certificate: /path/to/your/certificate
     private_key: /path/to/your/private_key
   ```

3. **Install Harbor**

   Run the install script:

   ```bash
   sudo ./install.sh
   ```

   This will start Harbor and its components using Docker Compose.

#### Example Usage

1. **Login to Harbor**

   ```bash
   docker login your.hostname.com
   ```

2. **Push an Image**

   Tag your Docker image and push it to Harbor:

   ```bash
   docker tag myapp:latest your.hostname.com/library/myapp:latest
   docker push your.hostname.com/library/myapp:latest
   ```

### Deploying Harbor on Amazon EKS

#### Prerequisites

- An EKS cluster
- kubectl configured to access your EKS cluster
- Helm (version 3+)

#### Steps

1. **Install Helm**

   If Helm is not installed, you can install it using:

   ```bash
   curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash
   ```

2. **Add Harbor Helm Repository**

   ```bash
   helm repo add harbor https://helm.goharbor.io
   helm repo update
   ```

3. **Create Namespace**

   Create a namespace for Harbor:

   ```bash
   kubectl create namespace harbor
   ```

4. **Install Harbor**

   Install Harbor using Helm:

   ```bash
   helm install harbor harbor/harbor --namespace harbor
   ```

   You can customize the installation by creating a `values.yaml` file and passing it to Helm:

   ```yaml
   expose:
     type: ingress
     tls:
       enabled: true
       certSource: secret
   harborAdminPassword: "yourpassword"
   persistence:
     enabled: true
     storageClass: "gp2"
     persistentVolumeClaim:
       registry:
         size: 100Gi
       chartmuseum:
         size: 5Gi
       jobservice:
         size: 1Gi
       database:
         size: 20Gi
       redis:
         size: 2Gi
       trivy:
         size: 5Gi
   ```

   ```bash
   helm install harbor harbor/harbor --namespace harbor -f values.yaml
   ```

5. **Accessing Harbor**

   Once Harbor is installed, you can access it using the provided ingress URL. If you have configured an ingress with a hostname, you should be able to access Harbor at `https://your.hostname.com`.

#### Example Usage in EKS

1. **Login to Harbor**

   ```bash
   docker login your.eks.hostname.com
   ```

2. **Push an Image**

   Tag your Docker image and push it to Harbor:

   ```bash
   docker tag myapp:latest your.eks.hostname.com/library/myapp:latest
   docker push your.eks.hostname.com/library/myapp:latest
   ```

### Conclusion ###

Harbor provides a robust and secure way to manage container images. By deploying it in a normal server environment or on Amazon EKS, you can take advantage of its enterprise-grade features to ensure the security and reliability of your containerized applications.

For more detailed information, refer to the [Harbor documentation](https://goharbor.io/docs/) and the [Harbor GitHub repository](https://github.com/goharbor/harbor).
