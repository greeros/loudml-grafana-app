# LoudML Grafana Application

Visualization panel and datasource for Grafana 6.7.x to connect with Loud ML AI solution for ICT and IoT
automation. https://loudml.io

![LoudML Panel in Grafana](https://raw.githubusercontent.com/vsergeyev/loudml-grafana-app/master/docs/loudml_grafana_panel.png)

![LoudML Datasource in Grafana](https://raw.githubusercontent.com/vsergeyev/loudml-grafana-app/master/docs/loudml_grafana_datasource.png)

Loud ML is an open source inference engine for metrics and events, and the fastest way to embed machine learning in your time series application. This includes APIs for storing and querying data, processing it in the background for ML or detecting outliers for alerting purposes, and more.
https://github.com/regel/loudml

# Installation

 * Plugin should be placed in `.../grafana/data/plugins`
 * git clone https://github.com/vsergeyev/loudml-grafana-app.git
 * cd loudml-grafana-app
 * yarn
 * yarn dev --watch
 * restart Grafana
 * LoudML app should be in plugins list, you may need to activate it
 * enjoy :)

# What inside

Loud ML Panel - is a version of Grafana's default Graph Panel with a "Create Baseline" button
to create ML model in 1-click.

Currently 1-click ML button ("Create Baseline") can produce model from:

 * InfluxDB datasource
 * OpenTSDB datasource
 * Elasticsearch datasource (beta)
 * Prometheus datasource (very draft)

Loud ML Datasource - is a connector to Loud ML server. It has capabilities to show models and jobs on server. You can add new and edit existing models.

# Prerequisites

    * Loud ML server https://github.com/regel/loudml
    * Grafana >= 5.4.0

# Configuration

In order to use Loud ML with Grafana you need to have a buckets in **loudml.yml** to reflect Grafana datasource(s) used in LoudML Graph

![LoudML Panel Configuration in Grafana](https://raw.githubusercontent.com/vsergeyev/loudml-grafana-app/master/docs/loudml_props.png)

Example: I have InfluxDB datasource with **telegraf** database as an input and will use **loudml** database as output for ML model predictions/forecasting/anomalies:

    buckets:
     - name: loudml
       type: influxdb
       addr: 127.0.0.1:8086
       database: loudml
       retention_policy: autogen
       measurement: loudml
       annotation_db: loudmlannotations
     - name: influxdb1
       type: influxdb
       addr: 127.0.0.1:8086
       database: telegraf
       retention_policy: autogen
       measurement: loudml
     - name: data
       type: influxdb
       addr: 127.0.0.1:8086
       database: data
       retention_policy: autogen
       measurement: sinus
     - name: opentsdb1
       type: opentsdb
       addr: 127.0.0.1:4242
       retention_policy: autogen
     - name: prom1
       type: prometheus
       addr: 127.0.0.1:9090
       retention_policy: autogen

InfluxDB **loudmlannotations** here specified to store annotations. (By default Loud ML server will store annotations in **chronograf** database). So on Grafana dashboard annotations/anomalies from Loud ML should be configured as:

    SELECT "text" FROM "autogen"."annotations" WHERE $timeFilter

![LoudML Annotations in Grafana](https://raw.githubusercontent.com/vsergeyev/loudml-grafana-app/master/docs/loudml_annotations.png)

# Support

Please post issue to tracker or contact me via vova.sergeyev at gmail.com.
