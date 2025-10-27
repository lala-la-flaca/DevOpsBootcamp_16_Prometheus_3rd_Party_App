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
   
5. Update the Helm repository.
   ```bash
   helm repo update
   ```
   <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_16_Prometheus_3rd_Party_App/blob/main/Img/2%20add%20the%20prometheus%20repository%20and%20update%20it.PNG" width=800/>
   
6. Create the Redis Values file.
   ```bash
    serviceMonitor:
    # When set true then use a ServiceMonitor to configure scraping
      enabled: true
      labels:
        release: monitoring
        redisAddress: redis://redis-cart:6379
   ```
   <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_16_Prometheus_3rd_Party_App/blob/main/Img/14%20Redis%20Values%20helm.PNG" width=800 />
   
8. Install the Redis Exporter chart using the values file. [Redis Exporter Helm](https://github.com/prometheus-community/helm-charts/tree/main/charts/prometheus-redis-exporter)
   
   <details><summary><strong>Prometheus Redis Exporter:</strong></summary>
     Prometheus Redis Exporter is a tool that collects metrics from a Redis instance and exposes them in a format that Prometheus can scrape and monitor
   </details>
   
   ```
     helm install redis-exporter prometheus-community/prometheus-redis-exporter -f redis-values.yaml
   ```
   
   <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_16_Prometheus_3rd_Party_App/blob/main/Img/3%20install%20chart.PNG" width=800/>
   
9. Verify that the Redis Exporter Pod is running.
    
    ```bash
    kubectl get pods
    ```
    <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_16_Prometheus_3rd_Party_App/blob/main/Img/4%20check%20redis%20exporter%20pod.png" width=800/>
    
10. Access Prometheus Web UI and confirm that the Redis exporter target appears in the target list.
    <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_16_Prometheus_3rd_Party_App/blob/main/Img/5%20new%20target%20dded.png" width=800/>


## Create Redis PrometheusRules
09. Create a redis-rules.yaml file and Add the PrometheusRules for Redis.

    <details><summary><strong>Prometheus Rules for 3rd-Party Apps: Exporter:</strong></summary>
      You can use predefined dashboards and alert rules for Redis and other third-party applications.<br>
      <a href="https://samber.github.io/awesome-prometheus-alerts/rules#redis" target="_blank">Prometheus Rules 3rd-Party Apps</a>
    </details>
    
   ```
        apiVersion: monitoring.coreos.com/v1
        kind: PrometheusRule
        metadata:
          name: redis-rules
          labels:
            app: kube-prometheus-stack
            release: monitoring
        spec:
            groups:
              - name: redis.rules
                rules:
                #Rules available at: https://samber.github.io/awesome-prometheus-alerts/rules#redis
                - alert: RedisDown
                  expr: redis_up == 0
                  for: 0m
                  labels:
                    severity: critical
                  annotations:
                    summary: Redis down (instance {{ $labels.instance }})
                    description: "Redis instance is down\n  VALUE = {{ $value }}\n  LABELS = {{ $labels }}"
                
                - alert: RedisTooManyConnections
                  expr: redis_connected_clients / redis_config_maxclients * 100 > 90
                  for: 2m
                  labels:
                    severity: warning
                  annotations:
                    summary: Redis too many connections (instance {{ $labels.instance }})
                    description: "Redis is running out of connections (> 90% used)\n  VALUE = {{ $value }}\n  LABELS = {{ $labels }}" 
   ```
   <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_16_Prometheus_3rd_Party_App/blob/main/Img/15%20Redis%20Rules%20yaml.png" width=800/>
   
10. Apply the Redis rules file.
    ```bash
    kubectl apply -f redis-rules.yaml
    ```
    <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_16_Prometheus_3rd_Party_App/blob/main/Img/6%20applying%20redis%20rules.png" width=800/>
    
11. Verify that the rules has been added to the Prometheus Alerts page.
    <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_16_Prometheus_3rd_Party_App/blob/main/Img/7%20rdis%20rules%20available%20prometheus%20alerts.PNG" width=800/>


## Create Dashboard.
12. Access Grafana dashboards.

    <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_16_Prometheus_3rd_Party_App/blob/main/Img/8%20adding%20a%20new%20dashbaord.png" width=800/>
    
13. Search for the Redis Exporter Dashboard.

    <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_16_Prometheus_3rd_Party_App/blob/main/Img/9%20search%20the%20desired%20dashboard.png" width=800/>
    
14. Copy the Dashboard ID.
    
15. Create a New Dashboard.
    
16. Import the Redis Exporter dashboard using the dashbaord ID.

    <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_16_Prometheus_3rd_Party_App/blob/main/Img/10%20dashboard%20id.png" width=800/>
    
17. Verify that the redis-exporter endpoint match the dashboard data.

    <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_16_Prometheus_3rd_Party_App/blob/main/Img/12%20endpoint%20redis%20exporter.png" width=800/>
    
18. Confirm that the dashboard displays metrics correctly.

    <img src="https://github.com/lala-la-flaca/DevOpsBootcamp_16_Prometheus_3rd_Party_App/blob/main/Img/13%20dashbard%202.PNG" width=800/>
