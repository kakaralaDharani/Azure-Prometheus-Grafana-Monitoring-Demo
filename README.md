Azure Prometheus & Grafana Monitoring Demo

📌 Project Overview
This project demonstrates a practical Azure-based monitoring solution for a Spring Boot application using Prometheus, Grafana, Node Exporter, and Blackbox Exporter.
The environment consists of two Azure Virtual Machines:

Application Server – hosts the Spring Boot application and Node Exporter.
Monitoring Server – hosts Prometheus, Blackbox Exporter, and Grafana.

Prometheus collects infrastructure and application availability metrics, while Grafana provides dashboards for visualization and monitoring.
The project also demonstrates monitoring an application hosted on an Azure VM and validating application availability through HTTP endpoint monitoring.

🏗️ Architecture

                         GitHub
                           │
                           │
                           ▼
                 Spring PetClinic Application
                           │
                           ▼
              ┌─────────────────────────────┐
              │    Azure Application VM      │
              │                             │
              │ Spring Boot App    :8080    │
              │ Node Exporter      :9100    │
              └──────────────┬──────────────┘
                             │
                 Metrics / HTTP Monitoring
                             │
                             ▼
              ┌─────────────────────────────┐
              │    Azure Monitoring VM      │
              │                             │
              │ Prometheus          :9090   │
              │ Blackbox Exporter   :9115   │
              │ Grafana             :3000   │
              └─────────────────────────────┘
                             │
                             ▼
                    Grafana Dashboard


Monitoring Flow

Application VM
     │
     ├── Spring Boot Application :8080
     │
     └── Node Exporter :9100
             │
             │
             ▼
      Prometheus :9090
             │
             ├── Infrastructure Metrics
             │
             └── Blackbox HTTP Probes
                     │
                     ▼
                Grafana :3000
                     │
                     ▼
              Monitoring Dashboard


🛠️ Technologies Used

Microsoft Azure-Cloud infrastructure
Azure Virtual Machines-Application and monitoring servers
Java-Application runtime
Spring Boot-Sample application
Maven -Application build and packaging
Node Exporter-Linux system metrics
Prometheus -Metrics collection and storage
Blackbox Exporter-HTTP endpoint availability monitoring
Grafana-Metrics visualization and dashboards
GitHub-Application source repository
Linux-Server operating system


☁️ Azure Infrastructure
Two Azure Virtual Machines were created for this project.
VM 1 – Application Server
The Application Server hosts:
Spring Boot application

Node Exporter

Ports used:

8080 → Spring Boot Application
9100 → Node Exporter


VM 2 – Monitoring Server

The Monitoring Server hosts:
Prometheus

Blackbox Exporter

Grafana

Ports used:

9090 → Prometheus
9115 → Blackbox Exporter
3000 → Grafana


🚀 Application Setup

A public Spring Boot application was used for the demonstration.

GitHub repository:
Spring PetClinic

https://github.com/spring-projects/spring-petclinic
The application was cloned into the Application Server.

git clone https://github.com/spring-projects/spring-petclinic.git
cd spring-petclinic


📦 Build the Application Using Maven

The application was packaged using Maven.

./mvnw clean package


After successful compilation and packaging, the generated JAR was available inside the target directory.

ls -lh target/


The application was started using:

java -jar target/*.jar


The Spring Boot application was exposed on:

Port: 8080


The application was then accessed through the browser using the Azure VM public IP and port 8080.



Example:

http://<APPLICATION_VM_PUBLIC_IP>:8080


📊 Node Exporter Setup

Node Exporter was installed on the Application Server to collect Linux system-level metrics.



Node Exporter runs on:

Port: 9100


The metrics endpoint was verified using:

curl http://localhost:9100/metrics


Metrics were also accessed from the browser:

http://<APPLICATION_VM_PUBLIC_IP>:9100/metrics


Example metrics include:

node_cpu_seconds_total
node_memory_MemAvailable_bytes
node_filesystem_avail_bytes


These metrics are later collected by Prometheus.

🔭 Prometheus Setup

Prometheus was installed on the Monitoring Server.



Prometheus runs on:

Port: 9090


Prometheus was accessed through:

http://<MONITORING_VM_PUBLIC_IP>:9090


The Prometheus configuration was updated using:

prometheus.yml


The configuration included monitoring targets for:



Application Server Node Exporter

Spring Boot application endpoint

Blackbox Exporter



Conceptually:

scrape_configs:

  - job_name: 'node-exporter'
    static_configs:
      - targets:
          - '<APPLICATION_VM_PRIVATE_IP>:9100'

  - job_name: 'blackbox-http'
    metrics_path: /probe
    params:
      module: [http_2xx]
    static_configs:
      - targets:
          - 'http://<APPLICATION_VM_PUBLIC_IP>:8080'


After modifying the configuration, Prometheus was restarted/reloaded and the targets were verified from the Prometheus UI.

🌐 Blackbox Exporter

Blackbox Exporter was installed on the Monitoring Server.



Its purpose in this project is to monitor the availability of the Spring Boot application through an HTTP probe.



Blackbox Exporter allows Prometheus to check whether the application endpoint is reachable and responding successfully.



The application endpoint was configured as a monitoring target.



Example:

http://<APPLICATION_VM_PUBLIC_IP>:8080


Blackbox Exporter:

Port: 9115


The monitoring flow is:

Spring Boot Application
          │
          ▼
    HTTP Endpoint
          │
          ▼
 Blackbox Exporter
          │
          ▼
      Prometheus
          │
          ▼
       Grafana


📈 Grafana Setup

Grafana was installed on the Monitoring Server.



Grafana runs on:

Port: 3000


Grafana was accessed using:

http://<MONITORING_VM_PUBLIC_IP>:3000


Prometheus was configured as the Grafana data source.



Configuration:

Grafana
   │
   ▼
Prometheus :9090
   │
   ▼
Collected Metrics


📊 Grafana Dashboard

A Grafana dashboard was created using Prometheus as the data source.



The dashboard was used to visualize monitoring information collected from the environment.



Examples of monitored information:



CPU utilization

Memory utilization

Filesystem metrics

System metrics

Application availability

HTTP probe status

Endpoint response information



The dashboard provides a centralized view of the application server and application availability.

🔗 Monitoring Architecture

The complete monitoring architecture is:

                     ┌──────────────────────┐
                     │       GitHub         │
                     │  Spring PetClinic    │
                     └──────────┬───────────┘
                                │
                                ▼
              ┌────────────────────────────────┐
              │     Azure Application VM       │
              │                                │
              │  Spring Boot App :8080         │
              │                                │
              │  Node Exporter :9100           │
              └───────────────┬────────────────┘
                              │
                 ┌────────────┴────────────┐
                 │                         │
                 ▼                         ▼
          Node Exporter              HTTP Endpoint
                 │                         │
                 │                         ▼
                 │                  Blackbox Exporter
                 │                         │
                 └────────────┬────────────┘
                              │
                              ▼
                    ┌──────────────────┐
                    │    Prometheus    │
                    │      :9090       │
                    └────────┬─────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │     Grafana      │
                    │      :3000       │
                    └────────┬─────────┘
                             │
                             ▼
                    Monitoring Dashboard


🔍 Verification

The following endpoints were successfully verified during the implementation.

Application

http://<APPLICATION_VM_PUBLIC_IP>:8080


Node Exporter

http://<APPLICATION_VM_PUBLIC_IP>:9100/metrics


Prometheus

http://<MONITORING_VM_PUBLIC_IP>:9090


Blackbox Exporter

http://<MONITORING_VM_PUBLIC_IP>:9115


Grafana

http://<MONITORING_VM_PUBLIC_IP>:3000


🛠️ Troubleshooting & Issues Faced

This project also involved troubleshooting real infrastructure and monitoring issues.

1. Node Exporter Process Not Running

Initially, Node Exporter showed the following message:

Listening on address=[::]:9100


However, later the metrics endpoint returned:

curl: (7) Failed to connect to localhost port 9100


The process was checked using:

ps -ef | grep node_exporter


The process was not running, so Node Exporter was started again.

2. Prometheus Connection Refused

Prometheus initially returned:

connection refused


when attempting to access:

20.118.241.58:9090


The Prometheus process and port were verified using:

sudo ss -lntp | grep 9090


and:

ps -ef | grep prometheus


This helped identify whether Prometheus was actively listening on port 9090.

3. Azure VM Shutdown

During the implementation, the Monitoring VM unexpectedly powered off.



The console displayed:

The system will power off now!

The SSH session was subsequently closed.

The VM was started again through Azure and the monitoring services were verified.

This highlighted the importance of checking Azure VM auto-shutdown schedules and VM availability when troubleshooting monitoring connectivity.

4. Application and Monitoring Connectivity

Connectivity between the Application Server and Monitoring Server was verified through the required ports.

The required network paths included:

Application VM :8080
Application VM :9100

Monitoring VM :9090
Monitoring VM :9115
Monitoring VM :3000

Azure network/security rules were configured to allow the required access.



🎯 Project Outcome

Successfully implemented a two-VM monitoring environment on Microsoft Azure.


The final solution provides:

Application Deployment
        +
Infrastructure Monitoring
        +
Application Availability Monitoring
        +
Metrics Collection
        +
Visualization


The project demonstrates how Prometheus and Grafana can be used to monitor both infrastructure-level metrics and application availability in an Azure environment.

👨‍💻 Author
Kakarala Pavithra Dharani
DevOps Engineer | Azure | AWS | Kubernetes | Terraform | CI/CD
GitHub: kakaralaDharani
