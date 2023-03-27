# node-influx-uptimerobot
A tool to get statistics from [Uptime Robot](https://uptimerobot.com/) and log it into [InfluxDB](https://www.influxdata.com/time-series-platform/influxdb/)

## Prerequisites
- NodeJS 5+
- [Uptime Robot](https://uptimerobot.com/) account
- [InfluxDB](https://www.influxdata.com/time-series-platform/influxdb/) instance

## Installation (NodeJS)
```bash
git clone https://github.com/trojanc/node-influx-uptimerobot.git
cd node-influx-uptimerobot
npm install
node index.js
```
Remember to create a configuration file, or have environment variables in place for your configuration.

## Application configuration
Place config in `config.json` or pass a parameter with the location of the config
file to use.

```json
{
  "application" : {
    "interval" : 10
  },
  "uptimerobot" : {
    "api_key" : "",
    "logs_limit" : 100,
    "response_times_limit" : 100
  },
  "influx" : {
    "org" : "",
    "url" : "",
    "token" : "",
    "bucket" : ""
  }
}
```
- **application.interval** Interval (seconds) at which to pull data. If not specified it will only do a pull once.
- **uptimerobot.api_key** Your Uptime Robot API key.
- **uptimerobot.logs_limit** Limit the number of logs to pull at a time.
- **uptimerobot.response_times_limit** Limit the number of responses to pull at time.
- **influx.org** Organization for your InfluxDB server.
- **influx.url** Url for your InfluxDB server.
- **influx.token** Token for your InfluxDB server.
- **influx.bucket** Name of the InfluxDB bucket to use.

Each of the above configuration options can also be set using environment variables.
Environment variables override any configuration set in a config file.
- **APPLICATION_INTERVAL** Interval (seconds) at which to pull data. If not specified it will only do a pull once.
- **UPTIMEROBOT_API_KEY** Your Uptime Robot API key.
- **UPTIMEROBOT_LOGS_LIMIT** Limit the number of logs to pull at a time.
- **UPTIMEROBOT_RESPONSE_TIMES_LIMIT** Your Uptime Robot API key.
- **INFLUX_ORG** Organization for your InfluxDB server.
- **INFLUX_URL** Url for your InfluxDB server.
- **INFLUX_TOKEN** Token for your InfluxDB server.
- **INFLUX_BUCKET** Name of the InfluxDB bucket to use.

# Uptime Robot configuration
To use this tool you need to get a API key from your Uptime Robot account.

This can be retrieve from [My Settings](https://uptimerobot.com/dashboard#mySettings) by either creating a Main API Key, or a Monitor Specific API Key.

If you create a Main API Key you will be able to log all monitors, if you use a Monitor Specific API Key you will only be able to log that single monitor.

### Logs
The `logs` table contains any messages that are logged by Uptime Robot for a monitor.
- **monitorName** Friendly name for the monitor.
- **reason** The reason of the downtime (if exists).
- **reason_detail** Additional information about the reason (if exists).
- **type** Type of log (1 - down, 2 - up, 99 - paused, 98 - started)
- **logDatetime** Datetime of the log.

### Response Time
The `response_time` table will contain entries of the response times for each monitor.
- **monitorName** Name for the monitor.
- **value** The response time of the monitor.

## Dashboard
![Dashboard](./docs/dashboard.png)

If you want to use grafana to visualize the reading you can import `grafana-uptimerobot-dashboard.json` to grafana and update
your data sources as required to have them linked up.
