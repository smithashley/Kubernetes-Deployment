# Retail Store UI Deployment and Monitoring 
This project consists of the deployment of the UI of an application on an EKS cluster complete with monitoring and logging, and load simulation to test endpoint and auto scaling.

![](https://github.com/smithashley/Retail-Store-UI-Deployment/blob/main/embedded_images/ui.png)

## Details of the Deployment
- Create CI/CD pipelines with code scanner, container image scanner
- Configure VPC, subnets, Security Groups to reference in cft template
- Deploy cluster
    - Configure EKS cluster endpoint to be private
    - Deploy worker nodes onto private subnets
    - Deny all global policy, then allow port 53 egress to kube-system
    - RBAC + IAM = IAM roles for service accounts (least privilege)
      - Service Accounts for Pods
      - Service Role for Prometheus
      - Service Role for Grafana
      - Admin (will use this access, nonroot user)
      - Developers
      - ReadOnly
- Install helm
- Deploy app on EKS Amazon Linux 2 or Bottlerocket (security first AMIs) with Horizontal or Vertical Pod Auto scaler
    - SGs, restrict pod access to instance metadata service, use netpol to restrict network traffic within cluster
- Set up monitoring
    - Install Prometheus
    - Deploy Prometheus operator and services and write CR for monitoring
    - Deploy Grafana
- Set up LB
    - Configure services 
    - ALB controller
    - Route53
    - WAF
- Run load simulation using Distributed Load Testing
- Document pipeline templates, yaml templates, and quick snap of the app ui, url, and Grafana dashboard
    - Monitor k8s control plane Prometheus metrics
    - Visualize metrics in Amazon Grafana

## Monitoring
![](https://github.com/smithashley/Retail-Store-UI-Deployment/blob/main/embedded_images/grafana.png)

- Describe metrics

## Security
![](https://github.com/smithashley/Retail-Store-UI-Deployment/blob/main/embedded_images/security.png)

With the 4 C’s of cloud native security in mind, security has been integrated:
- The CI/CD pipeline for the cluster contains code quality checks and security checks of the template and the CI/CD pipeline for the deployment contains a scan of the image container, code quality checks, and       security checks of the template.
- The containers are also secured at the pod level through SGs, restricted pod access to instance metadata service, network policy to restrict network traffic within cluster and RBAC. 
- The cluster is also secured by making the endpoint private/Deny all global policy, then allow port 53 egress to kube-system. 
- The API is secured in the cloud with a Web Application Firewall.

Additions that would improve the design:
- Running AWS Inspector to continually assess vulnerabilities and alignment with security best practices.
- Writing audit logs to S3 to monitor the cluster with AWS GuardDuty to detect potentially suspicious activities.
