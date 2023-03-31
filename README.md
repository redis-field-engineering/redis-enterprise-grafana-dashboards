# Redis Cloud Dashboards

These dashboards are intended to graphically present standard metrics of every level of a Redis Enterprise installation. Alert configuration files 
will assist you in providing notifications should any of a number of key values exceed their expected ranges. Lastly, metrics description files 
provide information about additional values that can be monitored.

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes. See deployment for 
notes on how to deploy the project on a live system.

### Prerequisites

You will need to install the following software packages. Depending on your distribution there may be different ways of installing; choose the 
packaging style with which you are most familiar

```
Prometheus
Grafana
```

### Installing

Once both Prometheus and Grafana have been installed you will need to modify Prometheus' config file and point it at Redis' metrics endpoint. Once 
that has been done you must create a Prometheus data source in Grafana's administration console. Please follow the 
instructions on the following page:

```
https://docs.redis.com/latest/rs/clusters/monitoring/prometheus-integration/
```

### Third-party plugin

Certain information displays require the installation of a third-party plugin, Infinity. Find it here:

```
https://grafana.com/grafana/plugins/yesoreyeram-infinity-datasource/
```


### Running the tests

In order to run the alerting tests you will need to copy the rules/ and tests/ folders to your Prometheus installation. Once they have been copied 
you can execute the tests as follows:

```
promtool test rules tests/*
```

### Modifying the alerts

Alerts can, and probably should, be modified to correpsond to your environment and its configuration. Additional alerts can be created 
following Prometheus' alerting guidelines. It is strongly recommend to create unit tests for each of your alerts to ensure they perform as expected.

Further details can be found [here](https://prometheus.io/docs/prometheus/latest/configuration/unit_testing_rules/)


## Importing and Configuring the Dashboards (Cloud)

Once all the components have been installed you can use the Grafana administration console to import the dashboard files. 

Open Grafana's dashboard tab, click on the blue 'New' button on the far right and select 'Import', then click on the 'Upload JSON file' button and 
navigate to the dashboard files included with the project in the 'dashboards' folder.

The Database Status Dashboard needs to have a variable configured. 

```
1. Open the Database Status dashboard settings and select 'Variables' from the left-hand menu
2. Add a variable 'subscription'
3. Set its data source to 'Infinity'
4. Type=JSON, Parser=Backend, Source=URL, Method=GET, Format=Table, URL=https://api.redislabs.com/v1/subscriptions
5. CLick on 'Headers, Request parameters' and add the following headers;
   - accept = application/json
   - x-secret-key = <your secret key>
   - x-api-secret-key = <your api key>
   
   add the following request parameters:
   - offset = 0
   - limit = 100
   
6. Open 'Parsing options and Result fields' and set the Rows/Root = subscriptions
   - then set the column selector to id, as subscriptionId, and format as Number
   
7. Click 'Run query' at the very bottom and your subscription id should be returned.
```

The Database Status Dashboard has two panels that need to 
be configured; Modules, and Configuration. They should use the Infinity datasource with the same settings as above with the following exceptions.

```
1. For Modules & Configuration: URL = https://api.redislabs.com/v1/subscriptions/$subscription/databases/$bdb
2. For Modules, Open 'Parsing options and Result fields' and set the Rows/Root=modules
3. Also for Modules, 'Parsing options and Result fields' need the following fields
   - name, as Name, format as String
   - version, as Version, format as String
3. For Configuration, 'Parsing options and Result fields' need the following fields
   - replication, as 1. Replication, format as String
   - dataPersistence, as 2. Persistence, format as String
   - backup.enableRemoteBackup, as 3. Backup, format as String
   - dataEvictionPolicy, as 4. Eviction, format as String
   - protocol, as 5. Protocol, format as String
```


## Importing and Configuring the Dashboards (Enterprise)

Once all the components have been installed you can use the Grafana administration console to import the dashboard files. 

Open Grafana's dashboard tab, click on the blue 'New' button on the far right and select 'Import', then click on the 'Upload JSON file' button and 
navigate to the dashboard files included with the project in the 'dashboards' folder.

The Database Status Dashboard has two panels that need to 
be configured; Modules, and Configuration. They should use the Infinity datasource with the following settings.

```
1. Type=JSON, Parser=Backend, Source=URL, Format=Table, Method=GET, URL=https://<ip address>:9443/v1/bdbs
2. For Modules, Open 'Parsing options and Result fields' and set the Rows/Root=module_list
3. Also for Modules, 'Parsing options and Result fields' need the following fields
   - module_name, as Name, format as String
   - semantic_version, as Version, format as String
3. For Configuration, 'Parsing options and Result fields' need the following fields
   - replication, as 1. Replication, format as String
   - data_persistence, as 2. Persistence, format as String
   - backup, as 3. Backup, format as String
   - eviction_policy, as 4. Eviction, format as String
   - rack_aware, as 5. Multi AZ, format as String
```

## Authors

John Burke - *Initial work* - [Redis](https://github.com/redis-field-engineering)

See also the list of [contributors](https://github.com/redis-field-engineering/redis-cloud-dashboards/graphs/contributors) who participated in this 
project.

## Support
Redis Cloud Dashboards is supported by Redis, Inc. on a good faith effort basis. To report bugs, request features, or receive assistance, please 
file an [issue](https://github.com/redis-field-engineering/redis-cloud-dashboards/issues).

## License
Redis Cloud Dashboards is licensed under the MIT License. Copyright © 2023 Redis, Inc.

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

## Acknowledgments

* Hat tip to anyone whose code was used
* Inspiration
* etc

