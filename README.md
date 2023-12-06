# Retail Store UI Deployment and Monitoring 
This project consists of the deployment of the UI of a secure application on an EKS cluster complete with monitoring of metrics, and load simulation to test monitoring and auto scaling. 
![](https://github.com/smithashley/Retail-Store-UI-Deployment/blob/main/embedded_images/ui.png)

## Details of the Deployment
- Create CI/CD pipelines for the deployment of the Kubernetes cluster, UI of the app, Prometheus, Grafana, VPC, and ALB
- Configure VPC, Subnets, and Security Groups 
- Deploy cluster
    - Configure EKS cluster with private endpoint
    - Deploy worker nodes onto private subnets
    - Deny all global policy, then allow port 53 egress to kube-system
    - Set up Role Based Access Control using IAM roles for service accounts (least privilege)
      - Service Accounts for Pods
      - Service Role for Prometheus
      - Service Role for Grafana
      - Admin 
      - Developers
      - ReadOnly
- Deploy UI 
    - Horizontal or Vertical Pod Auto scaler
    - SGs, restrict pod access to instance metadata service, use netpol to restrict network traffic within cluster
- Set up monitoring
    - Install Prometheus
    - Deploy Prometheus operator and services and write CR for monitoring
    - Deploy Grafana
- Set up LB
    - Configure service
    - ALB controller 
    - Route53
    - WAF
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
