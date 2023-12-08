
# Installing Prometheus --

docker volume create prometheus-data

mkdir ~/prometheus-config

docker run -d \
    -p 9090:9090 \
    -v ~/prometheus-config:/etc/prometheus/ \
    -v prometheus-data:/prometheus \
    prom/prometheus

cd ~/prometheus-config

rm -rf prometheus.yml   

vi prometheus.yml 

# Installing Grafana --

# create a persistent volume for your data
docker volume create grafana-storage

# start grafana
docker run -d -p 3000:3000 --name=grafana \
  --volume grafana-storage:/var/lib/grafana \
  grafana/grafana-oss

Default username - admin and password - admin

# Installing Node Exporter --

wget https://github.com/prometheus/node_exporter/releases/download/v<VERSION>/node_exporter-<VERSION>.<OS>-<ARCH>.tar.gz
tar xvfz node_exporter-*.*-amd64.tar.gz
cd node_exporter-*.*-amd64
./node_exporter


docker run -d \
    -p 9100:9100 \
  --net="host" \
  --pid="host" \
  -v "/:/host:ro,rslave" \
  quay.io/prometheus/node-exporter:latest \
  --path.rootfs=/host

