

На виртуальной машине установите любую open source CMS, которая включает в себя следующие компоненты: nginx, php-fpm, database (MySQL or Postgresql)
На этой же виртуальной машине установите Prometheus exporters для сбора метрик со всех компонентов системы (начиная с VM и заканчивая DB, не забудьте про blackbox exporter, который будет проверять доступность вашей CMS)
На этой же или дополнительной виртуальной машине установите Prometheus, задачей которого будет раз в 5 секунд собирать метрики с экспортеров.

# Dockerized WordPress with Prometheus Monitoring

This repository contains a Docker Compose setup for running a WordPress website with Nginx and MySQL, and monitoring the setup using Prometheus and various exporters.

## Project Structure

- `docker-compose.yml`: Docker Compose configuration file defining services like WordPress, MySQL, Nginx, Prometheus, and various exporters.
- `GAP-1/`: Contains configuration files for Prometheus and Alertmanager.
- `prometheus.yml`: Configuration for the Prometheus service.
- `alertmanager.yml`: Configuration for the Alertmanager service.
- `nginx/`: Contains Nginx configuration files.
- `README.md`: Documentation of the project setup and usage.

## Setup and Usage

### Clone the Repository:

### Start the Docker Services:
Navigate to the project directory and start the services using Docker Compose:

### Access the Services:
- WordPress: `http://localhost`
- phpMyAdmin: `http://localhost:8081`
- Prometheus: `http://localhost:9090`
- Blackbox: `http://localhost:9115`
- Alertmanager: `http://localhost:9093`
- Mysql: `http://localhost:9104`
- Node: `http://localhost:9100`
- Apache: `http://localhost:9117`

## Monitoring with Prometheus

Prometheus is configured to scrape metrics from Node Exporter, MySQL Exporter, and Apache Exporter.
Access the Prometheus UI at `http://localhost:9090` to view metrics and configure alerts.

## Alertmanager Configuration

Alertmanager handles alerts sent by Prometheus and takes care of deduplicating, grouping, and routing them to the correct receiver. Basic configuration can be found in `alertmanager.yml`. Modify this file to set up notification methods like email, Slack, etc.


