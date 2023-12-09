# Static Website Deployment and Monitoring 
This project consists of the deployment of a static website on an EKS cluster complete with monitoring of metrics, and load simulation to test monitoring and auto scaling. 

![](https://github.com/smithashley/Tea-Store/blob/main/embedded_images/website.PNG)

## Steps
- Configured VPC, Subnets, and Security Groups 
- Deployed cluster
    - Configure EKS cluster within private endpoint
- Installed Helm
- Deployed ArgoCD using Helm
- Deployed Prometheus operator using Helm
- Deployed static website using ArgoCD
  
```
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: website-app
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/smithashley/Tea-Store.git
    targetRevision: HEAD
    path: website
  destination:
    server: https://kubernetes.default.svc
    namespace: staging
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
      allowEmpty: false     
    - Horizontal or Vertical Pod Auto scaler
```
    
- Deployed application load balancer controller using Helm
- Run load simulation using Distributed Load Testing
    - https://aws.amazon.com/solutions/implementations/distributed-load-testing-on-aws/

## Monitoring
![](https://github.com/smithashley/Retail-Store-UI-Deployment/blob/main/embedded_images/grafana.png)

- Describe metrics

## Security
![](https://github.com/smithashley/Retail-Store-UI-Deployment/blob/main/embedded_images/security.png)

With the 4 C’s of cloud native security in mind, security has been integrated at every layer of the infrastructure:
- The CI/CD pipeline for the cluster contains code quality checks and security checks of the template and the CI/CD pipeline for the deployment contains a scan of the image container, code quality checks, and security checks of the template.
- The containers are also secured at the pod level through Security Groups, restricted pod access to instance metadata service, network policy to restrict network traffic within cluster, and Role Based Access Control. 
- The cluster is also secured by making the endpoint private/Deny all global policy, then allow port 53 egress to kube-system. 
- The API is secured in the cloud with a Web Application Firewall.

Additions that would improve the design:
- Running AWS Inspector to continually assess vulnerabilities and alignment with security best practices.
- Writing audit logs to S3 to monitor the cluster with AWS GuardDuty and detect potentially suspicious activities.
