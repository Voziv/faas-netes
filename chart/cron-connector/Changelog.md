# Change Log

## 0.5.0 

**Release date:** 2021-07-14

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update cron-connector version 
* Update Cron Connector docs to refer to OpenFaaS repository 

### Default value changes

```diff
diff --git a/chart/cron-connector/values.yaml b/chart/cron-connector/values.yaml
index 944ee130..1f1c6412 100644
--- a/chart/cron-connector/values.yaml
+++ b/chart/cron-connector/values.yaml
@@ -1,10 +1,17 @@
-image: ghcr.io/openfaas/cron-connector:0.3.2
+image: ghcr.io/openfaas/cron-connector:0.5.0
 
-gateway_url: http://gateway.openfaas:8080
-basic_auth: true
+gatewayURL: http://gateway.openfaas:8080
+
+# Invoke via the asynchronous function endpoint
+asyncInvocation: false
+
+# Set a contentType for all invocations
+contentType: text/plain
 
 nodeSelector: {}
 
 tolerations: []
 
 affinity: {}
+
+basicAuth: true
\ No newline at end of file
```

## 0.3.2 

**Release date:** 2020-11-17

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump cron-connector version 

### Default value changes

```diff
diff --git a/chart/cron-connector/values.yaml b/chart/cron-connector/values.yaml
index 9e17b515..944ee130 100644
--- a/chart/cron-connector/values.yaml
+++ b/chart/cron-connector/values.yaml
@@ -1,4 +1,5 @@
-image: openfaas/cron-connector:0.3.1
+image: ghcr.io/openfaas/cron-connector:0.3.2
+
 gateway_url: http://gateway.openfaas:8080
 basic_auth: true
 
```

## 0.3.1 

**Release date:** 2019-12-06

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Publish cron-connector chart 

### Default value changes

```diff
# No changes in this release
```

## 0.3.0 

**Release date:** 2019-12-04

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump cron-chart to use forked version 

### Default value changes

```diff
diff --git a/chart/cron-connector/values.yaml b/chart/cron-connector/values.yaml
index cb3f7222..9e17b515 100644
--- a/chart/cron-connector/values.yaml
+++ b/chart/cron-connector/values.yaml
@@ -1,4 +1,4 @@
-image: zeerorg/cron-connector:v0.2.2
+image: openfaas/cron-connector:0.3.1
 gateway_url: http://gateway.openfaas:8080
 basic_auth: true
 
@@ -6,4 +6,4 @@ nodeSelector: {}
 
 tolerations: []
 
-affinity: {}
\ No newline at end of file
+affinity: {}
```

## 0.2.0 

**Release date:** 2019-12-04

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Regenerate connector charts 
* Update cron-connector chart to apps/v1 
* Substitute labels keys with name and component and remove app.kubernetes.io 
* Removed app.kubernetes.io labels 
* Updated Documentation and Notes 

### Default value changes

```diff
# No changes in this release
```

## 0.1.0 

**Release date:** 2019-06-12

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Fix Alex's comments 

### Default value changes

```diff
# No changes in this release
```

## 0.2.1 

**Release date:** 2019-06-10

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Added cron-connector chart 

### Default value changes

```diff
image: zeerorg/cron-connector:v0.2.2
gateway_url: http://gateway.openfaas:8080
basic_auth: true

nodeSelector: {}

tolerations: []

affinity: {}```

---
Autogenerated from Helm Chart and git history using [helm-changelog](https://github.com/mogensen/helm-changelog)