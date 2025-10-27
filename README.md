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
  ✅ Deplo Redis via Helm Chart.<br>
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
1. Use EKS service from Demo 1 to deploy redis app.
2. Use the config-microoservice.yaml to deploy redis. The file used in the kubernetes module.
3. Verify that the the Redids pod is working correctly.

## Install Prometheus  Redis Exporter
5. Add the Prometheus helm repository
6. Update the helm repository.
7. Install the Redis Exporter chart.
   details><summary><strong>Prometheus Redis Exporter:</strong></summary>
   Prometheus Redis Exporter is a tool that collects metrics from a Redis instance and exposes them in a format that Prometheus can scrape and monitor
   </details>
8. Verify that the Redis Exporter Pods is running.
9. Access Prometheus Web UI and verify that the Redis exporter target has been added.

## Create Redis PrometheusRules
10. Add the PrometheusRules to a redis-rules.yaml file.
    details><summary><strong>Prometheus Rules for 3er PArty Apps: Exporter:</strong></summary>
   You can always use predefined dashboards for third-party apps. [Prometheus Rules 3rd Party Apps](https://samber.github.io/awesome-prometheus-alerts/rules#redis)
   </details>
12. Apply the redis-rules.yaml file
13. Verify that the rules has been added to the Prometheus Alerts.

## Create Dashboard.
14. Access Grafana dashboards
15. Search for the Redis Exporter Dashboard
16. Copy the Dashboard ID
17. Create a New Dashboard
18. Import the Redis Exporter dashboard using the dashbaord ID.
19. Verify that the redis-exporter endpoint match the dashboard.
20. Verity the dashboard.
