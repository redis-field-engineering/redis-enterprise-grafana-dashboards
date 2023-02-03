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
that has been done you must create a Prometheus data source in Grafana's administration console. You should name the data source 'Redis-Enterprise'; 
if you decide to name something else you will need to change the data source names in the individual dashboard JSON files. Please follow the 
instructions on the following page

```
https://docs.redis.com/latest/rs/clusters/monitoring/prometheus-integration/
```

Once this has been done you can use the Grafana administration console to import the files in dashboards/

## Running the tests

In order to run the alerting tests you will need to copy the rules/ and tests/ folders to your Prometheus installation. Once they have been copied 
you can execute the tests as follows:

```
promtool test rules tests/*
```

### Modifying the alerts

Alerts can, and probably should, be modified to correpsond to your environment and its configuration. Additional alerts can be created 
following Prometheus' alerting guidelines. It is strongly recommend to create unit tests for each of your alerts to ensure they perform as expected.

Further details can be found [here](https://prometheus.io/docs/prometheus/latest/configuration/unit_testing_rules/)

## Deployment

Open Grafana's dashboard tab, click on the blue 'New' button on the far right and select 'Import', then click on the 'Upload JSON file' button and 
navigate to the dashboard files included with the project in the 'dashboards' folder.

## Authors

* *B*John Burke** - *Initial work* - [Redis](https://github.com/redis-field-engineering)

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

