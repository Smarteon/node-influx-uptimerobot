# node-influx-uptimerobot
A tool to get statistics from [Uptime Robot](https://uptimerobot.com/) and log it into [InfluxDB](https://www.influxdata.com/time-series-platform/influxdb/)

## Prerequisites
- NodeJS 5+
- [Uptime Robot](https://uptimerobot.com/) account
- [InfluxDB](https://www.influxdata.com/time-series-platform/influxdb/) instance

## Installation
```bash
git clone https://github.com/trojanc/node-influx-uptimerobot.git
cd node-influx-uptimerobot
npm install
node index.js
```

## Installation (Docker)
```bash
git clone https://github.com/trojanc/node-influx-uptimerobot.git
cd node-influx-uptimerobot
docker-compose up
```

You probably would want to run both of the installation options above on some sort of schedule like a cron entry.

## Application configuration
Place config in `config.json` or pass a parameter with the location of the config
file to use.

```json
{
  "uptimerobot" : {
    "apikey" : "uxxxxxx-xxxxxxxxxxxxxxxxxxxxxxxx"
  },
  "influx" : {
    "host" : "localhost",
    "port" : 8086,
    "protocol" : "http",
    "username" : "admin",
    "password" : "admin",
    "database" : "uptimerobot"
  }
}
```
- **uptimerobot.apikey** Your uptimerobot API key.
- **influx.host** Hostname or IP of your influxdb server.
- **influx.port** Hostname or IP of your influxdb server.
- **influx.protocol** protocol for your influxdb server.
- **influx.username** Username for your influxdb server.
- **influx.password** Password for your influxdb server.
- **influx.database** Name of the influxdb database to use

# Uptime Robot configuration
To use this tool you need to get a API key from your Uptime Robot account.

This can be retrieve from [My Settings](https://uptimerobot.com/dashboard#mySettings) by either creating a Main API Key, or a Monitor Specific API Key. 

If you create a Main API Key you will be able to log all monitors, if you use a Monitor Specific API Key you will only be able to log that single monitor.

## Influx DB configuration
You can create a new influxdb database using
```
CREATE DATABASE uptimerobot;
```

```
USE uptimerobot;
```

When this script is ran it will create the tables if it does not exist. The tables it will populate are `logs` and `responseTime`. This can be seen by running
```
SHOW MEASUREMENTS;
```
```
name: measurements
------------------
logs
responseTime

```

### Logs
The `logs` table contains any messages that are logged by uptimerobot for a monitor.
- **friendlyname** Friendly name for the monitor.
- **id** ID of the monitor.
- **reason** The reason of the downtime (if exists).
- **type** Type of log (1 - down, 2 - up, 99 - paused, 98 - started)

### Response Time
The `responseTime` table will contain entries of the response times for each monitor.
- **friendlyname** Friendly name for the monitor.
- **id** ID of the monitor.
- **value** The response time of the monitor.

## Dashboard
![Dashboard](./docs/dashboard.png)

If you want to use grafana to visualize the reading you can import `grafana-uptimerobot-dashboard.json` to grafana and update
your data sources as required to have them linked up.
