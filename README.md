# Monitoreo de recursos y red con Prometheus & Grafana

# Primer paso: "Descargar Docker"

- sudo apt install docker

# Segundo paso: "Levantar Prometheus en Docker"

- docker pull prom/prometheus

Compruebo que la imagen se ha descargado correctamente:

- docker image ps

Ahora creamos el archivo 'prometheus.yml'

```bash

global:
	scrape_interval: 15s
	evaluation_interval: 15s
rule_files:
scrape_configs:
	- job_name: 'prometheus'
	static_configs:
		- targets: ['127.0.0.1:10087']

```

Colocaremos el fichero en la siguiente ruta:

"/Users/mauri/Desktop/prometheus/prometheus.yml"

# A continuación, levantamos el contenedor apuntando al fichero, que se mapeará al directorio "/etc/prometheus" del contenedor.

```bash

docker run -d --name prometheus -p 9090:9090 -p 10087:10087 -v /Users/mauri/Desktop/prometheus/prometheus.yml:/etc/prometheus/prometheus.yml prom/prometheus --config.file=/etc/prometheus/prometheus.yml

```

# Tercer paso: "Levantar Grafana en Docker"

docker run -d --name grafana -p 3000:3000 grafana/grafana

# Cuarto paso: "Colocar ambos contenedores en la misma red"

```bash

docker network create myNetwork
docker network connect myNetwork prometheus
docker network connect myNetwork grafana

```

# Quinto paso: "Añadir el Datasource y crear Dashboard"

En el apartado "Datasources", añadimos Prometheus y seleccionamos como origen "http://localhost:9090"

Muy importante también que la opción "Access" esté marcada a 'Browser' para no recibir un error "Bad Gateway".
