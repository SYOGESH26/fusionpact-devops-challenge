# Fusionpact DevOps Gauntlet Challenge

## About Me
________________________________________
I am **S. Yogesh**, a DevOps enthusiast skilled in Linux, Git, Docker, GitHub Actions, Kubernetes, and Ansible. I enjoy automating deployment processes and building reliable CI/CD pipelines.

## Procedure I Follow
________________________________________

# Command 1: Fork and Clone Repository
At first, I forked the repository from the given link.  
Clone the repo using:

git clone https://github.com/YogeshS/fusionpact-devops-challenge.git

<img width="775" height="446" alt="image" src="https://github.com/user-attachments/assets/7bcbc819-1de1-4e04-ab26-4c34c69487a9" />


# Command 2: Install Python Virtual Environment and Dependencies


pip install python3-venv
pip install -r backend/requirements.txt

### Command 3: Check Locally using Uvicorn

uvicorn app.main:app --host 0.0.0.0 --port 5000

I Access link: [http://127.0.0.1:5000](http://127.0.0.1:5000)

# Dockerfile Creation and Image Build
# Command 4: Create Dockerfile


vi Dockerfile

# Command 5: Build Docker Image

docker build -t fusionfact .
docker images   # Verify fusionfact is listed
docker run -p 5000:5000 -d fusionfact
curl 127.0.0.1:5000  # Verify running


# Command 6: Push Docker Image to GHCR

docker tag fusionfact ghcr.io/SYOGESH26/fusionfact
docker push ghcr.io/SYOGESH26/fusionfact

<img width="1050" height="307" alt="image" src="https://github.com/user-attachments/assets/83e168e3-6062-4be2-8844-7ce9e92e06f7" />

# Command 7: Configure Kubernetes Manifests

vi Deployment.yml
vi Service.yml

Apply Manifests:

kubectl apply -f Deployment.yml
kubectl apply -f Service.yml

Verify Deployment:

kubectl get pods

# Command 8: Monitoring Setup

# Run cAdvisor

docker run \
  --name=cadvisor \
  --volume=/:/rootfs:ro \
  --volume=/var/run:/var/run:ro \
  --volume=/sys:/sys:ro \
  --volume=/var/lib/docker/:/var/lib/docker:ro \
  --publish=8080:8080 \
  --detach=true \
  --restart=always \
  gcr.io/cadvisor/cadvisor:latest

Verify:

* Open browser: [http://localhost:8080](http://localhost:8080) → cAdvisor UI should appear

# Configure Prometheus

Create `prometheus.yml`:

global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'cadvisor'
    static_configs:
      - targets: ['localhost:8080']

PromQL Query Example (Memory usage % of container limit):


(container_memory_usage_bytes{container!="POD"} / container_spec_memory_limit_bytes{container!="POD"}) * 100

* Open Prometheus UI → Graph tab → Enter query → Execute → view memory usage %

### Command 9: Visual Dashboard in Grafana

1. Access Grafana Dashboard: [http://127.0.0.1:3000](http://127.0.0.1:3000)
2. Add Prometheus Data Source:

   * Grafana → Settings → Data Sources → Add → Prometheus
   * URL: `http://172.28.164.58:9090`
   * Click **Save & Test**
3. Import cAdvisor Dashboard:

   * Enter Dashboard ID: `14282`
   * Select Prometheus data source → Click Import
4. Monitor Metrics:

   * View container CPU, memory, network, and I/O metrics
   * Real-time monitoring via cAdvisor

 <img width="1050" height="438" alt="image" src="https://github.com/user-attachments/assets/118e38c4-02fb-4c5c-b181-69b0c3067622" />

