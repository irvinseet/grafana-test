
## Getting Started
1. Create a persistent volume for your data
```
docker volume create grafana-storage
```

2. Verify that the volume was created correctly, you should see a json output
```
docker volume inspect grafana-storage
```

3. Start grafana
```
docker run -d -p 3000:3000 --name=grafana --volume grafana-storage:/var/lib/grafana  -e “GF_INSTALL_PLUGINS=marcusolsson-json-datasource 1.3.3” grafana/grafana-enterprise
```
- docker run: run a container from a Docker image.
- -d: container to run in detached mode, meaning it runs in the background.
- -p 3000:3000: binds port 3000 of the host machine to port 3000 of the container. Grafana's web interface is accessible on port 3000.
- --volume grafana-storage:/var/lib/grafana: create a volume named "grafana-storage" and mounts it to the /var/lib/grafana directory inside the container. The volume provides persistent storage for Grafana's data.
- -e "GF_INSTALL_PLUGINS=marcusolsson-json-datasource 1.3.3": installs "marcusolsson-json-datasource" plugin version 1.3.3, which supports JSONPath [?()] eval expression.
- grafana/grafana-enterprise: The recommended and default edition of Grafana is Grafana Enterprise. It is free and includes all the features of the OSS edition.

4. Go to http://localhost:3000/ username/password should be admin/admin
5. Create a dashboard and import template (if available)
6. Connect datasource to API
