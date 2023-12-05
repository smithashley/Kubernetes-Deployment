# Retail Store UI Deployment and Monitoring 
This project consists of the deployment of the UI of an application on an EKS cluster complete with monitoring and logging, and load simulation to test endpoint and auto scaling.

![](https://github.com/smithashley/Retail-Store-UI-Deployment/blob/main/embedded_images/ui.png)

## Steps of Deployment
1.	Create CI/CD pipelines with code scanner, container image scanner
2.	Configure VPC, subnets, Security Groups to reference in cft template
3.	Deploy cluster
    a.	Configure EKS cluster endpoint to be private
    b.	Deploy worker nodes onto private subnets
    c.	Deny all global policy, then allow port 53 egress to kube-system
    d.	RBAC + IAM = IAM roles for service accounts (least privilege)
      i.	Service Accounts for Pods
      ii.	Service Role for Prometheus
      iii.	Service Role for Grafana
      iv.	Admin (will use this access, nonroot user)
      v.	Developers
      vi.	ReadOnly
4.	Install helm
5.	Deploy app on EKS Amazon Linux 2 or Bottlerocket (security first AMIs) with Horizontal or Vertical Pod Auto scaler
    a.	SGs, restrict pod access to instance metadata service, use netpol to restrict network traffic within cluster
6.	Set up monitoring
    a.	Install Prometheus
    b.	Deploy Prometheus operator and services and write CR for monitoring
    c.	Deploy Grafana
7.	Set up LB
    a.	Configure services 
    b.	ALB controller
    c.	Route53
    d.	WAF
8.	Run load simulation using Distributed Load Testing
9.	Document pipeline templates, yaml templates, and quick snap of the app ui, url, and Grafana dashboard
    a.	Monitor k8s control plane Prometheus metrics
    b.	Visualize metrics in Amazon Grafana

## Monitoring
![](https://github.com/smithashley/Retail-Store-UI-Deployment/blob/main/embedded_images/grafana.png)

1.	Describe metrics

## Security
![](https://github.com/smithashley/Retail-Store-UI-Deployment/blob/main/embedded_images/security.png)

With the 4 C’s of cloud native security in mind, security has been integrated:
1.	The CI/CD pipeline for the cluster contains code quality checks and security checks of the template and the CI/CD pipeline for the deployment contains a scan of the image container, code quality checks, and       security checks of the template.
2.	The containers are also secured at the pod level through SGs, restricted pod access to instance metadata service, network policy to restrict network traffic within cluster and RBAC. 
3.	The cluster is also secured by making the endpoint private/Deny all global policy, then allow port 53 egress to kube-system. 
4.	The API is secured in the cloud with a Web Application Firewall.

Additions that would improve the design:
1.	Running AWS Inspector to continually assess vulnerabilities and alignment with security best practices.
2.	Writing audit logs to S3 to monitor the cluster with AWS GuardDuty to detect potentially suspicious activities.
