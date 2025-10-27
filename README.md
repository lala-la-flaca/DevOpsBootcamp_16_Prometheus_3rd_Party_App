# <img width="250" height="250" alt="image" src="https://github.com/lala-la-flaca/DevOpsBootcamp_16_Prometheus/blob/main/Img/1a4f23660b53de848a6a721e3719b077.jpg"/> Module 16 – Monitoring with Prometheus

This exercise is part of Module 16 from the TWN DevOps Bootcamp. In Module 16, we learn how to monitor applications and infrastructure with Prometheus. This module covers everything from installing the Prometheus and Grafana stack on Kubernetes to setting up alerting and visual dashboards. We also learn how to monitor third-party apps such as Redis and collect custom metrics from your own applications. By the end of this module, you know how to build a complete monitoring solution that detects issues early, sends alerts, and visualizes performance data in real time.

---
<a id="demo2"></a>
# 🚨 Demo 3 –  Configure Monitoring for 3rd Party App
# 📌 Objective
Monitor a Redis service running in Kubernetes using Prometheus and Grafana dashboards.

# 🚀 Technologies Used
* Prometheus: Metrics collection and monitoring system.
* Grafana: Visualization and dashbaord tool.
* Linux: OS.
* Kubernetes: Container Orchestration platform.
* Helm: Package Manager for Kuebrnetes.
* AWS EKS: Managed Kubernetes Cluster.
* Terraform: To deploy K8 infrastructure.
* Redis: Target application for monitoring.

# 🎯 Features
  ✅ Deploy Redis and Redis Exporter via Helm Chart.<br>
  📁 Add a Redis dashboard to Grafana.<br>
  ⚙️ Sets up rules for redis downtime or high connections.<br>
  
# Prerequisites
* AWS account with valid keys.
* EKS demo from terraform module.
* Microoservices Demo from kubernetes module.
* Prometheus Demo1.
  
# 🏗 Project Architecture
<img src=""/>

# ⚙️ Project Configuration
## Redis Pod
1. Use the EKS service from Demo 1 to deploy the Redis application.
2. Deploy redis using the config-microoservice.yaml file from the Kubernetes module.
3. Verify that the Redids pod is working correctly.
   ```bash
   kubectl get pods
   ```

## Install Prometheus  Redis Exporter
4. Add the Prometheus Helm repository.
   
   ```bash
   helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
   ```
   <img src="" width=800/>
   
5. Update the Helm repository.
   ```bash
   helm repo update
   ```
   <img src="" width=800/>
   
6. Install the Redis Exporter chart in the monitoring namespace.
   
   details><summary><strong>Prometheus Redis Exporter:</strong></summary>
     Prometheus Redis Exporter is a tool that collects metrics from a Redis instance and exposes them in a format that Prometheus can scrape and monitor
   </details>
   
   <br>
   ```bash
   helm install redis-exporter prometheus-community/prometheus-redis-exporter -n monitoring
   ```
   <img src="" width=800/>
   
8. Verify that the Redis Exporter Pod is running.
    
    ```bash
    kubectl get pods -n monitoring
    ```
    <img src="" width=800/>
    
9. Access Prometheus Web UI and confirm that the Redis exporter target appears in the target list.
    <img src="" width=800/>

## Create Redis PrometheusRules
09. Create a redis-rules.yaml file and Add the PrometheusRules for Redis.
    details><summary><strong>Prometheus Rules for 3er PArty Apps: Exporter:</strong></summary>
    you can use predefined dashboards and alert rules for Redis and other third-party applications [PrometheusRules 3rd-Party Apps](https://samber.github.io/awesome-prometheus-alerts/rules#redis)
   </details>
   ```bash
   ```
   <img src="" width=800/>
   
10. Apply the Redis rules file.
    ```bash
    kubectl apply -f redis-rules.yaml -n monitoring
    ```
    <img src="" width=800/>
    
11. Verify that the rules has been added to the Prometheus Alerts page.
    <img src="" width=800/>

## Create Dashboard.
12. Access Grafana dashboards.
    <img src="" width=800/>
    
13. Search for the Redis Exporter Dashboard.
    <img src="" width=800/>
    
14. Copy the Dashboard ID.
    <img src="" width=800/>
    
15. Create a New Dashboard.
    <img src="" width=800/>
    
16. Import the Redis Exporter dashboard using the dashbaord ID.
    <img src="" width=800/>
    
17. Verify that the redis-exporter endpoint match the dashboard data.
    <img src="" width=800/>
    
18. Confirm that the dashboard displays metrics correctly.
    <img src="" width=800/>
