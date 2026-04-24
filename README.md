# Monitoring Project

## Task: Set Up a Monitoring Environment with Prometheus and Blackbox Exporter

Task: Set Up a Monitoring Environment with Prometheus and Blackbox Exporter

Welcome! Over the next week, you'll get hands-on experience with industry-standard monitoring tools. Your main goal is to set up a Prometheus server running inside a Docker container to monitor an external service's availability using the Blackbox Exporter.
This project will give you a great introduction to how modern systems ensure they are up and running.

### 🎯 Your Goal
By the end of this week, you will have:
    1. A running Blackbox Exporter that can probe HTTP endpoints.
    2. A running Prometheus server in a Docker container.
    3. A Prometheus instance that is successfully scraping metrics (data) from the Blackbox Exporter.
    4. A dashboard in Prometheus where you can see if a target service (like google.com) is up or down.

### 🗺️ The Plan: Step-by-Step Instructions
Follow these steps to complete the project.
Step 1: Understand the Tools
    • Prometheus: An open-source monitoring and alerting toolkit. It collects and stores its metrics as time series data, i.e., metrics information is stored with the timestamp at which it was recorded.
    • Blackbox Exporter: Prometheus exporters are tools that expose metrics from third-party systems. The Blackbox Exporter is unique because it allows you to probe endpoints over protocols like HTTP, HTTPS, DNS, TCP, and ICMP. Instead of monitoring the internal state of a service, it monitors it from the outside—like a user would. We will use it to check if a website is online.
## Step 2: Set Up the Blackbox Exporter
First, you'll run the Blackbox Exporter on your local machine.9115
    1. Download: Go to the Prometheus downloads page and download the correct version for your operating system.
    2. Configure: Create a configuration file named blackbox.yml in the same directory. This file tells the exporter how to probe a target. For now, we'll just set up a simple HTTP probe.
    3. Run: Open a terminal, navigate to the directory, and start the exporter using the configuration file:
    ```
    ./blackbox_exporter --config.file=blackbox.yml
    ```
   ## 4. Verify: Open your web browser and go to http://localhost:9115. You should see the Blackbox Exporter's web page. This means it's running!
Step 3: Set Up Prometheus in a Docker Container
Next, you'll set up the main monitoring server, Prometheus.
    1. Create a Configuration File: Prometheus needs to know what to monitor. Create a file named prometheus.yml.
        ◦ Important for Docker: When Prometheus is inside a Docker container, localhost refers to the container itself, not your host machine. To reach the Blackbox Exporter running on your host, you need to use a special Docker DNS name: host.docker.internal.


uld not be found.

Das bedeutet ganz simpel: Grafana wurde noch nicht erfolgreich installiert (oder der Installationsschritt hat nicht richtig gegriffen).

## 🔧 Schritt 1: Prüfen, ob Grafana wirklich installiert ist
Gib mal ein:

Bash
```￼
dpkg -l | grep grafana
```
Wenn da nichts kommt → Installation ist nicht durchgelaufen.

## 🔧 Schritt 2: Grafana sauber installieren
Mach nochmal explizit:

Bash
    2. Run Prometheus with Docker: Open a new terminal. Use the following command to run Prometheus in a container. This command also mounts your prometheus.yml file into the container.
    3. # Make sure you run this command from the directory where prometheus.yml is located
    4. docker run ```--name prometheus -p 9090:9090 -v $(pwd)/prometheus.yml:/etc/prometheus/prometheus.yml prom/prometheus```
## Step 4: Verify the Full Setup
Now let's see if everything is working together.
    1. Prometheus UI: Open your browser and go to ```http://localhost:9090```. This is the Prometheus dashboard.
    2. Check Targets: In the top menu, navigate to Status > Targets. You should see two jobs: prometheus and blackbox. Both should have a green "UP" state. This confirms that Prometheus is successfully scraping itself and the Blackbox Exporter.
    3. Query the Data: Click on the graph icon ("Graph" in the top menu). In the expression bar, you can now query the metrics. Try this query:
    4. probe_success
Press execute. You should see results for the targets you defined (http://google.com and https://bosch.com). A value of 1 means the probe was successful (the site is up), and 0 means it failed.

🎉 Congratulations!
If you've reached this point, you have successfully built a monitoring pipeline! You have a Prometheus server collecting data from a Blackbox Exporter, which is checking the health of external websites.
This is a foundational skill for anyone in software engineering or IT operations.


Einleitung grafana: 

Grafana ist ein tool, womit man die Daten von Server-Metriken , Datenbanken… visualisierst. Es wird häufig genutzt um Monotoring Systeme zu erstellen.

Grafana Installation:
## 1. Stelle sicher, dass alle wichtige Tools installiert sind. Mit dem Code:

```
sudo apt update
sudo apt install -y apt-transport-https wget gnupg
```


￼
Basic CPU / Mem / Net / Disk2.  Damit Ubuntu die Grafana-Pakete als vertrauenswürdig erkennt:

```
sudo mkdir -p /etc/apt/keyrings
wget -q -O - https://apt.grafana.com/gpg.key | sudo tee /etc/apt/keyrings/grafana.asc > /dev/null
sudo chmod 644 /etc/apt/keyrings/grafana.asc
```

## 3. Wie Grafana auf die Pakete zugreift

```echo "deb [signed-by=/etc/a```

Basic CPU / Mem / Net / Diskpt/keyrings/grafana.asc] https://apt.grafana.com stable main" | sudo tee /etc/apt/sources.list.d/grafana.list

## 4.  Installierung der neuen Paketquellen:

```sudo apt update```

## 5.  Grafana installieren

```sudo apt install grafana```



## 6. Grafana starten:

```
sudo systemctl enable grafana-server
sudo systemctl start grafana-server
```

##7.  Grafana richtig starten

```systemctl status grafana-server```

##8. Im Browser eingeben:
```http://localhost:3000```

User: Admin
Passwort: Admin
##9. Dort dann alle dashboards importieren: Prometheus, Blackbox exporter und node exporter








Prometheus einstellen:
##1. Download & Entpacken

```cd /tmp
wget https://github.com/prometheus/prometheus/releases/latest/download/prometheus-*.linux-amd64.tar.gz
tar xvf prometheus-*.tar.gz
cd prometheus-*```

##2. Verschieben (optional sauber)

```sudo mv prometheus promtool /usr/local/bin/
sudo mkdir /etc/prometheus
sudo mv prometheus.yml /etc/prometheus/```

##3. Minimal-Konfiguration prüfen
Datei:
```/etc/prometheus/prometheus.yml
Standard reicht erstmal:
global:
  scrape_interval: 15s
scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']```

##4. Starten
prometheus ```--config.file=/etc/prometheus/prometheus.yml```

##5. Web UI öffnen
Browser:
```http://localhost:9090```

##6. (Optional) Als Service einrichten
```sudo nano /etc/systemd/system/prometheus.service```
Inhalt:
```[Unit]
Description=Prometheus
After=network.target
[Service]
ExecStart=/usr/local/bin/prometheus --config.file=/etc/prometheus/prometheus.yml
Restart=always
[Install]
WantedBy=multi-user.target```
Dann:
```sudo systemctl daemon-reexec
sudo systemctl daemon-reload
sudo systemctl enable prometheus
sudo systemctl start prometheus```

##7. Status check
```systemctl status prometheus```


###Blackbox exporter einstellen
##1. Download & Entpacken
```cd /tmp
wget https://github.com/prometheus/blackbox_exporter/releases/latest/download/blackbox_exporter-*.linux-amd64.tar.gz
tar xvf blackbox_exporter-*.tar.gz
cd blackbox_exporter-*```

##2. Installieren
```sudo mv blackbox_exporter /usr/local/bin/
sudo mkdir /etc/blackbox_exporter
sudo mv blackbox.yml /etc/blackbox_exporter/```

##3. Konfiguration
Datei:
```/etc/blackbox_exporter/blackbox.yml
Standard reicht erstmal (HTTP check):
modules:
  http_2xx:
    prober: http
    timeout: 5s
    http:
      valid_status_codes: []```

##4. Starten
blackbox_exporter --config.file=/etc/blackbox_exporter/blackbox.yml
Web:
```http://localhost:9115```

##5. In Prometheus einbinden
Datei bearbeiten:
sudo nano /etc/prometheus/prometheus.yml
Hinzufügen:
```- job_name: 'blackbox'
  metrics_path: /probe
  params:
    module: [http_2xx]
  static_configs:
    - targets:
      - https://example.com
      - https://google.com
  relabel_configs:
    - source_labels: [__address__]
      target_label: __param_target
    - source_labels: [__param_target]
      target_label: instance
    - target_label: __address__
      replacement: localhost:9115```

##6. Prometheus neu starten
sudo systemctl restart prometheus

##7. Test
Im Browser:
```http://localhost:9090```
#Query:
probe_success



Node Exporter einstellen
##1. Download & Entpacken
```cd /tmp
wget https://github.com/prometheus/node_exporter/releases/latest/download/node_exporter-*.linux-amd64.tar.gz
tar xvf node_exporter-*.tar.gz
cd node_exporter-*```

##2. Installieren
```sudo mv node_exporter /usr/local/bin/```

##3. Starten
node_exporter
Web prüfen:
```http://localhost:9100/metrics```

##4. In Prometheus einbinden
Datei:
```sudo nano /etc/prometheus/prometheus.yml
Hinzufügen:
- job_name: 'node'
  static_configs:
    - targets: ['localhost:9100']```

##5. Prometheus neu starten
```sudo systemctl restart prometheus```


##6. Test
Im Browser:
```http://localhost:9090```
Query:
```node_cpu_seconds_total```

##7. Als Service
```sudo nano /etc/systemd/system/node_exporter.service```
```[Unit]
Description=Node Exporter
After=network.target
[Service]
ExecStart=/usr/local/bin/node_exporter
Restart=always
[Install]
WantedBy=multi-user.target```
#Dann:
```sudo systemctl daemon-reload
sudo systemctl enable node_exporter
sudo systemctl start node_exporter```








#Sobald der blackbox exporter failed beim starten solltest du das machen: 

```sudo -i```
```kill 1275```
```exit```
```ps aux  grep blackbox```
```systemctl restart blackbox_exporter```
systemctl status blackbox_exporter
