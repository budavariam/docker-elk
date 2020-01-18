# Filebeat with docker-compose

Load logs from `./logs` relative to repository root into logstash.

## Usage

If you want to include the Filebeat extension, run Docker Compose from the root of the repository with an additional
command line argument referencing the `filebeat-compose.yml` file:

```bash
docker-compose -f docker-compose.yml -f extensions/filebeat-compose/filebeat-compose.yml up
```
