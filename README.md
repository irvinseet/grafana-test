
## Getting Started
This guide assumes you are hosting an API from another Docker container, which can be accesed via http://172.17.0.1:8080/report.json from your Grafana Docker container.

1) Create a persistent volume for your data
```
docker volume create grafana-storage
```

2) Verify that the volume was created correctly, you should see a json output
```
docker volume inspect grafana-storage
```

3a) Start grafana
```
docker run  -d  -p 3000:3000 --name=grafana \
--volume grafana-storage:/var/lib/grafana \
-e "GF_INSTALL_PLUGINS=yesoreyeram-infinity-datasource" \
-e "GF_AUTH_ANONYMOUS_ENABLED=true" \
grafana/grafana
```
- `docker run`: run a container from a Docker image.
- `-d`: container to run in detached mode, meaning it runs in the background.
- `-p 3000:3000`: binds port 3000 of the host machine to port 3000 of the container. Grafana's web interface is accessible on port 3000.
- `--volume grafana-storage:/var/lib/grafana`: create a volume named "grafana-storage" and mounts it to the /var/lib/grafana directory inside the container. The volume provides persistent storage for Grafana's data.
- `-e "GF_INSTALL_PLUGINS=yesoreyeram-infinity-datasource"`: installs latest versions of Infinity Datasource plugin. You may specify the version, for example `yesoreyeram-infinity-datasource x.x`
- `-e "GF_AUTH_ANONYMOUS_ENABLED=true"`: Allows anonymous to view dashboard without logging in.
- `grafana/grafana`: You may specify the version, for example `grafana/grafana:8.2.6`

3b) Alternatively, run `docker compose up -d` in the same folder as docker-compose.yaml

### Grafana configuration
1) Go to http://localhost:3000/. Username/password should be admin/admin
2) Press side menu button on top right corner (☰) > Connections > Add new data source
    - Select JSON API, set URL to  `http://172.17.0.1:8080/report.json`, > Save & Test
    - Select Infinity, > Save and Test
3) Create a dashboard and import template.json dashboard, select respective data sources.
4) (if necessary) For panels using Infinity data source, set URL to `http://172.17.0.1:8080/report.json`

### More Information
- Grafana Docker Image - https://grafana.com/docs/grafana/latest/setup-grafana/installation/docker/
- Grafana Infinity Datasource plugin - https://sriramajeyam.com/grafana-infinity-datasource/
- UQL and JSONata - https://sriramajeyam.com/grafana-infinity-datasource/wiki/uql
