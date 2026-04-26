# Strimzi Kafka Helm Chart

This directory contains a Helm chart for deploying Strimzi Kafka and related resources.

## Usage

1. Add the Strimzi Helm repository:
   helm repo add strimzi https://strimzi.io/charts/
   helm repo update

2. Install the Strimzi Kafka Operator:
   helm install strimzi-kafka-operator strimzi/strimzi-kafka-operator -n kafka --create-namespace

3. Deploy your Kafka cluster and topics using custom resources in this chart or via `kubectl apply`.
