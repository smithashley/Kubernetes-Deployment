# Static Website Deployment and Monitoring 
This project consists of the deployment of a static website on an EKS cluster complete with monitoring of metrics, and load simulation to test monitoring and auto scaling. 

![](https://github.com/smithashley/Tea-Store/blob/main/embedded_images/website.PNG)

## Steps
- Configured VPC, Subnets, and Security Groups using CloudFormation
- Deployed EKS cluster using CloudFormation
- Created namespaces to isolate resources
- Installed Helm
    - https://helm.sh/ 
- Installed Helm chart for ArgoCD 
    - https://artifacthub.io/packages/helm/argo/argocd-apps/
- Installed Helm chart for the Prometheus operator
    -  https://artifacthub.io/packages/helm/prometheus-community/kube-prometheus-stack/
- Deployed static website using ArgoCD
- This deployment is complete with a horizontal pod autoscaler, service, service monitor, and ingress
![](https://github.com/smithashley/Kubernetes-Deployment-1/blob/main/embedded_images/argo-app.PNG)
  - Custom Object
 
```
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: website-app
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/smithashley/Kubernetes-Deployment-1.git
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
```
    
- Created Service Account for the Load Balancer
- Installed Helm chart for the AWS Load Balancer Controller
    - https://artifacthub.io/packages/helm/aws/aws-load-balancer-controller/ 
- Ran load test to simulate traffic
    - https://aws.amazon.com/solutions/implementations/distributed-load-testing-on-aws/ ???

## Monitoring
![](https://github.com/smithashley/Retail-Store-UI-Deployment/blob/main/embedded_images/grafana.png)

### Key symptoms to monitor Latency, Errors, and Traffic:
- Resource utilization: Monitor CPU, memory, and disk usage trends for pods, containers, and nodes. Alert on anomalies that deviate from normal patterns, indicating potential resource bottlenecks.
- Application performance: Track metrics like response times, error rates, and request latencies for your applications. Alert on rising trends or spikes, indicating potential performance degradation.
- Pod health: Monitor pod liveness and readiness probes to ensure pods are healthy and functioning. Alert on unhealthy pods to identify potential application issues or infrastructure problems.
- Configuration changes: Monitor for configuration changes made to deployments, pods, and services. Alert on unexpected changes to identify potential errors or security risks.

Proactive approach to monitor the cluster for symptoms that indicate potential issues before they escalate into outages:
- Define alert thresholds and rules:
    - Set thresholds based on historical data and expected behavior.
    - Define alert rules in Prometheus to trigger alerts when thresholds are crossed.
    - Configure Alertmanager to route alerts to the appropriate channels (e.g., Slack, email, PagerDuty).
- Set up automated workflows to trigger remediation actions based on specific alerts.
- Prioritize alerts based on severity:
    - Utilize multi-level alerting based on severity.
    - Focus on higher-severity alerts that require immediate attention.
    - Investigate and resolve lower-severity alerts before they escalate.
- Review:
    - Regularly review the effectiveness of your monitoring system and alert rules.
    - Analyze historical data to identify potential gaps and improve symptom detection.
    - Refine your thresholds and rules based on observed behavior and operational needs.
Benefits:
- Improved incident response times
- Proactive problem identification and resolution
- Reduced downtime and service disruptions
- Increased application performance and reliability
- Enhanced visibility into your Kubernetes environment

Additions that would improve the design:
- Running AWS Inspector to continually assess vulnerabilities and alignment with security best practices.
- Writing audit logs to S3 to monitor the cluster with AWS GuardDuty and detect potentially suspicious activities.
