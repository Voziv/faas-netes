# Change Log

## Next Release 

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* kafka-connector quickstart document updated according to latest updates on arkade and kafka 

### Default value changes

```diff
# No changes in this release
```

## 0.6.2 

**Release date:** 2021-07-14

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update image name for Kafka connector 
* Fix link for quickstart 

### Default value changes

```diff
diff --git a/chart/kafka-connector/values.yaml b/chart/kafka-connector/values.yaml
index e789d694..6c274442 100644
--- a/chart/kafka-connector/values.yaml
+++ b/chart/kafka-connector/values.yaml
@@ -1,7 +1,7 @@
 # You will need to create a license named "openfaas-license" - see the 
 # chart README for detailed instructions.
 
-image: ghcr.io/openfaas/kafka-pro-connector:0.6.0-rc2
+image: ghcr.io/openfaas/kafka-connector-pro:0.6.0-rc2
 
 replicas: 1
 
```

## 0.6.1 

**Release date:** 2021-07-06

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Add support for Content Type in Kafka connector 
* Rename quickstart 

### Default value changes

```diff
diff --git a/chart/kafka-connector/values.yaml b/chart/kafka-connector/values.yaml
index 8882b57c..e789d694 100644
--- a/chart/kafka-connector/values.yaml
+++ b/chart/kafka-connector/values.yaml
@@ -1,7 +1,7 @@
 # You will need to create a license named "openfaas-license" - see the 
 # chart README for detailed instructions.
 
-image: ghcr.io/openfaas/kafka-pro-connector:0.6.0-rc1
+image: ghcr.io/openfaas/kafka-pro-connector:0.6.0-rc2
 
 replicas: 1
 
@@ -15,7 +15,10 @@ asyncInvocation: false
 topics: faas-request
 
 # Your Kafka broker
-# brokerHost: kf-kafka:9092
+brokerHost: kf-kafka:9092
+
+# HTTP content-type for invoking functions
+contentType: text/plain
 
 printResponse: true
 printResponseBody: false
```

## 0.6.0 

**Release date:** 2021-07-06

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Add support for Kafka authentication and TLS 
* Update development instructions for Kafka 
* Update Kafka testing instructions 
* fix: Update prometheus scrape lables to use a slash 

### Default value changes

```diff
diff --git a/chart/kafka-connector/values.yaml b/chart/kafka-connector/values.yaml
index b3b2a342..8882b57c 100644
--- a/chart/kafka-connector/values.yaml
+++ b/chart/kafka-connector/values.yaml
@@ -1,7 +1,7 @@
 # You will need to create a license named "openfaas-license" - see the 
-# chart README for more.
+# chart README for detailed instructions.
 
-image: ghcr.io/openfaas/kafka-connector-pro:0.5.0
+image: ghcr.io/openfaas/kafka-pro-connector:0.6.0-rc1
 
 replicas: 1
 
@@ -15,7 +15,7 @@ asyncInvocation: false
 topics: faas-request
 
 # Your Kafka broker
-brokerHost: kafka:9092
+# brokerHost: kf-kafka:9092
 
 printResponse: true
 printResponseBody: false
@@ -31,3 +31,56 @@ nodeSelector: {}
 tolerations: []
 
 affinity: {}
+
+# Encryption
+tls: false
+
+# Authentication
+
+## When set to true, create secrets: "kafka-broker-username" and "kafka-broker-password"
+saslAuth: false
+
+# kubectl create secret generic \
+# kafka-broker-username \
+# -n openfaas \
+# --from-file broker-username=./broker-username.txt
+
+# kubectl create secret generic \
+# kafka-broker-password \
+# -n openfaas \
+# --from-file broker-password=./broker-password.txt
+
+brokerPasswordSecret: kafka-broker-password
+brokerUsernameSecret: kafka-broker-username
+
+## When using a custom CA:
+
+# kubectl create secret generic \
+# kafka-broker-ca \
+# -n openfaas \
+# --from-file broker-ca=./broker-ca.pem
+
+# When using client certs
+
+# kubectl create secret generic \
+# kafka-broker-cert \
+# -n openfaas \
+# --from-file broker-cert=./broker-cert.txt
+
+# kubectl create secret generic \
+# kafka-broker-key \
+# -n openfaas \
+# --from-file broker-key=./broker-key.txt
+
+# Set to empty to disable:
+
+caSecret: ""
+certSecret: ""
+keySecret: ""
+
+# Or give a name to each to enable
+
+# caSecret: kafka-broker-ca
+# certSecret: kafka-broker-cert
+# keySecret: kafka-broker-key
+
```

## 0.5.1 

**Release date:** 2021-01-06

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update chart location 

### Default value changes

```diff
# No changes in this release
```

## 0.5.0 

**Release date:** 2021-01-06

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Upgrade to PRO edition of Kafka connector 
* Update Kafka chart for new PRO edition 

### Default value changes

```diff
diff --git a/chart/kafka-connector/values.yaml b/chart/kafka-connector/values.yaml
index d5b591d2..b3b2a342 100644
--- a/chart/kafka-connector/values.yaml
+++ b/chart/kafka-connector/values.yaml
@@ -1,4 +1,8 @@
-image: ghcr.io/openfaas/kafka-connector:0.4.1
+# You will need to create a license named "openfaas-license" - see the 
+# chart README for more.
+
+image: ghcr.io/openfaas/kafka-connector-pro:0.5.0
+
 replicas: 1
 
 # Max timeout for a function
```

## 0.4.3 

**Release date:** 2020-12-30

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Fix: #732 

### Default value changes

```diff
diff --git a/chart/kafka-connector/values.yaml b/chart/kafka-connector/values.yaml
index 4a030632..d5b591d2 100644
--- a/chart/kafka-connector/values.yaml
+++ b/chart/kafka-connector/values.yaml
@@ -20,7 +20,7 @@ printResponseBody: false
 gatewayURL: http://gateway.openfaas:8080
 
 # Basic auth for the gateway
-basicAuth: true
+basic_auth: true
 
 nodeSelector: {}
 
```

## 0.4.2 

**Release date:** 2020-11-17

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update Kafka connector to GHCR 

### Default value changes

```diff
diff --git a/chart/kafka-connector/values.yaml b/chart/kafka-connector/values.yaml
index c761d71b..4a030632 100644
--- a/chart/kafka-connector/values.yaml
+++ b/chart/kafka-connector/values.yaml
@@ -1,4 +1,4 @@
-image: openfaas/kafka-connector:0.4.0
+image: ghcr.io/openfaas/kafka-connector:0.4.1
 replicas: 1
 
 # Max timeout for a function
```

## 0.4.1 

**Release date:** 2020-11-17

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Add async options to Kafka connector chart 
* Update README.md 

### Default value changes

```diff
diff --git a/chart/kafka-connector/values.yaml b/chart/kafka-connector/values.yaml
index 9138a23b..c761d71b 100644
--- a/chart/kafka-connector/values.yaml
+++ b/chart/kafka-connector/values.yaml
@@ -1,11 +1,26 @@
 image: openfaas/kafka-connector:0.4.0
 replicas: 1
-gateway_url: http://gateway.openfaas:8080
+
+# Max timeout for a function
+upstreamTimeout: 2m
+
+# Use with slow consumers or long running functions
+asyncInvocation: false
+
+# Alter to the topics required
 topics: faas-request
-print_response: true
-print_response_body: false
-broker_host: kafka:9092
-basic_auth: true
+
+# Your Kafka broker
+brokerHost: kafka:9092
+
+printResponse: true
+printResponseBody: false
+
+# Internal gateway URL
+gatewayURL: http://gateway.openfaas:8080
+
+# Basic auth for the gateway
+basicAuth: true
 
 nodeSelector: {}
 
```

## 0.4.0 

**Release date:** 2019-12-04

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Regenerate connector charts 
* Update Kafka connector chart to be k8s 1.16 compatible 
* Turn off prometheus scrape 

### Default value changes

```diff
# No changes in this release
```

## 0.3.0 

**Release date:** 2019-08-31

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump Kafka-connector chart 
* Update port configuration change 

### Default value changes

```diff
diff --git a/chart/kafka-connector/values.yaml b/chart/kafka-connector/values.yaml
index 0279e9c4..9138a23b 100644
--- a/chart/kafka-connector/values.yaml
+++ b/chart/kafka-connector/values.yaml
@@ -1,10 +1,10 @@
-image: openfaas/kafka-connector:0.3.4
+image: openfaas/kafka-connector:0.4.0
 replicas: 1
 gateway_url: http://gateway.openfaas:8080
 topics: faas-request
 print_response: true
 print_response_body: false
-broker_host: kafka
+broker_host: kafka:9092
 basic_auth: true
 
 nodeSelector: {}
```

## 0.2.3 

**Release date:** 2019-07-24

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump Kafka chart for verbose logging 
* Updated yaml to deploy test client 
* Add print_response_body to kafka readme 

### Default value changes

```diff
diff --git a/chart/kafka-connector/values.yaml b/chart/kafka-connector/values.yaml
index befb439c..0279e9c4 100644
--- a/chart/kafka-connector/values.yaml
+++ b/chart/kafka-connector/values.yaml
@@ -1,4 +1,4 @@
-image: openfaas/kafka-connector:0.3.3
+image: openfaas/kafka-connector:0.3.4
 replicas: 1
 gateway_url: http://gateway.openfaas:8080
 topics: faas-request
```

## 0.2.2 

**Release date:** 2019-04-12

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update Kafka chart for #408 
* Add print_response_body 

### Default value changes

```diff
diff --git a/chart/kafka-connector/values.yaml b/chart/kafka-connector/values.yaml
index 26712b4c..befb439c 100644
--- a/chart/kafka-connector/values.yaml
+++ b/chart/kafka-connector/values.yaml
@@ -3,6 +3,7 @@ replicas: 1
 gateway_url: http://gateway.openfaas:8080
 topics: faas-request
 print_response: true
+print_response_body: false
 broker_host: kafka
 basic_auth: true
 
```

## 0.1.2 

**Release date:** 2019-04-06

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump Kafka chart to 0.3.3 
* Change to fullnameOverride 
* Use deploymentNameOverride instead of fullnameOverride 
* Parameterize connector deployment name 

### Default value changes

```diff
diff --git a/chart/kafka-connector/values.yaml b/chart/kafka-connector/values.yaml
index 1e27e437..26712b4c 100644
--- a/chart/kafka-connector/values.yaml
+++ b/chart/kafka-connector/values.yaml
@@ -1,4 +1,4 @@
-image: functions/kafka-connector:0.3.0
+image: openfaas/kafka-connector:0.3.3
 replicas: 1
 gateway_url: http://gateway.openfaas:8080
 topics: faas-request
```

## 0.1.1 

**Release date:** 2019-03-30

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update logs command to use a deployment 
* Update development instructions for Kafka 

### Default value changes

```diff
# No changes in this release
```

## 0.1.0 

**Release date:** 2019-03-30

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Implement a simple kafka connector helm chart 

### Default value changes

```diff
image: functions/kafka-connector:0.3.0
replicas: 1
gateway_url: http://gateway.openfaas:8080
topics: faas-request
print_response: true
broker_host: kafka
basic_auth: true

nodeSelector: {}

tolerations: []

affinity: {}
```

---
Autogenerated from Helm Chart and git history using [helm-changelog](https://github.com/mogensen/helm-changelog)
