# Change Log

## 7.4.3 

**Release date:** 2021-07-22

![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Fix syntax error introduced in queue-worker 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 13ad1e2f..df1f53ee 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -140,10 +140,10 @@ queueWorker:
       memory: "120Mi"
       cpu: "50m"
 # Requires openfaas PRO subscription
-  maxRetryAttempts: 10
-  maxRetryWait: 120s
-  initialRetryWait: 10s
-  retry_http_codes: 429,502,500,504,408
+  maxRetryAttempts: "10"
+  maxRetryWait: "120s"
+  initialRetryWait: "10s"
+  httpRetryCodes: "429,502,500,504,408"
 
 # monitoring and auto-scaling components
 # both components
```

## 7.4.2 

**Release date:** 2021-07-22

![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Add list of retry codes for queue worker 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index d74ffb21..13ad1e2f 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -143,6 +143,7 @@ queueWorker:
   maxRetryAttempts: 10
   maxRetryWait: 120s
   initialRetryWait: 10s
+  retry_http_codes: 429,502,500,504,408
 
 # monitoring and auto-scaling components
 # both components
```

## 7.4.1 

**Release date:** 2021-07-22

![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update chart to support queue-worker envs 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index cae078cf..d74ffb21 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -66,8 +66,6 @@ faasIdler:
     requests:
       memory: "64Mi"
 
-## OSS components
-
 gateway:
   image: ghcr.io/openfaas/gateway:0.20.12
   readTimeout: "65s"
@@ -141,6 +139,10 @@ queueWorker:
     requests:
       memory: "120Mi"
       cpu: "50m"
+# Requires openfaas PRO subscription
+  maxRetryAttempts: 10
+  maxRetryWait: 120s
+  initialRetryWait: 10s
 
 # monitoring and auto-scaling components
 # both components
```

## 7.4.0 

**Release date:** 2021-07-17

![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update Ingress for K8s 1.22 
* Update Helm chart to reflect different Ingress api versions 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 62c32723..cae078cf 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -17,6 +17,8 @@ clusterRole: false            # Set to true to have OpenFaaS administrate multip
 
 createCRDs: true
 
+k8sVersionOverride: "" #  Allow kubeVersion to be overridden for the ingress creation
+
 # create pod security policies for OpenFaaS control plane
 # https://kubernetes.io/docs/concepts/policy/pod-security-policy/
 psp: false
@@ -68,9 +70,9 @@ faasIdler:
 
 gateway:
   image: ghcr.io/openfaas/gateway:0.20.12
-  readTimeout : "65s"
-  writeTimeout : "65s"
-  upstreamTimeout : "60s"  # Must be smaller than read/write_timeout
+  readTimeout: "65s"
+  writeTimeout: "65s"
+  upstreamTimeout: "60s"  # Must be smaller than read/write_timeout
   replicas: 1
   scaleFromZero: true
   # change the port when creating multiple releases in the same baremetal cluster
@@ -96,9 +98,9 @@ basicAuthPlugin:
 
 faasnetes:
   image: ghcr.io/openfaas/faas-netes:0.13.4
-  readTimeout : "60s"
-  writeTimeout : "60s"
-  imagePullPolicy : "Always"    # Image pull policy for deployed functions
+  readTimeout: "60s"
+  writeTimeout: "60s"
+  imagePullPolicy: "Always"    # Image pull policy for deployed functions
   httpProbe: true               # Setting to true will use HTTP for readiness and liveness probe on Pods (incompatible with Istio < 1.1.5)
   setNonRootUser: false
   readinessProbe:
@@ -134,7 +136,7 @@ queueWorker:
   maxInflight: 1
   gatewayInvoke: true
   queueGroup: "faas"
-  ackWait : "60s"
+  ackWait: "60s"
   resources:
     requests:
       memory: "120Mi"
@@ -180,6 +182,10 @@ nats:
 # ingress configuration
 ingress:
   enabled: false
+  ## For k8s >= 1.18 you need to specify the pathType
+  ## See https://kubernetes.io/blog/2020/04/02/improvements-to-the-ingress-api-in-kubernetes-1.18/#better-path-matching-with-path-types
+  #pathType: ImplementationSpecific
+
   # Used to create Ingress record (should be used with exposeServices: false).
   hosts:
     - host: gateway.openfaas.local  # Replace with gateway.example.com if public-facing
@@ -189,7 +195,7 @@ ingress:
   annotations:
     kubernetes.io/ingress.class: nginx
   tls:
-    # Secrets must be manually created in the namespace.
+  # Secrets must be manually created in the namespace.
 
 # ingressOperator (optional) – component to have specific FQDN and TLS for Functions
 # https://github.com/openfaas-incubator/ingress-operator
```

## 7.3.2 

**Release date:** 2021-06-17

![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Chart update: allow secret creation in binary format 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 83e153d8..62c32723 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -95,7 +95,7 @@ basicAuthPlugin:
       cpu: "20m"
 
 faasnetes:
-  image: ghcr.io/openfaas/faas-netes:0.13.2
+  image: ghcr.io/openfaas/faas-netes:0.13.4
   readTimeout : "60s"
   writeTimeout : "60s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
```

## 7.3.1 

**Release date:** 2021-06-15

![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update chart version after merge 
* Update OIDC plugin version 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 8294e4e2..83e153d8 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -48,7 +48,7 @@ oidcAuthPlugin:
       memory: "120Mi"
       cpu: "50m"
   replicas: 1
-  image: openfaas/openfaas-oidc-plugin:0.3.7
+  image: openfaas/openfaas-oidc-plugin:0.3.8
   securityContext: true
 
 # Requires openfaasPRO=true
```

## 7.3.0 

**Release date:** 2021-05-18

![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update Istio support in chart 
* fix: Remove label disabling istio injection on nats 
* fix: remove unneeded istio Policy and DestinationRules 
* feat: Add istio PeerAuthentication when mtls is enabled 
* feat: Add prometheus path label to the scrap configuration 
* fix!: Rename NATS Service port from clients to TCP 
* fix: Update prometheus scrape lables to use a slash 
* Update links for PRO features 

### Default value changes

```diff
# No changes in this release
```

## 7.2.8 

**Release date:** 2021-04-17

![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update chart for GHCR image for queue-worker 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 0bbd0a18..8294e4e2 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -127,7 +127,7 @@ operator:
       cpu: "50m"
 
 queueWorker:
-  image: openfaas/queue-worker:0.11.2
+  image: ghcr.io/openfaas/queue-worker:0.11.6
   # Control HA of queue-worker
   replicas: 1
   # Control the concurrent invocations
```

## 7.2.7 

**Release date:** 2021-04-17

![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Revert chart go docker hub for queue-worker 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index b56f9135..0bbd0a18 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -127,7 +127,7 @@ operator:
       cpu: "50m"
 
 queueWorker:
-  image: ghcr.io/openfaas/queue-worker:0.11.4
+  image: openfaas/queue-worker:0.11.2
   # Control HA of queue-worker
   replicas: 1
   # Control the concurrent invocations
```

## 7.2.6 

**Release date:** 2021-04-16

![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update faas-idler image version 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index a2f314ec..b56f9135 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -54,7 +54,7 @@ oidcAuthPlugin:
 # Requires openfaasPRO=true
 # scale-to-zero feature
 faasIdler:
-  image: ghcr.io/openfaas/faas-idler-pro:0.5.0
+  image: ghcr.io/openfaas/faas-idler-pro:0.4.2
   replicas: 1
   create: true
   inactivityDuration: 30m               # If a function is inactive for 15 minutes, it may be scaled to zero
```

## 7.2.5 

**Release date:** 2021-04-10

![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update queue-worker to latest GHCR image 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 84d36748..a2f314ec 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -127,7 +127,7 @@ operator:
       cpu: "50m"
 
 queueWorker:
-  image: openfaas/queue-worker:0.11.2
+  image: ghcr.io/openfaas/queue-worker:0.11.4
   # Control HA of queue-worker
   replicas: 1
   # Control the concurrent invocations
```

## 7.2.4 

**Release date:** 2021-04-06

![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Use multi-arch basic-auth image in chart 
* chore: Remove duplicate settings from the values files 
* feat: Remove nodeSelector from helm chart value 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 87575d9b..84d36748 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -54,7 +54,7 @@ oidcAuthPlugin:
 # Requires openfaasPRO=true
 # scale-to-zero feature
 faasIdler:
-  image: ghcr.io/openfaas/faas-idler-pro:0.4.2
+  image: ghcr.io/openfaas/faas-idler-pro:0.5.0
   replicas: 1
   create: true
   inactivityDuration: 30m               # If a function is inactive for 15 minutes, it may be scaled to zero
@@ -87,14 +87,13 @@ gateway:
       cpu: "50m"
 
 basicAuthPlugin:
-  image: openfaas/basic-auth-plugin:0.20.1
+  image: ghcr.io/openfaas/basic-auth:0.20.12
   replicas: 1
   resources:
     requests:
       memory: "50Mi"
       cpu: "20m"
 
-
 faasnetes:
   image: ghcr.io/openfaas/faas-netes:0.13.2
   readTimeout : "60s"
@@ -171,8 +170,9 @@ nats:
   image: nats-streaming:0.17.0
   enableMonitoring: false
   metrics:
+    # Should stay off by default because the exporter is not multi-arch (yet)
     enabled: false
-    image: synadia/prometheus-nats-exporter:0.6.2 
+    image: synadia/prometheus-nats-exporter:0.6.2
   resources:
     requests:
       memory: "120Mi"
@@ -201,8 +201,7 @@ ingressOperator:
     requests:
       memory: "25Mi"
 
-nodeSelector:
-  beta.kubernetes.io/arch: amd64
+nodeSelector: {}
 
 tolerations: []
 
```

## 7.2.3 

**Release date:** 2021-03-25

![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* gateway: bump gateway image to use the latest available 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 736b8196..87575d9b 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -67,7 +67,7 @@ faasIdler:
 ## OSS components
 
 gateway:
-  image: ghcr.io/openfaas/gateway:0.20.11
+  image: ghcr.io/openfaas/gateway:0.20.12
   readTimeout : "65s"
   writeTimeout : "65s"
   upstreamTimeout : "60s"  # Must be smaller than read/write_timeout
@@ -96,7 +96,7 @@ basicAuthPlugin:
 
 
 faasnetes:
-  image: ghcr.io/openfaas/faas-netes:0.13.1
+  image: ghcr.io/openfaas/faas-netes:0.13.2
   readTimeout : "60s"
   writeTimeout : "60s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
@@ -117,7 +117,7 @@ faasnetes:
 
 # replaces faas-netes with openfaas-operator
 operator:
-  image: ghcr.io/openfaas/faas-netes:0.13.1
+  image: ghcr.io/openfaas/faas-netes:0.13.2
   create: false
   # set this to false when creating multiple releases in the same cluster
   # must be true for the first one only
```

## 7.2.2 

**Release date:** 2021-03-02

![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update basic installation instructions 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 49a99706..736b8196 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -27,7 +27,7 @@ generateBasicAuth: false
 # image pull policy for openfaas components, can change to `IfNotPresent` in offline env
 openfaasImagePullPolicy: "Always"
 
-# openfaasPRO components, which require openfaasPRO=true
+# openfaasPRO components, which requires openfaasPRO=true
 oidcAuthPlugin:
   enabled: false
   provider: "" # Leave blank, or put "azure"
```

## 7.2.1 

**Release date:** 2021-02-27

![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump gateway and faas-netes 
* Add namespace to prom role 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 5c79acf8..49a99706 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -67,7 +67,7 @@ faasIdler:
 ## OSS components
 
 gateway:
-  image: ghcr.io/openfaas/gateway:0.20.8
+  image: ghcr.io/openfaas/gateway:0.20.11
   readTimeout : "65s"
   writeTimeout : "65s"
   upstreamTimeout : "60s"  # Must be smaller than read/write_timeout
@@ -96,7 +96,7 @@ basicAuthPlugin:
 
 
 faasnetes:
-  image: ghcr.io/openfaas/faas-netes:0.12.18
+  image: ghcr.io/openfaas/faas-netes:0.13.1
   readTimeout : "60s"
   writeTimeout : "60s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
@@ -117,7 +117,7 @@ faasnetes:
 
 # replaces faas-netes with openfaas-operator
 operator:
-  image: ghcr.io/openfaas/faas-netes:0.12.18
+  image: ghcr.io/openfaas/faas-netes:0.13.1
   create: false
   # set this to false when creating multiple releases in the same cluster
   # must be true for the first one only
```

## 7.2.0 

**Release date:** 2021-02-17

![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Move to explicit informers for Kubernetes objects 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 05d83af1..5c79acf8 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -96,7 +96,7 @@ basicAuthPlugin:
 
 
 faasnetes:
-  image: ghcr.io/openfaas/faas-netes:0.12.17
+  image: ghcr.io/openfaas/faas-netes:0.12.18
   readTimeout : "60s"
   writeTimeout : "60s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
@@ -117,7 +117,7 @@ faasnetes:
 
 # replaces faas-netes with openfaas-operator
 operator:
-  image: ghcr.io/openfaas/faas-netes:0.12.17
+  image: ghcr.io/openfaas/faas-netes:0.12.18
   create: false
   # set this to false when creating multiple releases in the same cluster
   # must be true for the first one only
```

## 7.1.2 

**Release date:** 2021-02-14

![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update faas-netes 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 41e4b303..05d83af1 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -96,7 +96,7 @@ basicAuthPlugin:
 
 
 faasnetes:
-  image: ghcr.io/openfaas/faas-netes:0.12.15
+  image: ghcr.io/openfaas/faas-netes:0.12.17
   readTimeout : "60s"
   writeTimeout : "60s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
@@ -117,7 +117,7 @@ faasnetes:
 
 # replaces faas-netes with openfaas-operator
 operator:
-  image: ghcr.io/openfaas/faas-netes:0.12.15
+  image: ghcr.io/openfaas/faas-netes:0.12.17
   create: false
   # set this to false when creating multiple releases in the same cluster
   # must be true for the first one only
```

## 7.1.1 

**Release date:** 2021-02-14

![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update gateway to 0.20.08 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 950bbd5c..41e4b303 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -67,7 +67,7 @@ faasIdler:
 ## OSS components
 
 gateway:
-  image: ghcr.io/openfaas/gateway:0.20.7
+  image: ghcr.io/openfaas/gateway:0.20.8
   readTimeout : "65s"
   writeTimeout : "65s"
   upstreamTimeout : "60s"  # Must be smaller than read/write_timeout
```

## 7.1.0 

**Release date:** 2021-02-08

![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Upgrade chart to indicate Helm 3 
* updated chart version to 0.20.7 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 90876a4f..950bbd5c 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -67,7 +67,7 @@ faasIdler:
 ## OSS components
 
 gateway:
-  image: ghcr.io/openfaas/gateway:0.20.5
+  image: ghcr.io/openfaas/gateway:0.20.7
   readTimeout : "65s"
   writeTimeout : "65s"
   upstreamTimeout : "60s"  # Must be smaller than read/write_timeout
```

## 7.0.4 

**Release date:** 2021-01-30

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update gw to GHCR and remove arm overrides 
* Allow configuring of prometheus deployment annotations via helm chart 
* Really fix profiles RBAC for operator 
* Update reference to show directFunctions is false by default 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 2ab68324..90876a4f 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -67,7 +67,7 @@ faasIdler:
 ## OSS components
 
 gateway:
-  image: openfaas/gateway:0.20.2
+  image: ghcr.io/openfaas/gateway:0.20.5
   readTimeout : "65s"
   writeTimeout : "65s"
   upstreamTimeout : "60s"  # Must be smaller than read/write_timeout
@@ -96,7 +96,7 @@ basicAuthPlugin:
 
 
 faasnetes:
-  image: ghcr.io/openfaas/faas-netes:0.12.13
+  image: ghcr.io/openfaas/faas-netes:0.12.15
   readTimeout : "60s"
   writeTimeout : "60s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
@@ -117,7 +117,7 @@ faasnetes:
 
 # replaces faas-netes with openfaas-operator
 operator:
-  image: ghcr.io/openfaas/faas-netes:0.12.13
+  image: ghcr.io/openfaas/faas-netes:0.12.15
   create: false
   # set this to false when creating multiple releases in the same cluster
   # must be true for the first one only
@@ -149,6 +149,7 @@ prometheus:
   resources:
     requests:
       memory: "512Mi"
+  annotations: {}
 
 alertmanager:
   image: prom/alertmanager:v0.18.0
```

## 7.0.3 

**Release date:** 2021-01-16

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Add Profile CRD to chart for operator 
* Enable profiles in operator RBAC w/o clusterrole 
* Update idler section 
* Add instructions for license key 

### Default value changes

```diff
# No changes in this release
```

## 7.0.2 

**Release date:** 2021-01-08

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Fix issue with faas-netes filtering 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index ae4f2cb3..2ab68324 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -96,7 +96,7 @@ basicAuthPlugin:
 
 
 faasnetes:
-  image: ghcr.io/openfaas/faas-netes:0.12.12
+  image: ghcr.io/openfaas/faas-netes:0.12.13
   readTimeout : "60s"
   writeTimeout : "60s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
@@ -117,7 +117,7 @@ faasnetes:
 
 # replaces faas-netes with openfaas-operator
 operator:
-  image: ghcr.io/openfaas/faas-netes:0.12.12
+  image: ghcr.io/openfaas/faas-netes:0.12.13
   create: false
   # set this to false when creating multiple releases in the same cluster
   # must be true for the first one only
```

## 7.0.1 

**Release date:** 2021-01-06

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update PRO scale to zero feature 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index cc229660..ae4f2cb3 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -52,14 +52,14 @@ oidcAuthPlugin:
   securityContext: true
 
 # Requires openfaasPRO=true
-# faas-idler configuration
+# scale-to-zero feature
 faasIdler:
-  image: ghcr.io/openfaas/faas-idler-pro:0.4.1
+  image: ghcr.io/openfaas/faas-idler-pro:0.4.2
   replicas: 1
   create: true
   inactivityDuration: 30m               # If a function is inactive for 15 minutes, it may be scaled to zero
   reconcileInterval: 2m                 # The interval between each attempt to scale functions to zero
-  dryRun: true                          # Set to false to enable the idler to apply changes and scale to zero
+  readOnly: true                        # When set to true, no functions are scaled to zero
   resources:
     requests:
       memory: "64Mi"
```

## 7.0.0 

**Release date:** 2021-01-06

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update charts to reflect new changes 
* Add OpenFaaS PRO idler to chart and rename oauth2 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index bc6cc8c6..cc229660 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -1,12 +1,20 @@
 functionNamespace: openfaas-fn  # Default namespace for functions
 
-async: true
+# See https://www.openfaas.com/support for more
+openfaasPRO: false
 
 exposeServices: true
+
+async: true
+
 serviceType: NodePort
+
 httpProbe: true               # Setting to true will use HTTP for readiness and liveness probe on the OpenFaaS system Pods (incompatible with Istio < 1.1.5)
+
 rbac: true
+
 clusterRole: false            # Set to true to have OpenFaaS administrate multiple namespaces
+
 createCRDs: true
 
 # create pod security policies for OpenFaaS control plane
@@ -19,8 +27,44 @@ generateBasicAuth: false
 # image pull policy for openfaas components, can change to `IfNotPresent` in offline env
 openfaasImagePullPolicy: "Always"
 
-gatewayExternal:
-  annotations: {}
+# openfaasPRO components, which require openfaasPRO=true
+oidcAuthPlugin:
+  enabled: false
+  provider: "" # Leave blank, or put "azure"
+  license: "example"
+  insecureTLS: false
+  scopes: "openid profile email"
+  jwksURL: https://example.eu.auth0.com/.well-known/jwks.json
+  tokenURL: https://example.eu.auth0.com/oauth/token
+  audience: https://example.eu.auth0.com/api/v2/
+  authorizeURL: https://example.eu.auth0.com/authorize
+  welcomePageURL: https://gw.oauth.example.com
+  cookieDomain: ".oauth.example.com"
+  baseHost: "http://auth.oauth.example.com"
+  clientSecret: SECRET
+  clientID: ID
+  resources:
+    requests:
+      memory: "120Mi"
+      cpu: "50m"
+  replicas: 1
+  image: openfaas/openfaas-oidc-plugin:0.3.7
+  securityContext: true
+
+# Requires openfaasPRO=true
+# faas-idler configuration
+faasIdler:
+  image: ghcr.io/openfaas/faas-idler-pro:0.4.1
+  replicas: 1
+  create: true
+  inactivityDuration: 30m               # If a function is inactive for 15 minutes, it may be scaled to zero
+  reconcileInterval: 2m                 # The interval between each attempt to scale functions to zero
+  dryRun: true                          # Set to false to enable the idler to apply changes and scale to zero
+  resources:
+    requests:
+      memory: "64Mi"
+
+## OSS components
 
 gateway:
   image: openfaas/gateway:0.20.2
@@ -50,28 +94,6 @@ basicAuthPlugin:
       memory: "50Mi"
       cpu: "20m"
 
-oauth2Plugin:
-  enabled: false
-  provider: "" # Leave blank, or put "azure"
-  license: "example"
-  insecureTLS: false
-  scopes: "openid profile email"
-  jwksURL: https://example.eu.auth0.com/.well-known/jwks.json
-  tokenURL: https://example.eu.auth0.com/oauth/token
-  audience: https://example.eu.auth0.com/api/v2/
-  authorizeURL: https://example.eu.auth0.com/authorize
-  welcomePageURL: https://gw.oauth.example.com
-  cookieDomain: ".oauth.example.com"
-  baseHost: "http://auth.oauth.example.com"
-  clientSecret: SECRET
-  clientID: ID
-  resources:
-    requests:
-      memory: "120Mi"
-      cpu: "50m"
-  replicas: 1
-  image: openfaas/openfaas-oidc-plugin:0.3.7
-  securityContext: true
 
 faasnetes:
   image: ghcr.io/openfaas/faas-netes:0.12.12
@@ -178,18 +200,6 @@ ingressOperator:
     requests:
       memory: "25Mi"
 
-# faas-idler configuration
-faasIdler:
-  image: openfaas/faas-idler:0.4.0
-  replicas: 1
-  create: true
-  inactivityDuration: 30m               # If a function is inactive for 15 minutes, it may be scaled to zero
-  reconcileInterval: 2m                 # The interval between each attempt to scale functions to zero
-  dryRun: true                          # Set to false to enable the idler to apply changes and scale to zero
-  resources:
-    requests:
-      memory: "64Mi"
-
 nodeSelector:
   beta.kubernetes.io/arch: amd64
 
@@ -201,3 +211,6 @@ kubernetesDNSDomain: cluster.local
 
 istio:
   mtls: false
+
+gatewayExternal:
+  annotations: {}
```

## 6.2.3 

**Release date:** 2020-12-30

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump faas-netes version 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index b3bac028..bc6cc8c6 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -74,7 +74,7 @@ oauth2Plugin:
   securityContext: true
 
 faasnetes:
-  image: ghcr.io/openfaas/faas-netes:0.12.9
+  image: ghcr.io/openfaas/faas-netes:0.12.12
   readTimeout : "60s"
   writeTimeout : "60s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
@@ -95,7 +95,7 @@ faasnetes:
 
 # replaces faas-netes with openfaas-operator
 operator:
-  image: ghcr.io/openfaas/faas-netes:0.12.9
+  image: ghcr.io/openfaas/faas-netes:0.12.12
   create: false
   # set this to false when creating multiple releases in the same cluster
   # must be true for the first one only
```

## 6.2.2 

**Release date:** 2020-12-11

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Disable direct_functions 
* Prevent duplicated namespace entries 
* Limit the Prometheus role to two namespaces 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 954550be..b3bac028 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -33,7 +33,7 @@ gateway:
   nodePort: 31112
   maxIdleConns: 1024
   maxIdleConnsPerHost: 1024
-  directFunctions: true
+  directFunctions: false
   # Custom logs provider url. For example openfaas-loki would be
   # "http://ofloki-openfaas-loki.openfaas:9191/"
   logsProviderURL: ""
```

## 6.2.1 

**Release date:** 2020-12-05

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Required fix for memory leak in gateway 
* Silence errors for namespace listing 
* Clarify intent of clusterRole flag in chart 
* Update CRD for IngressOperator 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 0e68e013..954550be 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -23,7 +23,7 @@ gatewayExternal:
   annotations: {}
 
 gateway:
-  image: openfaas/gateway:0.20.1
+  image: openfaas/gateway:0.20.2
   readTimeout : "65s"
   writeTimeout : "65s"
   upstreamTimeout : "60s"  # Must be smaller than read/write_timeout
@@ -74,7 +74,7 @@ oauth2Plugin:
   securityContext: true
 
 faasnetes:
-  image: ghcr.io/openfaas/faas-netes:0.12.8
+  image: ghcr.io/openfaas/faas-netes:0.12.9
   readTimeout : "60s"
   writeTimeout : "60s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
@@ -95,7 +95,7 @@ faasnetes:
 
 # replaces faas-netes with openfaas-operator
 operator:
-  image: ghcr.io/openfaas/faas-netes:0.12.8
+  image: ghcr.io/openfaas/faas-netes:0.12.9
   create: false
   # set this to false when creating multiple releases in the same cluster
   # must be true for the first one only
```

## 6.2.0 

**Release date:** 2020-11-23

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Fix #715 
* Update ARM64 images and enable auth for armhf 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 2ddd3023..0e68e013 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -171,7 +171,7 @@ ingress:
 # ingressOperator (optional) – component to have specific FQDN and TLS for Functions
 # https://github.com/openfaas-incubator/ingress-operator
 ingressOperator:
-  image: openfaas/ingress-operator:0.6.2
+  image: openfaas/ingress-operator:0.6.6
   replicas: 1
   create: false
   resources:
```

## 6.1.6 

**Release date:** 2020-11-17

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update armhf container images and faas-netes 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 91c5012e..2ddd3023 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -43,7 +43,7 @@ gateway:
       cpu: "50m"
 
 basicAuthPlugin:
-  image: openfaas/basic-auth-plugin:0.18.18
+  image: openfaas/basic-auth-plugin:0.20.1
   replicas: 1
   resources:
     requests:
@@ -74,7 +74,7 @@ oauth2Plugin:
   securityContext: true
 
 faasnetes:
-  image: openfaas/faas-netes:0.12.3
+  image: ghcr.io/openfaas/faas-netes:0.12.8
   readTimeout : "60s"
   writeTimeout : "60s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
@@ -95,7 +95,7 @@ faasnetes:
 
 # replaces faas-netes with openfaas-operator
 operator:
-  image: openfaas/faas-netes:0.12.3
+  image: ghcr.io/openfaas/faas-netes:0.12.8
   create: false
   # set this to false when creating multiple releases in the same cluster
   # must be true for the first one only
```

## 6.1.5 

**Release date:** 2020-11-17

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update faas-idler to 0.4.0 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 093c6986..91c5012e 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -180,7 +180,7 @@ ingressOperator:
 
 # faas-idler configuration
 faasIdler:
-  image: openfaas/faas-idler:0.3.0
+  image: openfaas/faas-idler:0.4.0
   replicas: 1
   create: true
   inactivityDuration: 30m               # If a function is inactive for 15 minutes, it may be scaled to zero
```

## 6.1.4 

**Release date:** 2020-11-17

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update chart to openfaas gateway 0.20.1 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 4011cf1d..093c6986 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -23,7 +23,7 @@ gatewayExternal:
   annotations: {}
 
 gateway:
-  image: openfaas/gateway:0.20.0
+  image: openfaas/gateway:0.20.1
   readTimeout : "65s"
   writeTimeout : "65s"
   upstreamTimeout : "60s"  # Must be smaller than read/write_timeout
```

## 6.1.3 

**Release date:** 2020-11-17

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update gateway version for UI fixes 
* Update CRD specs to use apiextensions v1 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index e0a74aec..4011cf1d 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -23,7 +23,7 @@ gatewayExternal:
   annotations: {}
 
 gateway:
-  image: openfaas/gateway:0.18.18
+  image: openfaas/gateway:0.20.0
   readTimeout : "65s"
   writeTimeout : "65s"
   upstreamTimeout : "60s"  # Must be smaller than read/write_timeout
```

## 6.1.2 

**Release date:** 2020-11-09

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update helm chart for Prometheus role 
* Skip multiple execution of RoleBinding's on xaas-faas 
* Updated ARM helm charts to reference the same image versions as those used for amd64 

### Default value changes

```diff
# No changes in this release
```

## 6.1.1 

**Release date:** 2020-11-04

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Regenerate chart 
* Update README with new createCRDs flag 

### Default value changes

```diff
# No changes in this release
```

## 6.1.0 

**Release date:** 2020-11-03

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Profiles CRD toggle 
* Update chart port for forwarding 
* Add instructions for arkade to the helm chart README 
* Remove HELM.md instructions 
* deleted duplicate mapping in operator-rbac.yaml 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 1f5ed8c4..e0a74aec 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -7,6 +7,7 @@ serviceType: NodePort
 httpProbe: true               # Setting to true will use HTTP for readiness and liveness probe on the OpenFaaS system Pods (incompatible with Istio < 1.1.5)
 rbac: true
 clusterRole: false            # Set to true to have OpenFaaS administrate multiple namespaces
+createCRDs: true
 
 # create pod security policies for OpenFaaS control plane
 # https://kubernetes.io/docs/concepts/policy/pod-security-policy/
```

## 6.0.4 

**Release date:** 2020-09-21

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump OIDC version for faas-netes 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index bbc51129..1f5ed8c4 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -69,7 +69,7 @@ oauth2Plugin:
       memory: "120Mi"
       cpu: "50m"
   replicas: 1
-  image: openfaas/openfaas-oidc-plugin:0.3.5
+  image: openfaas/openfaas-oidc-plugin:0.3.7
   securityContext: true
 
 faasnetes:
```

## 6.0.3 

**Release date:** 2020-09-17

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update OIDC plugin version 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index d4eea219..bbc51129 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -69,7 +69,7 @@ oauth2Plugin:
       memory: "120Mi"
       cpu: "50m"
   replicas: 1
-  image: openfaas/openfaas-oidc-plugin:0.3.3
+  image: openfaas/openfaas-oidc-plugin:0.3.5
   securityContext: true
 
 faasnetes:
```

## 6.0.2 

**Release date:** 2020-09-17

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update chart to latest faas-netes version 0.12.3 
* Add multiple namespaces in the operator 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 0369eb36..d4eea219 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -42,7 +42,7 @@ gateway:
       cpu: "50m"
 
 basicAuthPlugin:
-  image: openfaas/basic-auth-plugin:0.18.17
+  image: openfaas/basic-auth-plugin:0.18.18
   replicas: 1
   resources:
     requests:
@@ -73,7 +73,7 @@ oauth2Plugin:
   securityContext: true
 
 faasnetes:
-  image: openfaas/faas-netes:0.12.2
+  image: openfaas/faas-netes:0.12.3
   readTimeout : "60s"
   writeTimeout : "60s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
@@ -94,7 +94,7 @@ faasnetes:
 
 # replaces faas-netes with openfaas-operator
 operator:
-  image: openfaas/faas-netes:0.12.2
+  image: openfaas/faas-netes:0.12.3
   create: false
   # set this to false when creating multiple releases in the same cluster
   # must be true for the first one only
@@ -105,7 +105,7 @@ operator:
       cpu: "50m"
 
 queueWorker:
-  image: openfaas/queue-worker:0.11.0
+  image: openfaas/queue-worker:0.11.2
   # Control HA of queue-worker
   replicas: 1
   # Control the concurrent invocations
```

## 6.0.1 

**Release date:** 2020-08-04

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update chart for Profiles fix #675 
* fix for #675 
* Fix local chart testing instructions 

### Default value changes

```diff
# No changes in this release
```

## 6.0.0 

**Release date:** 2020-07-20

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update chart to include Profiles feature 
* Split profile RBAC to separate role 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index d5170f1a..0369eb36 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -22,7 +22,7 @@ gatewayExternal:
   annotations: {}
 
 gateway:
-  image: openfaas/gateway:0.18.17
+  image: openfaas/gateway:0.18.18
   readTimeout : "65s"
   writeTimeout : "65s"
   upstreamTimeout : "60s"  # Must be smaller than read/write_timeout
@@ -73,7 +73,7 @@ oauth2Plugin:
   securityContext: true
 
 faasnetes:
-  image: openfaas/faas-netes:0.10.5
+  image: openfaas/faas-netes:0.12.2
   readTimeout : "60s"
   writeTimeout : "60s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
@@ -94,7 +94,7 @@ faasnetes:
 
 # replaces faas-netes with openfaas-operator
 operator:
-  image: openfaas/faas-netes:0.10.5
+  image: openfaas/faas-netes:0.12.2
   create: false
   # set this to false when creating multiple releases in the same cluster
   # must be true for the first one only
```

## 5.8.8 

**Release date:** 2020-07-15

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Regenerate openfaas helm chart 
* Make the Profile CRD required via the helm chart 
* Update chart and ensure profile namespace is passed down 
* Fix policy remove 
* Rename policy to profile 

### Default value changes

```diff
# No changes in this release
```

## 5.8.7 

**Release date:** 2020-06-18

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update ingress operator CRD 
* Add Policy CRD 

### Default value changes

```diff
# No changes in this release
```

## 5.8.6 

**Release date:** 2020-06-15

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Publish new chart for operator clusterrole 
* Add cluster role and binding to operator 

### Default value changes

```diff
# No changes in this release
```

## 5.8.5 

**Release date:** 2020-06-09

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump openfaas chart version 
* Fix issue with ingress operator looking to a wrong namespace Fix issue when ingress operator was looking for crds and ingresses in openfaas namespace while the chart was deployed to any other 
* Fix IngressOperator install from OpenFaaS Chart 

### Default value changes

```diff
# No changes in this release
```

## 5.8.4 

**Release date:** 2020-05-28

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update ingress-operator version 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index f76ca0c3..d5170f1a 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -170,7 +170,7 @@ ingress:
 # ingressOperator (optional) – component to have specific FQDN and TLS for Functions
 # https://github.com/openfaas-incubator/ingress-operator
 ingressOperator:
-  image: openfaas/ingress-operator:0.5.0
+  image: openfaas/ingress-operator:0.6.2
   replicas: 1
   create: false
   resources:
```

## 5.8.3 

**Release date:** 2020-05-22

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Add NATS monitoring feature 
* Expose nats monitoring metrics to prometheus 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index effbe7e5..f76ca0c3 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -146,6 +146,9 @@ nats:
     port: ""
   image: nats-streaming:0.17.0
   enableMonitoring: false
+  metrics:
+    enabled: false
+    image: synadia/prometheus-nats-exporter:0.6.2 
   resources:
     requests:
       memory: "120Mi"
```

## 5.8.2 

**Release date:** 2020-05-08

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Fix for K8s 1.18 in faas-netes 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index f552b563..effbe7e5 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -73,7 +73,7 @@ oauth2Plugin:
   securityContext: true
 
 faasnetes:
-  image: openfaas/faas-netes:0.10.3
+  image: openfaas/faas-netes:0.10.5
   readTimeout : "60s"
   writeTimeout : "60s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
@@ -94,7 +94,7 @@ faasnetes:
 
 # replaces faas-netes with openfaas-operator
 operator:
-  image: openfaas/faas-netes:0.10.3
+  image: openfaas/faas-netes:0.10.5
   create: false
   # set this to false when creating multiple releases in the same cluster
   # must be true for the first one only
```

## 5.8.1 

**Release date:** 2020-05-04

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump chart to fix openfaasImagePullPolicy 
* Fix typo in helm chart for openfaas 

### Default value changes

```diff
# No changes in this release
```

## 5.8.0 

**Release date:** 2020-05-01

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update helm chart to include max_inflight 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index f11921e9..f552b563 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -42,7 +42,7 @@ gateway:
       cpu: "50m"
 
 basicAuthPlugin:
-  image: openfaas/basic-auth-plugin:0.18.11
+  image: openfaas/basic-auth-plugin:0.18.17
   replicas: 1
   resources:
     requests:
@@ -105,12 +105,14 @@ operator:
       cpu: "50m"
 
 queueWorker:
-  durableQueueSubscription: false
-  queueGroup: "faas"
-  image: openfaas/queue-worker:0.10.1
-  ackWait : "60s"
+  image: openfaas/queue-worker:0.11.0
+  # Control HA of queue-worker
   replicas: 1
+  # Control the concurrent invocations
+  maxInflight: 1
   gatewayInvoke: true
+  queueGroup: "faas"
+  ackWait : "60s"
   resources:
     requests:
       memory: "120Mi"
```

## 5.7.2 

**Release date:** 2020-04-24

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Scrape function Pods 

### Default value changes

```diff
# No changes in this release
```

## 5.7.1 

**Release date:** 2020-04-23

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Namespace support for logs 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index f6d56e2d..f11921e9 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -73,7 +73,7 @@ oauth2Plugin:
   securityContext: true
 
 faasnetes:
-  image: openfaas/faas-netes:0.10.2
+  image: openfaas/faas-netes:0.10.3
   readTimeout : "60s"
   writeTimeout : "60s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
@@ -94,7 +94,7 @@ faasnetes:
 
 # replaces faas-netes with openfaas-operator
 operator:
-  image: openfaas/faas-netes:0.10.2
+  image: openfaas/faas-netes:0.10.3
   create: false
   # set this to false when creating multiple releases in the same cluster
   # must be true for the first one only
```

## 5.7.0 

**Release date:** 2020-04-22

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump gateway for multiple queue support 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 5475149d..f6d56e2d 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -22,7 +22,7 @@ gatewayExternal:
   annotations: {}
 
 gateway:
-  image: openfaas/gateway:0.18.16
+  image: openfaas/gateway:0.18.17
   readTimeout : "65s"
   writeTimeout : "65s"
   upstreamTimeout : "60s"  # Must be smaller than read/write_timeout
```

## 5.6.7 

**Release date:** 2020-04-22

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update queue-worker and gateway 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index d42a493b..5475149d 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -22,7 +22,7 @@ gatewayExternal:
   annotations: {}
 
 gateway:
-  image: openfaas/gateway:0.18.13
+  image: openfaas/gateway:0.18.16
   readTimeout : "65s"
   writeTimeout : "65s"
   upstreamTimeout : "60s"  # Must be smaller than read/write_timeout
@@ -107,7 +107,7 @@ operator:
 queueWorker:
   durableQueueSubscription: false
   queueGroup: "faas"
-  image: openfaas/queue-worker:0.10.0
+  image: openfaas/queue-worker:0.10.1
   ackWait : "60s"
   replicas: 1
   gatewayInvoke: true
```

## 5.6.6 

**Release date:** 2020-04-22

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump queue-worker 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 3a05df41..d42a493b 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -73,7 +73,7 @@ oauth2Plugin:
   securityContext: true
 
 faasnetes:
-  image: openfaas/faas-netes:0.10.2-rc1
+  image: openfaas/faas-netes:0.10.2
   readTimeout : "60s"
   writeTimeout : "60s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
@@ -94,7 +94,7 @@ faasnetes:
 
 # replaces faas-netes with openfaas-operator
 operator:
-  image: openfaas/faas-netes:0.10.2-rc1
+  image: openfaas/faas-netes:0.10.2
   create: false
   # set this to false when creating multiple releases in the same cluster
   # must be true for the first one only
@@ -107,7 +107,7 @@ operator:
 queueWorker:
   durableQueueSubscription: false
   queueGroup: "faas"
-  image: openfaas/queue-worker:0.9.0
+  image: openfaas/queue-worker:0.10.0
   ackWait : "60s"
   replicas: 1
   gatewayInvoke: true
```

## 5.6.5 

**Release date:** 2020-04-16

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update armhf and arm64 images for merged operator 
* Merge the operator and faas-netes 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index c0bd02c6..3a05df41 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -73,7 +73,7 @@ oauth2Plugin:
   securityContext: true
 
 faasnetes:
-  image: openfaas/faas-netes:0.10.1
+  image: openfaas/faas-netes:0.10.2-rc1
   readTimeout : "60s"
   writeTimeout : "60s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
@@ -94,7 +94,7 @@ faasnetes:
 
 # replaces faas-netes with openfaas-operator
 operator:
-  image: openfaas/openfaas-operator:0.14.3
+  image: openfaas/faas-netes:0.10.2-rc1
   create: false
   # set this to false when creating multiple releases in the same cluster
   # must be true for the first one only
```

## 5.6.4 

**Release date:** 2020-04-09

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update Operator and OIDC auth plugin 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index efb64071..c0bd02c6 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -69,7 +69,7 @@ oauth2Plugin:
       memory: "120Mi"
       cpu: "50m"
   replicas: 1
-  image: alexellis2/openfaas-oidc-plugin:0.3.0
+  image: openfaas/openfaas-oidc-plugin:0.3.3
   securityContext: true
 
 faasnetes:
@@ -94,7 +94,7 @@ faasnetes:
 
 # replaces faas-netes with openfaas-operator
 operator:
-  image: openfaas/openfaas-operator:0.14.2
+  image: openfaas/openfaas-operator:0.14.3
   create: false
   # set this to false when creating multiple releases in the same cluster
   # must be true for the first one only
```

## 5.6.3 

**Release date:** 2020-04-03

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Add basic auth to oauth plugin 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 46c94879..efb64071 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -51,6 +51,7 @@ basicAuthPlugin:
 
 oauth2Plugin:
   enabled: false
+  provider: "" # Leave blank, or put "azure"
   license: "example"
   insecureTLS: false
   scopes: "openid profile email"
@@ -68,7 +69,7 @@ oauth2Plugin:
       memory: "120Mi"
       cpu: "50m"
   replicas: 1
-  image: alexellis2/openfaas-oidc-plugin:0.2.5
+  image: alexellis2/openfaas-oidc-plugin:0.3.0
   securityContext: true
 
 faasnetes:
```

## 5.6.2 

**Release date:** 2020-03-24

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump gateway for idler changes 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 72fb5c50..46c94879 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -22,7 +22,7 @@ gatewayExternal:
   annotations: {}
 
 gateway:
-  image: openfaas/gateway:0.18.11
+  image: openfaas/gateway:0.18.13
   readTimeout : "65s"
   writeTimeout : "65s"
   upstreamTimeout : "60s"  # Must be smaller than read/write_timeout
```

## 5.6.1 

**Release date:** 2020-03-19

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update operator to fix bug with constraints 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index d702490c..72fb5c50 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -93,7 +93,7 @@ faasnetes:
 
 # replaces faas-netes with openfaas-operator
 operator:
-  image: openfaas/openfaas-operator:0.14.1
+  image: openfaas/openfaas-operator:0.14.2
   create: false
   # set this to false when creating multiple releases in the same cluster
   # must be true for the first one only
```

## 5.6.0 

**Release date:** 2020-03-19

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update helm chart 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 67a9c681..d702490c 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -173,7 +173,7 @@ ingressOperator:
 
 # faas-idler configuration
 faasIdler:
-  image: openfaas/faas-idler:0.2.1
+  image: openfaas/faas-idler:0.3.0
   replicas: 1
   create: true
   inactivityDuration: 30m               # If a function is inactive for 15 minutes, it may be scaled to zero
```

## 5.5.6 

**Release date:** 2020-03-05

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump OpenFaaS gateway to 0.18.11 for padding issue in UI 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index a5d7d91b..67a9c681 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -22,7 +22,7 @@ gatewayExternal:
   annotations: {}
 
 gateway:
-  image: openfaas/gateway:0.18.10
+  image: openfaas/gateway:0.18.11
   readTimeout : "65s"
   writeTimeout : "65s"
   upstreamTimeout : "60s"  # Must be smaller than read/write_timeout
@@ -42,7 +42,7 @@ gateway:
       cpu: "50m"
 
 basicAuthPlugin:
-  image: openfaas/basic-auth-plugin:0.17.0
+  image: openfaas/basic-auth-plugin:0.18.11
   replicas: 1
   resources:
     requests:
```

## 5.5.5 

**Release date:** 2020-02-25

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update nats-streaming docker tag to 0.17.0 
* Highlight steps for ARM 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index ee7d6853..a5d7d91b 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -141,7 +141,7 @@ nats:
     enabled: false
     host: ""
     port: ""
-  image: nats-streaming:0.11.2
+  image: nats-streaming:0.17.0
   enableMonitoring: false
   resources:
     requests:
```

## 5.5.4 

**Release date:** 2020-02-09

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump to 0.2.5 of ODIC plugin 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 1c80ea1e..ee7d6853 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -68,7 +68,7 @@ oauth2Plugin:
       memory: "120Mi"
       cpu: "50m"
   replicas: 1
-  image: alexellis2/openfaas-oidc-plugin:0.2.4
+  image: alexellis2/openfaas-oidc-plugin:0.2.5
   securityContext: true
 
 faasnetes:
```

## 5.5.3 

**Release date:** 2020-02-09

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Fix chart for OIDC 
* Update openfaas-operator and CRD - bump operator version to 0.14.1 - remove glog args from operator deployment - update Function CRD to v1 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 616e4d52..1c80ea1e 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -63,13 +63,12 @@ oauth2Plugin:
   baseHost: "http://auth.oauth.example.com"
   clientSecret: SECRET
   clientID: ID
-
   resources:
     requests:
       memory: "120Mi"
       cpu: "50m"
   replicas: 1
-  image: alexellis2/openfaas-oidc-plugin:0.2.5-arm64
+  image: alexellis2/openfaas-oidc-plugin:0.2.4
   securityContext: true
 
 faasnetes:
@@ -94,7 +93,7 @@ faasnetes:
 
 # replaces faas-netes with openfaas-operator
 operator:
-  image: openfaas/openfaas-operator:0.12.0
+  image: openfaas/openfaas-operator:0.14.1
   create: false
   # set this to false when creating multiple releases in the same cluster
   # must be true for the first one only
```

## 5.5.2 

**Release date:** 2020-02-06

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update faas-netes and gateway versions 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 09e2a391..616e4d52 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -22,7 +22,7 @@ gatewayExternal:
   annotations: {}
 
 gateway:
-  image: openfaas/gateway:0.18.7
+  image: openfaas/gateway:0.18.10
   readTimeout : "65s"
   writeTimeout : "65s"
   upstreamTimeout : "60s"  # Must be smaller than read/write_timeout
@@ -73,7 +73,7 @@ oauth2Plugin:
   securityContext: true
 
 faasnetes:
-  image: openfaas/faas-netes:0.10.0
+  image: openfaas/faas-netes:0.10.1
   readTimeout : "60s"
   writeTimeout : "60s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
```

## 5.5.1 

**Release date:** 2020-02-04

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update RBAC for IngressOperator 

### Default value changes

```diff
# No changes in this release
```

## 5.5.0 

**Release date:** 2020-02-04

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump chart to use faas-netes 0.10.0 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 60fbf065..09e2a391 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -73,7 +73,7 @@ oauth2Plugin:
   securityContext: true
 
 faasnetes:
-  image: openfaas/faas-netes:0.9.15
+  image: openfaas/faas-netes:0.10.0
   readTimeout : "60s"
   writeTimeout : "60s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
```

## 5.4.2 

**Release date:** 2020-01-29

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump chart version 
* Add ability to change logs_provider_url env variable 
* Add oauth2Plugin configuration 
* Add notes on getting help 
* Add helm3 usage 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 299ec03f..60fbf065 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -33,11 +33,45 @@ gateway:
   maxIdleConns: 1024
   maxIdleConnsPerHost: 1024
   directFunctions: true
+  # Custom logs provider url. For example openfaas-loki would be
+  # "http://ofloki-openfaas-loki.openfaas:9191/"
+  logsProviderURL: ""
   resources:
     requests:
       memory: "120Mi"
       cpu: "50m"
 
+basicAuthPlugin:
+  image: openfaas/basic-auth-plugin:0.17.0
+  replicas: 1
+  resources:
+    requests:
+      memory: "50Mi"
+      cpu: "20m"
+
+oauth2Plugin:
+  enabled: false
+  license: "example"
+  insecureTLS: false
+  scopes: "openid profile email"
+  jwksURL: https://example.eu.auth0.com/.well-known/jwks.json
+  tokenURL: https://example.eu.auth0.com/oauth/token
+  audience: https://example.eu.auth0.com/api/v2/
+  authorizeURL: https://example.eu.auth0.com/authorize
+  welcomePageURL: https://gw.oauth.example.com
+  cookieDomain: ".oauth.example.com"
+  baseHost: "http://auth.oauth.example.com"
+  clientSecret: SECRET
+  clientID: ID
+
+  resources:
+    requests:
+      memory: "120Mi"
+      cpu: "50m"
+  replicas: 1
+  image: alexellis2/openfaas-oidc-plugin:0.2.5-arm64
+  securityContext: true
+
 faasnetes:
   image: openfaas/faas-netes:0.9.15
   readTimeout : "60s"
@@ -150,14 +184,6 @@ faasIdler:
     requests:
       memory: "64Mi"
 
-basicAuthPlugin:
-  image: openfaas/basic-auth-plugin:0.17.0
-  replicas: 1
-  resources:
-    requests:
-      memory: "50Mi"
-      cpu: "20m"
-
 nodeSelector:
   beta.kubernetes.io/arch: amd64
 
```

## 5.4.1 

**Release date:** 2020-01-14

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump IngressOperator version to support paths 
* Update README.md 
* Fix a typo in the async URL 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index b65730a2..299ec03f 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -131,7 +131,7 @@ ingress:
 # ingressOperator (optional) – component to have specific FQDN and TLS for Functions
 # https://github.com/openfaas-incubator/ingress-operator
 ingressOperator:
-  image: openfaas/ingress-operator:0.4.0
+  image: openfaas/ingress-operator:0.5.0
   replicas: 1
   create: false
   resources:
@@ -143,8 +143,8 @@ faasIdler:
   image: openfaas/faas-idler:0.2.1
   replicas: 1
   create: true
-  inactivityDuration: 15m               # If a function is inactive for 15 minutes, it may be scaled to zero
-  reconcileInterval: 1m                 # The interval between each attempt to scale functions to zero
+  inactivityDuration: 30m               # If a function is inactive for 15 minutes, it may be scaled to zero
+  reconcileInterval: 2m                 # The interval between each attempt to scale functions to zero
   dryRun: true                          # Set to false to enable the idler to apply changes and scale to zero
   resources:
     requests:
```

## 5.4.0 

**Release date:** 2019-12-23

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Add generateBasicAuth option 
* Generate basic auth credentials on Helm install - add basicAuthAutogen boolean to chart options - generate admin password and basic auth secret with a pre-install Helm hook - add credentials retrieval commands to chart readme - tested on EKS 1.14 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index fdce5ac6..b65730a2 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -13,6 +13,7 @@ clusterRole: false            # Set to true to have OpenFaaS administrate multip
 psp: false
 securityContext: true
 basic_auth: true
+generateBasicAuth: false
 
 # image pull policy for openfaas components, can change to `IfNotPresent` in offline env
 openfaasImagePullPolicy: "Always"
```

## 5.3.11 

**Release date:** 2019-12-19

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump gateway 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index bafe41e0..fdce5ac6 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -21,7 +21,7 @@ gatewayExternal:
   annotations: {}
 
 gateway:
-  image: openfaas/gateway:0.18.6
+  image: openfaas/gateway:0.18.7
   readTimeout : "65s"
   writeTimeout : "65s"
   upstreamTimeout : "60s"  # Must be smaller than read/write_timeout
```

## 5.3.10 

**Release date:** 2019-12-17

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update faas-netes for namespaces endpoint 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 1e5e6f64..bafe41e0 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -38,7 +38,7 @@ gateway:
       cpu: "50m"
 
 faasnetes:
-  image: openfaas/faas-netes:0.9.13
+  image: openfaas/faas-netes:0.9.15
   readTimeout : "60s"
   writeTimeout : "60s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
```

## 5.3.9 

**Release date:** 2019-12-17

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update gateway to 0.18.6 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index e9e72bf4..1e5e6f64 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -21,7 +21,7 @@ gatewayExternal:
   annotations: {}
 
 gateway:
-  image: openfaas/gateway:0.18.5
+  image: openfaas/gateway:0.18.6
   readTimeout : "65s"
   writeTimeout : "65s"
   upstreamTimeout : "60s"  # Must be smaller than read/write_timeout
```

## 5.3.8 

**Release date:** 2019-12-10

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump 'openfaas/queue-worker' and 'openfaas/gateway'. 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 643a5bd6..e9e72bf4 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -21,7 +21,7 @@ gatewayExternal:
   annotations: {}
 
 gateway:
-  image: openfaas/gateway:0.18.3
+  image: openfaas/gateway:0.18.5
   readTimeout : "65s"
   writeTimeout : "65s"
   upstreamTimeout : "60s"  # Must be smaller than read/write_timeout
@@ -70,7 +70,9 @@ operator:
       cpu: "50m"
 
 queueWorker:
-  image: openfaas/queue-worker:0.8.4
+  durableQueueSubscription: false
+  queueGroup: "faas"
+  image: openfaas/queue-worker:0.9.0
   ackWait : "60s"
   replicas: 1
   gatewayInvoke: true
@@ -99,6 +101,7 @@ alertmanager:
 
 # async provider
 nats:
+  channel: "faas-request"
   external:
     clusterName: ""
     enabled: false
```

## 5.3.7 

**Release date:** 2019-12-04

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Add nodeSelector for CPU architecture 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index b654f423..643a5bd6 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -154,7 +154,8 @@ basicAuthPlugin:
       memory: "50Mi"
       cpu: "20m"
 
-nodeSelector: {}
+nodeSelector:
+  beta.kubernetes.io/arch: amd64
 
 tolerations: []
 
```

## 5.3.6 

**Release date:** 2019-11-26

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump 'openfaas/queue-worker'. 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 8dcb1f11..b654f423 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -70,7 +70,7 @@ operator:
       cpu: "50m"
 
 queueWorker:
-  image: openfaas/queue-worker:0.8.0
+  image: openfaas/queue-worker:0.8.4
   ackWait : "60s"
   replicas: 1
   gatewayInvoke: true
```

## 5.3.5 

**Release date:** 2019-11-25

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Revert queue-worker version 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index fbf6f139..8dcb1f11 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -70,7 +70,7 @@ operator:
       cpu: "50m"
 
 queueWorker:
-  image: openfaas/queue-worker:0.8.3
+  image: openfaas/queue-worker:0.8.0
   ackWait : "60s"
   replicas: 1
   gatewayInvoke: true
```

## 5.3.4 

**Release date:** 2019-11-25

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update Operator RBAC permissions 

### Default value changes

```diff
# No changes in this release
```

## 5.3.3 

**Release date:** 2019-11-25

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump version of openfaas-operator 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index ad53f26c..fbf6f139 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -59,7 +59,7 @@ faasnetes:
 
 # replaces faas-netes with openfaas-operator
 operator:
-  image: openfaas/openfaas-operator:0.9.9
+  image: openfaas/openfaas-operator:0.12.0
   create: false
   # set this to false when creating multiple releases in the same cluster
   # must be true for the first one only
```

## 5.3.2 

**Release date:** 2019-11-25

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump chart to 5.3.2 
* Bump 'faas-netes', 'gateway' and 'nats-queue-worker'. 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index f566bf8b..ad53f26c 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -21,7 +21,7 @@ gatewayExternal:
   annotations: {}
 
 gateway:
-  image: openfaas/gateway:0.18.2
+  image: openfaas/gateway:0.18.3
   readTimeout : "65s"
   writeTimeout : "65s"
   upstreamTimeout : "60s"  # Must be smaller than read/write_timeout
@@ -38,7 +38,7 @@ gateway:
       cpu: "50m"
 
 faasnetes:
-  image: openfaas/faas-netes:0.9.9
+  image: openfaas/faas-netes:0.9.13
   readTimeout : "60s"
   writeTimeout : "60s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
@@ -70,7 +70,7 @@ operator:
       cpu: "50m"
 
 queueWorker:
-  image: openfaas/queue-worker:0.8.0
+  image: openfaas/queue-worker:0.8.3
   ackWait : "60s"
   replicas: 1
   gatewayInvoke: true
```

## 5.3.1 

**Release date:** 2019-11-25

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Regenerate charts 
* Add RBAC for logs / namespaces / endpoint invocation 

### Default value changes

```diff
# No changes in this release
```

## 5.3.0 

**Release date:** 2019-11-22

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Allow for using an externally-managed NATS Streaming server. 
* Fix PSP name in ClusterRole and Rolebinding Namespace 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 8c2273f3..f566bf8b 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -99,6 +99,11 @@ alertmanager:
 
 # async provider
 nats:
+  external:
+    clusterName: ""
+    enabled: false
+    host: ""
+    port: ""
   image: nats-streaming:0.11.2
   enableMonitoring: false
   resources:
```

## 5.2.2 

**Release date:** 2019-11-07

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump faas-netes to 0.9.9 
* Update for .Values.gateway.directFunctions value 
* Update for feedback 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index a0b3beba..8c2273f3 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -38,7 +38,7 @@ gateway:
       cpu: "50m"
 
 faasnetes:
-  image: openfaas/faas-netes:0.9.8
+  image: openfaas/faas-netes:0.9.9
   readTimeout : "60s"
   writeTimeout : "60s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
```

## 5.2.1 

**Release date:** 2019-10-24

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Add RBAC for endpoints 
* Update notes on endpoint balancing 
* Update note on Endpoint load-balancing 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 58abfdeb..a0b3beba 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -31,7 +31,7 @@ gateway:
   nodePort: 31112
   maxIdleConns: 1024
   maxIdleConnsPerHost: 1024
-  directFunctions: false
+  directFunctions: true
   resources:
     requests:
       memory: "120Mi"
```

## 5.2.0 

**Release date:** 2019-10-24

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Add option to Chart for direct_functions 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 63014194..58abfdeb 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -21,7 +21,7 @@ gatewayExternal:
   annotations: {}
 
 gateway:
-  image: openfaas/gateway:0.18.0
+  image: openfaas/gateway:0.18.2
   readTimeout : "65s"
   writeTimeout : "65s"
   upstreamTimeout : "60s"  # Must be smaller than read/write_timeout
@@ -31,13 +31,14 @@ gateway:
   nodePort: 31112
   maxIdleConns: 1024
   maxIdleConnsPerHost: 1024
+  directFunctions: false
   resources:
     requests:
       memory: "120Mi"
       cpu: "50m"
 
 faasnetes:
-  image: openfaas/faas-netes:0.9.4
+  image: openfaas/faas-netes:0.9.8
   readTimeout : "60s"
   writeTimeout : "60s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
```

## 5.1.5 

**Release date:** 2019-10-21

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump ARM images 

### Default value changes

```diff
# No changes in this release
```

## 5.1.4 

**Release date:** 2019-10-04

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Name HTTP port for the OpenFaaS gateway 

### Default value changes

```diff
# No changes in this release
```

## 5.1.3 

**Release date:** 2019-09-29

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump to faas-netes 0.9.4 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 38526d3b..63014194 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -37,7 +37,7 @@ gateway:
       cpu: "50m"
 
 faasnetes:
-  image: openfaas/faas-netes:0.9.3
+  image: openfaas/faas-netes:0.9.4
   readTimeout : "60s"
   writeTimeout : "60s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
```

## 5.1.2 

**Release date:** 2019-09-29

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Move to faas-netes 0.9.3 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 7429d25f..38526d3b 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -37,7 +37,7 @@ gateway:
       cpu: "50m"
 
 faasnetes:
-  image: openfaas/faas-netes:0.9.2
+  image: openfaas/faas-netes:0.9.3
   readTimeout : "60s"
   writeTimeout : "60s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
```

## 5.1.1 

**Release date:** 2019-09-29

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Revert faas-netes 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 38526d3b..7429d25f 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -37,7 +37,7 @@ gateway:
       cpu: "50m"
 
 faasnetes:
-  image: openfaas/faas-netes:0.9.3
+  image: openfaas/faas-netes:0.9.2
   readTimeout : "60s"
   writeTimeout : "60s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
```

## 5.1.0 

**Release date:** 2019-09-28

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump faas-netes to 0.9.3 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 7429d25f..38526d3b 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -37,7 +37,7 @@ gateway:
       cpu: "50m"
 
 faasnetes:
-  image: openfaas/faas-netes:0.9.2
+  image: openfaas/faas-netes:0.9.3
   readTimeout : "60s"
   writeTimeout : "60s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
```

## 5.0.1 

**Release date:** 2019-09-22

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update chart to latest faas-netes/gateway 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 6762c9b5..7429d25f 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -21,7 +21,7 @@ gatewayExternal:
   annotations: {}
 
 gateway:
-  image: openfaas/gateway:0.17.4
+  image: openfaas/gateway:0.18.0
   readTimeout : "65s"
   writeTimeout : "65s"
   upstreamTimeout : "60s"  # Must be smaller than read/write_timeout
@@ -37,7 +37,7 @@ gateway:
       cpu: "50m"
 
 faasnetes:
-  image: openfaas/faas-netes:0.9.0
+  image: openfaas/faas-netes:0.9.2
   readTimeout : "60s"
   writeTimeout : "60s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
```

## 5.0.0 

**Release date:** 2019-09-21

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump to faas-netes 0.9.0 / apps/v1 
* Update templates to support Kubernetes 1.16 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index be7fdc22..6762c9b5 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -1,4 +1,4 @@
-functionNamespace: openfaas-fn
+functionNamespace: openfaas-fn  # Default namespace for functions
 
 async: true
 
@@ -37,7 +37,7 @@ gateway:
       cpu: "50m"
 
 faasnetes:
-  image: openfaas/faas-netes:0.8.7
+  image: openfaas/faas-netes:0.9.0
   readTimeout : "60s"
   writeTimeout : "60s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
```

## 4.8.2 

**Release date:** 2019-09-20

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Add function_namespace to gateway 

### Default value changes

```diff
# No changes in this release
```

## 4.8.1 

**Release date:** 2019-09-20

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Populate namespace in faas-netes 
* Bump helm chart values 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index f83afb85..be7fdc22 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -6,7 +6,7 @@ exposeServices: true
 serviceType: NodePort
 httpProbe: true               # Setting to true will use HTTP for readiness and liveness probe on the OpenFaaS system Pods (incompatible with Istio < 1.1.5)
 rbac: true
-clusterRole: true            # Set to true to have OpenFaaS administrate multiple namespaces
+clusterRole: false            # Set to true to have OpenFaaS administrate multiple namespaces
 
 # create pod security policies for OpenFaaS control plane
 # https://kubernetes.io/docs/concepts/policy/pod-security-policy/
@@ -20,9 +20,8 @@ openfaasImagePullPolicy: "Always"
 gatewayExternal:
   annotations: {}
 
-
 gateway:
-  image: openfaas/gateway:0.17.3
+  image: openfaas/gateway:0.17.4
   readTimeout : "65s"
   writeTimeout : "65s"
   upstreamTimeout : "60s"  # Must be smaller than read/write_timeout
@@ -38,7 +37,7 @@ gateway:
       cpu: "50m"
 
 faasnetes:
-  image: openfaas/faas-netes:0.8.6
+  image: openfaas/faas-netes:0.8.7
   readTimeout : "60s"
   writeTimeout : "60s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
```

## 4.8.0 

**Release date:** 2019-09-20

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Add ClusterRole option 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 194dfee4..f83afb85 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -6,6 +6,8 @@ exposeServices: true
 serviceType: NodePort
 httpProbe: true               # Setting to true will use HTTP for readiness and liveness probe on the OpenFaaS system Pods (incompatible with Istio < 1.1.5)
 rbac: true
+clusterRole: true            # Set to true to have OpenFaaS administrate multiple namespaces
+
 # create pod security policies for OpenFaaS control plane
 # https://kubernetes.io/docs/concepts/policy/pod-security-policy/
 psp: false
@@ -18,6 +20,7 @@ openfaasImagePullPolicy: "Always"
 gatewayExternal:
   annotations: {}
 
+
 gateway:
   image: openfaas/gateway:0.17.3
   readTimeout : "65s"
```

## 4.7.4 

**Release date:** 2019-09-20

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Generate charts 
* Remove Memory Limits resource to ingressOperator 
* Add comment to describe ingressOperator in values.yaml 
* Change ingressOperator.create default to false in README.md 
* Make ingressOperator disabled by default 
* Remove rbac verification for serviceAccountName in dep template 
* Fix ingressOperator.resources default in README 
* Add ingress-operator in the openfaas Helm chart 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index e1299e8b..194dfee4 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -19,7 +19,7 @@ gatewayExternal:
   annotations: {}
 
 gateway:
-  image: openfaas/gateway:0.17.2
+  image: openfaas/gateway:0.17.3
   readTimeout : "65s"
   writeTimeout : "65s"
   upstreamTimeout : "60s"  # Must be smaller than read/write_timeout
@@ -116,6 +116,16 @@ ingress:
   tls:
     # Secrets must be manually created in the namespace.
 
+# ingressOperator (optional) – component to have specific FQDN and TLS for Functions
+# https://github.com/openfaas-incubator/ingress-operator
+ingressOperator:
+  image: openfaas/ingress-operator:0.4.0
+  replicas: 1
+  create: false
+  resources:
+    requests:
+      memory: "25Mi"
+
 # faas-idler configuration
 faasIdler:
   image: openfaas/faas-idler:0.2.1
```

## 4.7.3 

**Release date:** 2019-09-19

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Add namespace to istio destination rules host 
* use namespace values in istio dest rule host 

### Default value changes

```diff
# No changes in this release
```

## 4.7.2 

**Release date:** 2019-09-18

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump gateway to use SVG logo 
* Add PLONK reference 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 9921fd6a..e1299e8b 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -19,7 +19,7 @@ gatewayExternal:
   annotations: {}
 
 gateway:
-  image: openfaas/gateway:0.17.0
+  image: openfaas/gateway:0.17.2
   readTimeout : "65s"
   writeTimeout : "65s"
   upstreamTimeout : "60s"  # Must be smaller than read/write_timeout
```

## 4.7.1 

**Release date:** 2019-09-12

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump faas-netes to 0.8.6 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 1bfd521d..9921fd6a 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -35,7 +35,7 @@ gateway:
       cpu: "50m"
 
 faasnetes:
-  image: openfaas/faas-netes:0.8.5
+  image: openfaas/faas-netes:0.8.6
   readTimeout : "60s"
   writeTimeout : "60s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
```

## 4.7.0 

**Release date:** 2019-09-10

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Move to multi-arch versions of Prometheus/Alertmanager 
* Update images for prometheus and alertmanager 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 91965c6e..1bfd521d 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -79,14 +79,14 @@ queueWorker:
 # monitoring and auto-scaling components
 # both components
 prometheus:
-  image: prom/prometheus:v2.7.1
+  image: prom/prometheus:v2.11.0
   create: true
   resources:
     requests:
       memory: "512Mi"
 
 alertmanager:
-  image: prom/alertmanager:v0.16.1
+  image: prom/alertmanager:v0.18.0
   create: true
   resources:
     requests:
```

## 4.6.4 

**Release date:** 2019-09-10

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update armhf/arm64 helm charts 

### Default value changes

```diff
# No changes in this release
```

## 4.6.3 

**Release date:** 2019-09-05

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump charts for #492 
* Allow configuring of gateway-external svc annotations via helm chart 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 4d3930e5..91965c6e 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -15,6 +15,9 @@ basic_auth: true
 # image pull policy for openfaas components, can change to `IfNotPresent` in offline env
 openfaasImagePullPolicy: "Always"
 
+gatewayExternal:
+  annotations: {}
+
 gateway:
   image: openfaas/gateway:0.17.0
   readTimeout : "65s"
```

## 4.6.2 

**Release date:** 2019-08-26

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update armhf and arm64 images to latest available 

### Default value changes

```diff
# No changes in this release
```

## 4.6.1 

**Release date:** 2019-08-25

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Add config for basic-auth-plugin for ARM64 chart 

### Default value changes

```diff
# No changes in this release
```

## 4.6.0 

**Release date:** 2019-08-23

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump up chart versions for latest images 
* Bump image tags in helm chart for security patch 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index ba925332..4d3930e5 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -16,7 +16,7 @@ basic_auth: true
 openfaasImagePullPolicy: "Always"
 
 gateway:
-  image: openfaas/gateway:0.16.0
+  image: openfaas/gateway:0.17.0
   readTimeout : "65s"
   writeTimeout : "65s"
   upstreamTimeout : "60s"  # Must be smaller than read/write_timeout
@@ -32,7 +32,7 @@ gateway:
       cpu: "50m"
 
 faasnetes:
-  image: openfaas/faas-netes:0.8.4
+  image: openfaas/faas-netes:0.8.5
   readTimeout : "60s"
   writeTimeout : "60s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
@@ -64,7 +64,7 @@ operator:
       cpu: "50m"
 
 queueWorker:
-  image: openfaas/queue-worker:0.7.2
+  image: openfaas/queue-worker:0.8.0
   ackWait : "60s"
   replicas: 1
   gatewayInvoke: true
@@ -115,7 +115,7 @@ ingress:
 
 # faas-idler configuration
 faasIdler:
-  image: openfaas/faas-idler:0.1.9
+  image: openfaas/faas-idler:0.2.1
   replicas: 1
   create: true
   inactivityDuration: 15m               # If a function is inactive for 15 minutes, it may be scaled to zero
@@ -126,7 +126,7 @@ faasIdler:
       memory: "64Mi"
 
 basicAuthPlugin:
-  image: openfaas/basic-auth-plugin:0.1.1
+  image: openfaas/basic-auth-plugin:0.17.0
   replicas: 1
   resources:
     requests:
```

## 4.5.0 

**Release date:** 2019-08-07

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update to 0.8.4 of faas-netes and add Istio mTLS 
* Add Istio version to chart readme 
* Add Istio mTLS instructions to readme 
* Add support for Istio mTLS - enable sidecar injection on all openfaas control plane services except nats - enforce mTLS for openfaas and openfaas-fn - disable mTLS from traffic to nats - disable alertmanager gossip (not compatible with Istio) - add istio.mtls option to helm chart (disable by default) - autoscaling and asyc calls tested on GKE v1.13 and Istio v1.2.3 with Istio gateway exposing openfaas with LE TLS 
* Update armhf changes 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 4f496195..ba925332 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -32,7 +32,7 @@ gateway:
       cpu: "50m"
 
 faasnetes:
-  image: openfaas/faas-netes:0.8.3
+  image: openfaas/faas-netes:0.8.4
   readTimeout : "60s"
   writeTimeout : "60s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
@@ -140,3 +140,6 @@ tolerations: []
 affinity: {}
 
 kubernetesDNSDomain: cluster.local
+
+istio:
+  mtls: false
```

## 4.4.1 

**Release date:** 2019-08-05

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update charts to latest operator/faas-netes/faas 
* README: Fix code blocks in helm template section 
* Restrict pod security policy capabilities Allow NET_ADMIN and NET_RAW so that OpenFaaS can run under Istio or Linkerd 
* Add pod security policy to helm chart - create OF PSP with privileged, hostIPC, hostNetwork and hostPID disable - bind the PSP cluster role to all service accounts in the OF namespace - tested on EKS 1.13 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index a28deeab..4f496195 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -6,6 +6,9 @@ exposeServices: true
 serviceType: NodePort
 httpProbe: true               # Setting to true will use HTTP for readiness and liveness probe on the OpenFaaS system Pods (incompatible with Istio < 1.1.5)
 rbac: true
+# create pod security policies for OpenFaaS control plane
+# https://kubernetes.io/docs/concepts/policy/pod-security-policy/
+psp: false
 securityContext: true
 basic_auth: true
 
@@ -13,7 +16,7 @@ basic_auth: true
 openfaasImagePullPolicy: "Always"
 
 gateway:
-  image: openfaas/gateway:0.15.0
+  image: openfaas/gateway:0.16.0
   readTimeout : "65s"
   writeTimeout : "65s"
   upstreamTimeout : "60s"  # Must be smaller than read/write_timeout
@@ -29,7 +32,7 @@ gateway:
       cpu: "50m"
 
 faasnetes:
-  image: openfaas/faas-netes:0.8.1
+  image: openfaas/faas-netes:0.8.3
   readTimeout : "60s"
   writeTimeout : "60s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
@@ -50,7 +53,7 @@ faasnetes:
 
 # replaces faas-netes with openfaas-operator
 operator:
-  image: openfaas/openfaas-operator:0.9.7
+  image: openfaas/openfaas-operator:0.9.9
   create: false
   # set this to false when creating multiple releases in the same cluster
   # must be true for the first one only
```

## 4.4.0 

**Release date:** 2019-07-08

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Enable logs by bumping images 
* Fix indent 
* Correct typo in chart README 
* Update references to Istio 
* Allow deploying core services with http healthchecks 
* Move the arm value overrides into the chart 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index b117f381..a28deeab 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -4,6 +4,7 @@ async: true
 
 exposeServices: true
 serviceType: NodePort
+httpProbe: true               # Setting to true will use HTTP for readiness and liveness probe on the OpenFaaS system Pods (incompatible with Istio < 1.1.5)
 rbac: true
 securityContext: true
 basic_auth: true
@@ -12,7 +13,7 @@ basic_auth: true
 openfaasImagePullPolicy: "Always"
 
 gateway:
-  image: openfaas/gateway:0.14.4
+  image: openfaas/gateway:0.15.0
   readTimeout : "65s"
   writeTimeout : "65s"
   upstreamTimeout : "60s"  # Must be smaller than read/write_timeout
@@ -28,7 +29,7 @@ gateway:
       cpu: "50m"
 
 faasnetes:
-  image: openfaas/faas-netes:0.8.0
+  image: openfaas/faas-netes:0.8.1
   readTimeout : "60s"
   writeTimeout : "60s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
```

## 4.3.5 

**Release date:** 2019-07-03

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Publish charts for 4.3.5 
* Rebase to upstream 
* Set intialDelay to zero 
* Align liveness/readiness probe values 
* Add notes on tuning cold-starts 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index e80e4268..b117f381 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -22,6 +22,10 @@ gateway:
   nodePort: 31112
   maxIdleConns: 1024
   maxIdleConnsPerHost: 1024
+  resources:
+    requests:
+      memory: "120Mi"
+      cpu: "50m"
 
 faasnetes:
   image: openfaas/faas-netes:0.8.0
@@ -32,12 +36,16 @@ faasnetes:
   setNonRootUser: false
   readinessProbe:
     initialDelaySeconds: 2
-    timeoutSeconds: 2           # Tuned-in to run checks early and quickly to support fast cold-start from zero replicas
+    timeoutSeconds: 1           # Tuned-in to run checks early and quickly to support fast cold-start from zero replicas
     periodSeconds: 2            # Reduce to 1 for a faster cold-start, increase higher for lower-CPU usage
   livenessProbe:
     initialDelaySeconds: 2
-    timeoutSeconds: 2
-    periodSeconds: 10           # Reduce to 1 for a faster cold-start, increase higher for lower-CPU usage
+    timeoutSeconds: 1
+    periodSeconds: 2           # Reduce to 1 for a faster cold-start, increase higher for lower-CPU usage
+  resources:
+    requests:
+      memory: "120Mi"
+      cpu: "50m"
 
 # replaces faas-netes with openfaas-operator
 operator:
@@ -46,27 +54,46 @@ operator:
   # set this to false when creating multiple releases in the same cluster
   # must be true for the first one only
   createCRD: true
+  resources:
+    requests:
+      memory: "120Mi"
+      cpu: "50m"
 
 queueWorker:
   image: openfaas/queue-worker:0.7.2
   ackWait : "60s"
   replicas: 1
   gatewayInvoke: true
+  resources:
+    requests:
+      memory: "120Mi"
+      cpu: "50m"
 
 # monitoring and auto-scaling components
 # both components
 prometheus:
   image: prom/prometheus:v2.7.1
   create: true
+  resources:
+    requests:
+      memory: "512Mi"
 
 alertmanager:
   image: prom/alertmanager:v0.16.1
   create: true
+  resources:
+    requests:
+      memory: "25Mi"
+    limits:
+      memory: "50Mi"
 
 # async provider
 nats:
   image: nats-streaming:0.11.2
   enableMonitoring: false
+  resources:
+    requests:
+      memory: "120Mi"
 
 # ingress configuration
 ingress:
@@ -89,11 +116,18 @@ faasIdler:
   create: true
   inactivityDuration: 15m               # If a function is inactive for 15 minutes, it may be scaled to zero
   reconcileInterval: 1m                 # The interval between each attempt to scale functions to zero
-  dryRun: true                          # Set to true to enable the idler to apply changes and scale to zero
+  dryRun: true                          # Set to false to enable the idler to apply changes and scale to zero
+  resources:
+    requests:
+      memory: "64Mi"
 
 basicAuthPlugin:
   image: openfaas/basic-auth-plugin:0.1.1
   replicas: 1
+  resources:
+    requests:
+      memory: "50Mi"
+      cpu: "20m"
 
 nodeSelector: {}
 
```

## 4.3.4 

**Release date:** 2019-07-03

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Enable HTTP probe by default 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index ffc85df2..e80e4268 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -1,4 +1,4 @@
-functionNamespace:
+functionNamespace: openfaas-fn
 
 async: true
 
@@ -28,16 +28,16 @@ faasnetes:
   readTimeout : "60s"
   writeTimeout : "60s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
-  httpProbe: false              # Setting to true will use HTTP for readiness and liveness (incompatible with Istio < 1.1.5)
+  httpProbe: true               # Setting to true will use HTTP for readiness and liveness probe on Pods (incompatible with Istio < 1.1.5)
   setNonRootUser: false
   readinessProbe:
-    initialDelaySeconds: 0
-    timeoutSeconds: 1           # Tuned-in to run checks early and quickly to support fast cold-start from zero replicas
-    periodSeconds: 1
+    initialDelaySeconds: 2
+    timeoutSeconds: 2           # Tuned-in to run checks early and quickly to support fast cold-start from zero replicas
+    periodSeconds: 2            # Reduce to 1 for a faster cold-start, increase higher for lower-CPU usage
   livenessProbe:
-    initialDelaySeconds: 3
-    timeoutSeconds: 1
-    periodSeconds: 10
+    initialDelaySeconds: 2
+    timeoutSeconds: 2
+    periodSeconds: 10           # Reduce to 1 for a faster cold-start, increase higher for lower-CPU usage
 
 # replaces faas-netes with openfaas-operator
 operator:
```

## 4.3.3 

**Release date:** 2019-06-25

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update charts for armhf/arm64 to add basic-auth-plugin 

### Default value changes

```diff
# No changes in this release
```

## 4.3.2 

**Release date:** 2019-06-25

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update gateway/faas-netes versions 
* Remove kail dependency 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 7ff8608d..ffc85df2 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -12,7 +12,7 @@ basic_auth: true
 openfaasImagePullPolicy: "Always"
 
 gateway:
-  image: openfaas/gateway:0.13.9
+  image: openfaas/gateway:0.14.4
   readTimeout : "65s"
   writeTimeout : "65s"
   upstreamTimeout : "60s"  # Must be smaller than read/write_timeout
@@ -24,7 +24,7 @@ gateway:
   maxIdleConnsPerHost: 1024
 
 faasnetes:
-  image: openfaas/faas-netes:0.7.8
+  image: openfaas/faas-netes:0.8.0
   readTimeout : "60s"
   writeTimeout : "60s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
```

## 4.3.1 

**Release date:** 2019-06-21

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump latest changes from @stefanprodan 
* Remove comment about operator not supporting httpProbe 
* Helm chart: Bump openfaas-operator to 0.9.7 Reuse faas-netes env vars for the operator 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 1936cf88..7ff8608d 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -41,7 +41,7 @@ faasnetes:
 
 # replaces faas-netes with openfaas-operator
 operator:
-  image: openfaas/openfaas-operator:0.9.4
+  image: openfaas/openfaas-operator:0.9.7
   create: false
   # set this to false when creating multiple releases in the same cluster
   # must be true for the first one only
```

## 4.3.0 

**Release date:** 2019-06-15

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump openfaas chart version to 4.3.0 
* Update gateway to 0.13.9 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 7dd76ea8..1936cf88 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -12,7 +12,7 @@ basic_auth: true
 openfaasImagePullPolicy: "Always"
 
 gateway:
-  image: openfaas/gateway:0.13.7
+  image: openfaas/gateway:0.13.9
   readTimeout : "65s"
   writeTimeout : "65s"
   upstreamTimeout : "60s"  # Must be smaller than read/write_timeout
```

## 4.2.0 

**Release date:** 2019-06-09

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Enable basic-auth plugin e2e 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 29fb2240..7dd76ea8 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -8,6 +8,21 @@ rbac: true
 securityContext: true
 basic_auth: true
 
+# image pull policy for openfaas components, can change to `IfNotPresent` in offline env
+openfaasImagePullPolicy: "Always"
+
+gateway:
+  image: openfaas/gateway:0.13.7
+  readTimeout : "65s"
+  writeTimeout : "65s"
+  upstreamTimeout : "60s"  # Must be smaller than read/write_timeout
+  replicas: 1
+  scaleFromZero: true
+  # change the port when creating multiple releases in the same baremetal cluster
+  nodePort: 31112
+  maxIdleConns: 1024
+  maxIdleConnsPerHost: 1024
+
 faasnetes:
   image: openfaas/faas-netes:0.7.8
   readTimeout : "60s"
@@ -24,27 +39,6 @@ faasnetes:
     timeoutSeconds: 1
     periodSeconds: 10
 
-gateway:
-  image: openfaas/gateway:0.13.6
-  readTimeout : "65s"
-  writeTimeout : "65s"
-  upstreamTimeout : "60s"  # Must be smaller than read/write_timeout
-  replicas: 1
-  scaleFromZero: true
-  # change the port when creating multiple releases in the same baremetal cluster
-  nodePort: 31112
-  maxIdleConns: 1024
-  maxIdleConnsPerHost: 1024
-
-queueWorker:
-  image: openfaas/queue-worker:0.7.2
-  ackWait : "60s"
-  replicas: 1
-  gatewayInvoke: true
-
-# image pull policy for openfaas components, can change to `IfNotPresent` in offline env
-openfaasImagePullPolicy: "Always"
-
 # replaces faas-netes with openfaas-operator
 operator:
   image: openfaas/openfaas-operator:0.9.4
@@ -53,6 +47,12 @@ operator:
   # must be true for the first one only
   createCRD: true
 
+queueWorker:
+  image: openfaas/queue-worker:0.7.2
+  ackWait : "60s"
+  replicas: 1
+  gatewayInvoke: true
+
 # monitoring and auto-scaling components
 # both components
 prometheus:
@@ -84,16 +84,16 @@ ingress:
 
 # faas-idler configuration
 faasIdler:
-  create: true
-  replicas: 1
   image: openfaas/faas-idler:0.1.9
+  replicas: 1
+  create: true
   inactivityDuration: 15m               # If a function is inactive for 15 minutes, it may be scaled to zero
   reconcileInterval: 1m                 # The interval between each attempt to scale functions to zero
   dryRun: true                          # Set to true to enable the idler to apply changes and scale to zero
 
 basicAuthPlugin:
+  image: openfaas/basic-auth-plugin:0.1.1
   replicas: 1
-  image: openfaas/basic-auth-plugin:0.1.0
 
 nodeSelector: {}
 
```

## 4.1.1 

**Release date:** 2019-06-08

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update charts to remove comments 
* Remove Kubernetes 1.6 notes 

### Default value changes

```diff
# No changes in this release
```

## 4.1.0 

**Release date:** 2019-06-08

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Helm chart: bump faas-netes version to 0.7.8 Add note about Istio version and HTTP probe compatibility 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index e119129f..29fb2240 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -9,11 +9,11 @@ securityContext: true
 basic_auth: true
 
 faasnetes:
-  image: openfaas/faas-netes:0.7.5
+  image: openfaas/faas-netes:0.7.8
   readTimeout : "60s"
   writeTimeout : "60s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
-  httpProbe: false              # Setting to true will use a lock file for readiness and liveness (incompatible with Istio)
+  httpProbe: false              # Setting to true will use HTTP for readiness and liveness (incompatible with Istio < 1.1.5)
   setNonRootUser: false
   readinessProbe:
     initialDelaySeconds: 0
```

## 4.0.0 

**Release date:** 2019-06-06

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Move to k8s.io/api/apps from extensions 
* Add k8s.io/api/apps to faas-netes RBAC Tested on Kind with Helm 
* Add security context to faas-netes container inside the gateway pod. Fix issue #438 
* Deploy basic-auth-plugin for faas-netes / operator 
* Update Helm Chart README 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 5a47c2aa..e119129f 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -25,7 +25,7 @@ faasnetes:
     periodSeconds: 10
 
 gateway:
-  image: openfaas/gateway:0.13.0
+  image: openfaas/gateway:0.13.6
   readTimeout : "65s"
   writeTimeout : "65s"
   upstreamTimeout : "60s"  # Must be smaller than read/write_timeout
@@ -91,6 +91,10 @@ faasIdler:
   reconcileInterval: 1m                 # The interval between each attempt to scale functions to zero
   dryRun: true                          # Set to true to enable the idler to apply changes and scale to zero
 
+basicAuthPlugin:
+  replicas: 1
+  image: openfaas/basic-auth-plugin:0.1.0
+
 nodeSelector: {}
 
 tolerations: []
```

## 3.3.1 

**Release date:** 2019-05-07

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump charts for gateway_invoke feature 
* Use production release of image 
* Add gateway_invoke to helm chart 
* Update note on probes 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index c069af96..5a47c2aa 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -37,9 +37,10 @@ gateway:
   maxIdleConnsPerHost: 1024
 
 queueWorker:
-  image: openfaas/queue-worker:0.7.1
+  image: openfaas/queue-worker:0.7.2
   ackWait : "60s"
   replicas: 1
+  gatewayInvoke: true
 
 # image pull policy for openfaas components, can change to `IfNotPresent` in offline env
 openfaasImagePullPolicy: "Always"
```

## 3.3.0 

**Release date:** 2019-04-26

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update chart to 0.3.3 
* Add CPU requests 
* Set initial requests for RAM 
* Add new contrib script for generating yaml from helm 
* Move the faas-netes version to 0.7.5 
* Add support for setting the functions container user id 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 62d64e5a..c069af96 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -6,14 +6,15 @@ exposeServices: true
 serviceType: NodePort
 rbac: true
 securityContext: true
-basic_auth: false
+basic_auth: true
 
 faasnetes:
-  image: openfaas/faas-netes:0.7.3
+  image: openfaas/faas-netes:0.7.5
   readTimeout : "60s"
   writeTimeout : "60s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
   httpProbe: false              # Setting to true will use a lock file for readiness and liveness (incompatible with Istio)
+  setNonRootUser: false
   readinessProbe:
     initialDelaySeconds: 0
     timeoutSeconds: 1           # Tuned-in to run checks early and quickly to support fast cold-start from zero replicas
```

## 3.2.3 

**Release date:** 2019-04-23

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump faas-netes/operator 
* Add note to helm chart about ingress 
* Prepare for httpProbes 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index cd8f1c34..62d64e5a 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -9,7 +9,7 @@ securityContext: true
 basic_auth: false
 
 faasnetes:
-  image: openfaas/faas-netes:0.7.2
+  image: openfaas/faas-netes:0.7.3
   readTimeout : "60s"
   writeTimeout : "60s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
@@ -45,7 +45,7 @@ openfaasImagePullPolicy: "Always"
 
 # replaces faas-netes with openfaas-operator
 operator:
-  image: openfaas/openfaas-operator:0.9.3
+  image: openfaas/openfaas-operator:0.9.4
   create: false
   # set this to false when creating multiple releases in the same cluster
   # must be true for the first one only
@@ -71,7 +71,7 @@ ingress:
   enabled: false
   # Used to create Ingress record (should be used with exposeServices: false).
   hosts:
-    - host: gateway.openfaas.local
+    - host: gateway.openfaas.local  # Replace with gateway.example.com if public-facing
       serviceName: gateway
       servicePort: 8080
       path: /
```

## 3.2.2 

**Release date:** 2019-04-15

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump faas-netes to include serviceAccount feature 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index dcdbd192..cd8f1c34 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -9,7 +9,7 @@ securityContext: true
 basic_auth: false
 
 faasnetes:
-  image: openfaas/faas-netes:0.7.1
+  image: openfaas/faas-netes:0.7.2
   readTimeout : "60s"
   writeTimeout : "60s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
@@ -24,7 +24,7 @@ faasnetes:
     periodSeconds: 10
 
 gateway:
-  image: openfaas/gateway:0.12.0
+  image: openfaas/gateway:0.13.0
   readTimeout : "65s"
   writeTimeout : "65s"
   upstreamTimeout : "60s"  # Must be smaller than read/write_timeout
@@ -45,7 +45,7 @@ openfaasImagePullPolicy: "Always"
 
 # replaces faas-netes with openfaas-operator
 operator:
-  image: openfaas/openfaas-operator:0.9.2
+  image: openfaas/openfaas-operator:0.9.3
   create: false
   # set this to false when creating multiple releases in the same cluster
   # must be true for the first one only
```

## 3.2.1 

**Release date:** 2019-04-15

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Clarify zero scale requirements 

### Default value changes

```diff
# No changes in this release
```

## 3.2.0 

**Release date:** 2019-04-08

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Extend faasIdler defaults 
* Document create flags 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 46572441..dcdbd192 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -85,8 +85,8 @@ faasIdler:
   create: true
   replicas: 1
   image: openfaas/faas-idler:0.1.9
-  inactivityDuration: 5m
-  reconcileInterval: 30s
+  inactivityDuration: 15m               # If a function is inactive for 15 minutes, it may be scaled to zero
+  reconcileInterval: 1m                 # The interval between each attempt to scale functions to zero
   dryRun: true                          # Set to true to enable the idler to apply changes and scale to zero
 
 nodeSelector: {}
```

## 3.1.0 

**Release date:** 2019-04-06

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Add create toggle for components 
* Remove double quoting from README 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 822ff771..46572441 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -52,10 +52,14 @@ operator:
   createCRD: true
 
 # monitoring and auto-scaling components
+# both components
 prometheus:
   image: prom/prometheus:v2.7.1
+  create: true
+
 alertmanager:
   image: prom/alertmanager:v0.16.1
+  create: true
 
 # async provider
 nats:
@@ -78,6 +82,7 @@ ingress:
 
 # faas-idler configuration
 faasIdler:
+  create: true
   replicas: 1
   image: openfaas/faas-idler:0.1.9
   inactivityDuration: 5m
```

## 3.0.0 

**Release date:** 2019-04-06

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump OF chart to 3.0.0 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index d9f6c0a0..822ff771 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -9,7 +9,7 @@ securityContext: true
 basic_auth: false
 
 faasnetes:
-  image: openfaas/faas-netes:0.7.0
+  image: openfaas/faas-netes:0.7.1
   readTimeout : "60s"
   writeTimeout : "60s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
@@ -24,7 +24,7 @@ faasnetes:
     periodSeconds: 10
 
 gateway:
-  image: openfaas/gateway:0.11.1
+  image: openfaas/gateway:0.12.0
   readTimeout : "65s"
   writeTimeout : "65s"
   upstreamTimeout : "60s"  # Must be smaller than read/write_timeout
```

## 2.1.4 

**Release date:** 2019-04-03

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Fix pull policy for faas-netes 

### Default value changes

```diff
# No changes in this release
```

## 2.1.3 

**Release date:** 2019-03-27

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump chart to 2.1.3 
* Add emptyDir mount to Prometheus 

### Default value changes

```diff
# No changes in this release
```

## 2.1.2 

**Release date:** 2019-03-08

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump openfaas-operator to 0.9.2 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index faba5560..d9f6c0a0 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -45,7 +45,7 @@ openfaasImagePullPolicy: "Always"
 
 # replaces faas-netes with openfaas-operator
 operator:
-  image: openfaas/openfaas-operator:0.9.1
+  image: openfaas/openfaas-operator:0.9.2
   create: false
   # set this to false when creating multiple releases in the same cluster
   # must be true for the first one only
```

## 2.1.1 

**Release date:** 2019-03-05

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Rename faas-netesd to faas-netes 
* Change faasnetesd to faasnetes to keep consistency 
* Bump queue-worker version to 0.7.1 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 71698dd0..faba5560 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -8,7 +8,7 @@ rbac: true
 securityContext: true
 basic_auth: false
 
-faasnetesd:
+faasnetes:
   image: openfaas/faas-netes:0.7.0
   readTimeout : "60s"
   writeTimeout : "60s"
@@ -36,7 +36,7 @@ gateway:
   maxIdleConnsPerHost: 1024
 
 queueWorker:
-  image: openfaas/queue-worker:0.7.0
+  image: openfaas/queue-worker:0.7.1
   ackWait : "60s"
   replicas: 1
 
```

## 2.1.0 

**Release date:** 2019-02-28

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump charts for queue-worker 

### Default value changes

```diff
# No changes in this release
```

## 2.0.0 

**Release date:** 2019-02-18

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update helm chart to latest alertmanager 
* Ugrade altermanager to 0.16.1 
* Remove alert value from the alert labels 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index a040a829..71698dd0 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -55,7 +55,7 @@ operator:
 prometheus:
   image: prom/prometheus:v2.7.1
 alertmanager:
-  image: prom/alertmanager:v0.15.0
+  image: prom/alertmanager:v0.16.1
 
 # async provider
 nats:
```

## 1.9.1 

**Release date:** 2019-02-15

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump charts for Prometheus Pod SD 
* Enable Istio sidecar for faas-idler 
* Upgrade Prometheus to v2.7.1 - tested on GKE 1.11 and Istio 1.0.3 
* Disable Istio sidecar except for Gateway and QueueWorker - fix for OF async: NATS protocol is not supported by Envoy - tested on GKE with Istio 1.0.3 
* Enable Gateway Prometheus scrapping - restrict scraping to port 8080 
* Use Kubernetes discovery for Gateway scrapping - add Prometheus Kubernetes service account, role and RBAC - enable pod scraping for OpenFaaS main namespace 
* Upgrade Prometheus to v2.6.1 - fix for kubernetes_sd_configs namespace restriction 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 199d086d..a040a829 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -53,7 +53,7 @@ operator:
 
 # monitoring and auto-scaling components
 prometheus:
-  image: prom/prometheus:v2.3.1
+  image: prom/prometheus:v2.7.1
 alertmanager:
   image: prom/alertmanager:v0.15.0
 
```

## 1.8.1 

**Release date:** 2019-02-12

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump gateway to 0.11.1 and queue-worker to 0.7.0 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 3369e88d..199d086d 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -24,7 +24,7 @@ faasnetesd:
     periodSeconds: 10
 
 gateway:
-  image: openfaas/gateway:0.11.0
+  image: openfaas/gateway:0.11.1
   readTimeout : "65s"
   writeTimeout : "65s"
   upstreamTimeout : "60s"  # Must be smaller than read/write_timeout
@@ -36,7 +36,7 @@ gateway:
   maxIdleConnsPerHost: 1024
 
 queueWorker:
-  image: openfaas/queue-worker:0.5.4
+  image: openfaas/queue-worker:0.7.0
   ackWait : "60s"
   replicas: 1
 
```

## 1.8.0 

**Release date:** 2019-02-04

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump gateway to 0.11.0 
* Update README typo 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 71c4f9ae..3369e88d 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -24,7 +24,7 @@ faasnetesd:
     periodSeconds: 10
 
 gateway:
-  image: openfaas/gateway:0.10.2
+  image: openfaas/gateway:0.11.0
   readTimeout : "65s"
   writeTimeout : "65s"
   upstreamTimeout : "60s"  # Must be smaller than read/write_timeout
```

## 1.7.1 

**Release date:** 2019-01-29

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump chart for load-testing values 
* Add overrides for max_idle_conns_* 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 20103df5..71c4f9ae 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -32,6 +32,8 @@ gateway:
   scaleFromZero: true
   # change the port when creating multiple releases in the same baremetal cluster
   nodePort: 31112
+  maxIdleConns: 1024
+  maxIdleConnsPerHost: 1024
 
 queueWorker:
   image: openfaas/queue-worker:0.5.4
```

## 1.7.0 

**Release date:** 2019-01-29

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump chart to include AlertManager auth 
* Bump of gateway version to 0.10.2 
* Update anchor link 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 9d3d1a2b..20103df5 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -24,7 +24,7 @@ faasnetesd:
     periodSeconds: 10
 
 gateway:
-  image: openfaas/gateway:0.9.14
+  image: openfaas/gateway:0.10.2
   readTimeout : "65s"
   writeTimeout : "65s"
   upstreamTimeout : "60s"  # Must be smaller than read/write_timeout
```

## 1.6.3 

**Release date:** 2019-01-22

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump chart version to 1.6.3 
* Add basic auth to alertmanager config 
* Fixes #350 by adding missing namespace to ingress resource template 

### Default value changes

```diff
# No changes in this release
```

## 1.6.2 

**Release date:** 2019-01-11

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update to latest OpenFaaS Operator version 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 15268c2e..9d3d1a2b 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -43,7 +43,7 @@ openfaasImagePullPolicy: "Always"
 
 # replaces faas-netes with openfaas-operator
 operator:
-  image: openfaas/openfaas-operator:0.9.0
+  image: openfaas/openfaas-operator:0.9.1
   create: false
   # set this to false when creating multiple releases in the same cluster
   # must be true for the first one only
```

## 1.6.1 

**Release date:** 2019-01-10

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump charts for openfaas-operator, faas-netes and GW 
* Update operator to v0.9.0 - change operator RBAC from secrets readonly to full access (needed by the new secrets management API) 
* Extend RBAC permissions to allow secret crud 
* Add NATS address env var to worker - prep for external NATS cluster support 
* Fix worker basic auth env var - the basic auth was hardcoded 
* Fix faas-idler node selector and toleration indentation 
* Disable Prometheus scraping for components without /metrics - tested on GKE with Weave Cloud 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 21db4b03..15268c2e 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -9,7 +9,7 @@ securityContext: true
 basic_auth: false
 
 faasnetesd:
-  image: openfaas/faas-netes:0.6.4
+  image: openfaas/faas-netes:0.7.0
   readTimeout : "60s"
   writeTimeout : "60s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
@@ -24,7 +24,7 @@ faasnetesd:
     periodSeconds: 10
 
 gateway:
-  image: openfaas/gateway:0.9.11
+  image: openfaas/gateway:0.9.14
   readTimeout : "65s"
   writeTimeout : "65s"
   upstreamTimeout : "60s"  # Must be smaller than read/write_timeout
@@ -43,7 +43,7 @@ openfaasImagePullPolicy: "Always"
 
 # replaces faas-netes with openfaas-operator
 operator:
-  image: openfaas/openfaas-operator:0.8.12
+  image: openfaas/openfaas-operator:0.9.0
   create: false
   # set this to false when creating multiple releases in the same cluster
   # must be true for the first one only
```

## 1.6.0 

**Release date:** 2018-12-28

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Replace cluster role with namespaced role 
* Update chart instructions 

### Default value changes

```diff
# No changes in this release
```

## 1.5.0 

**Release date:** 2018-12-03

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump charts for operator/faas-netes 
* Bump operator and faas-netes 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 639044bb..21db4b03 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -9,14 +9,14 @@ securityContext: true
 basic_auth: false
 
 faasnetesd:
-  image: openfaas/faas-netes:0.6.3
+  image: openfaas/faas-netes:0.6.4
   readTimeout : "60s"
   writeTimeout : "60s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
   httpProbe: false              # Setting to true will use a lock file for readiness and liveness (incompatible with Istio)
   readinessProbe:
     initialDelaySeconds: 0
-    timeoutSeconds: 1
+    timeoutSeconds: 1           # Tuned-in to run checks early and quickly to support fast cold-start from zero replicas
     periodSeconds: 1
   livenessProbe:
     initialDelaySeconds: 3
@@ -43,7 +43,7 @@ openfaasImagePullPolicy: "Always"
 
 # replaces faas-netes with openfaas-operator
 operator:
-  image: openfaas/openfaas-operator:0.8.10
+  image: openfaas/openfaas-operator:0.8.12
   create: false
   # set this to false when creating multiple releases in the same cluster
   # must be true for the first one only
@@ -80,7 +80,7 @@ faasIdler:
   image: openfaas/faas-idler:0.1.9
   inactivityDuration: 5m
   reconcileInterval: 30s
-  dryRun: true
+  dryRun: true                          # Set to true to enable the idler to apply changes and scale to zero
 
 nodeSelector: {}
 
```

## 1.4.4 

**Release date:** 2018-11-27

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump new helm chart version to 1.4.4 
* Add the nats monitoring port number to the readme docs 
* Fix copypasta in the chart readme for nats monitoring 
* Add nats.enableMonitoring chart configuration 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 9fea1bed..639044bb 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -58,6 +58,7 @@ alertmanager:
 # async provider
 nats:
   image: nats-streaming:0.11.2
+  enableMonitoring: false
 
 # ingress configuration
 ingress:
```

## 1.4.3 

**Release date:** 2018-11-22

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump charts for #327 
* Set operator image pull policy using values.yaml - tested on Docker for Mac by installing the chart with internet, going offline, uninstalling and then installing again with IfNotPresent - fix #322 
* Update operator to v0.8.10 - fix validation for cpu and memory limits 
* Bump gw to 0.9.11 
* Bump yaml and charts to use 0.9.10 gateway 
* Update note on helm chart nodeports 
* Disable nats monitoring port by default 
* Bump nat-streaming to 0.11.2 
* Bump nat-streaming to 0.11.0 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index e6627cca..9fea1bed 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -24,7 +24,7 @@ faasnetesd:
     periodSeconds: 10
 
 gateway:
-  image: openfaas/gateway:0.9.7
+  image: openfaas/gateway:0.9.11
   readTimeout : "65s"
   writeTimeout : "65s"
   upstreamTimeout : "60s"  # Must be smaller than read/write_timeout
@@ -43,7 +43,7 @@ openfaasImagePullPolicy: "Always"
 
 # replaces faas-netes with openfaas-operator
 operator:
-  image: openfaas/openfaas-operator:0.8.9
+  image: openfaas/openfaas-operator:0.8.10
   create: false
   # set this to false when creating multiple releases in the same cluster
   # must be true for the first one only
@@ -57,7 +57,7 @@ alertmanager:
 
 # async provider
 nats:
-  image: nats-streaming:0.6.0
+  image: nats-streaming:0.11.2
 
 # ingress configuration
 ingress:
```

## 1.3.3 

**Release date:** 2018-10-21

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump chart to include gw 0.9.7 
* Bump gw version to 0.9.7 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index e99dab6c..e6627cca 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -24,7 +24,7 @@ faasnetesd:
     periodSeconds: 10
 
 gateway:
-  image: openfaas/gateway:0.9.6
+  image: openfaas/gateway:0.9.7
   readTimeout : "65s"
   writeTimeout : "65s"
   upstreamTimeout : "60s"  # Must be smaller than read/write_timeout
```

## 1.3.2 

**Release date:** 2018-10-19

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump queue worker to 0.5.4 
* Fix fass idler deployment metadata 
* Add tiller-less install instructions 
* Update defaults in README 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 4719098a..e99dab6c 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -34,7 +34,7 @@ gateway:
   nodePort: 31112
 
 queueWorker:
-  image: openfaas/queue-worker:0.5.2
+  image: openfaas/queue-worker:0.5.4
   ackWait : "60s"
   replicas: 1
 
```

## 1.3.1 

**Release date:** 2018-09-29

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump gateway, faas-netes and timeouts 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 145cee7c..4719098a 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -9,9 +9,9 @@ securityContext: true
 basic_auth: false
 
 faasnetesd:
-  image: openfaas/faas-netes:0.6.2
-  readTimeout : "20s"
-  writeTimeout : "20s"
+  image: openfaas/faas-netes:0.6.3
+  readTimeout : "60s"
+  writeTimeout : "60s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
   httpProbe: false              # Setting to true will use a lock file for readiness and liveness (incompatible with Istio)
   readinessProbe:
@@ -24,10 +24,10 @@ faasnetesd:
     periodSeconds: 10
 
 gateway:
-  image: openfaas/gateway:0.9.5
-  readTimeout : "20s"
-  writeTimeout : "20s"
-  upstreamTimeout : "15s"  # Must be smaller than read/write_timeout
+  image: openfaas/gateway:0.9.6
+  readTimeout : "65s"
+  writeTimeout : "65s"
+  upstreamTimeout : "60s"  # Must be smaller than read/write_timeout
   replicas: 1
   scaleFromZero: true
   # change the port when creating multiple releases in the same baremetal cluster
@@ -35,7 +35,7 @@ gateway:
 
 queueWorker:
   image: openfaas/queue-worker:0.5.2
-  ackWait : "30s"
+  ackWait : "60s"
   replicas: 1
 
 # image pull policy for openfaas components, can change to `IfNotPresent` in offline env
```

## 1.3.0 

**Release date:** 2018-09-22

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump operator version to 0.8.9 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index d2431881..145cee7c 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -43,7 +43,7 @@ openfaasImagePullPolicy: "Always"
 
 # replaces faas-netes with openfaas-operator
 operator:
-  image: openfaas/openfaas-operator:0.8.7
+  image: openfaas/openfaas-operator:0.8.9
   create: false
   # set this to false when creating multiple releases in the same cluster
   # must be true for the first one only
```

## 1.2.9 

**Release date:** 2018-09-21

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Revert operator version 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 12263acb..d2431881 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -43,7 +43,7 @@ openfaasImagePullPolicy: "Always"
 
 # replaces faas-netes with openfaas-operator
 operator:
-  image: openfaas/openfaas-operator:0.8.8
+  image: openfaas/openfaas-operator:0.8.7
   create: false
   # set this to false when creating multiple releases in the same cluster
   # must be true for the first one only
```

## 1.2.8 

**Release date:** 2018-09-20

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump idler to 0.1.9 
* Mention scaling to zero in the chart README 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index a75464e8..12263acb 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -76,7 +76,7 @@ ingress:
 # faas-idler configuration
 faasIdler:
   replicas: 1
-  image: openfaas/faas-idler:0.1.7
+  image: openfaas/faas-idler:0.1.9
   inactivityDuration: 5m
   reconcileInterval: 30s
   dryRun: true
```

## 1.2.7 

**Release date:** 2018-09-20

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump chart version 
* Remove duplicate if blocks in queue worker chart 
* Add basic auth for queue worker 
* Added faas-idler to the helm chart 
* Update gateway to v0.9.5 - regenerate Helm chart v1.2.6 with the new gateway 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index aba159a5..a75464e8 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -24,12 +24,12 @@ faasnetesd:
     periodSeconds: 10
 
 gateway:
-  image: openfaas/gateway:0.9.4
+  image: openfaas/gateway:0.9.5
   readTimeout : "20s"
   writeTimeout : "20s"
   upstreamTimeout : "15s"  # Must be smaller than read/write_timeout
   replicas: 1
-  scaleFromZero: false
+  scaleFromZero: true
   # change the port when creating multiple releases in the same baremetal cluster
   nodePort: 31112
 
@@ -73,6 +73,14 @@ ingress:
   tls:
     # Secrets must be manually created in the namespace.
 
+# faas-idler configuration
+faasIdler:
+  replicas: 1
+  image: openfaas/faas-idler:0.1.7
+  inactivityDuration: 5m
+  reconcileInterval: 30s
+  dryRun: true
+
 nodeSelector: {}
 
 tolerations: []
```

## 1.2.6 

**Release date:** 2018-09-19

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump core services to latest versions - gateway 0.9.4 (fix issue with direct_functions and path behaviour) - faas-netes 0.6.2 (expand HTTP methods for proxying through provider) - openfaas-operator 0.8.8 (fixes issue with reconciliation loop) 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 3f616d71..aba159a5 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -9,7 +9,7 @@ securityContext: true
 basic_auth: false
 
 faasnetesd:
-  image: openfaas/faas-netes:0.6.1
+  image: openfaas/faas-netes:0.6.2
   readTimeout : "20s"
   writeTimeout : "20s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
@@ -24,7 +24,7 @@ faasnetesd:
     periodSeconds: 10
 
 gateway:
-  image: openfaas/gateway:0.9.2
+  image: openfaas/gateway:0.9.4
   readTimeout : "20s"
   writeTimeout : "20s"
   upstreamTimeout : "15s"  # Must be smaller than read/write_timeout
@@ -43,7 +43,7 @@ openfaasImagePullPolicy: "Always"
 
 # replaces faas-netes with openfaas-operator
 operator:
-  image: openfaas/openfaas-operator:0.8.6
+  image: openfaas/openfaas-operator:0.8.8
   create: false
   # set this to false when creating multiple releases in the same cluster
   # must be true for the first one only
```

## 1.2.5 

**Release date:** 2018-09-15

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump helm to fix #298 #297 
* Set direct functions env vars when asyc is disabled - fix #298 

### Default value changes

```diff
# No changes in this release
```

## 1.2.4 

**Release date:** 2018-09-12

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump Helm chart version - Istio compatibility #279 
* Mention Istio mTLS and wget usage 
* Remove duplicate login command from readme 
* Update Queue Worker to v0.5.2 - tested on GKE with Istio 1.0.2 
* Update faas-netes and operator to latest version 
* Add timeout to wget health checks 
* Resolve readme merge conflict 
* Make the liveness and readiness probes Istio mTLS compatible - use wget instead of HTTP calls for health checks 
* Make the Queue Worker Istio compatible - remove the last dot from the domain name so that Envoy can resolve the destination 
* Make the Gateway Istio compatible - remove the last dot from the domain name so that Envoy can resolve the destination - use a named port for the Gateway service to enable Istio L7 HTTP/S routing and tracing 
* Fix CRD validation - remove type array since it breaks the OpenFaaS store functions 
* Move basic auth setup in the main install section - make the default install instruction use auth 
* Add openAPIV3Schema validation to CRD - validate limits and requests - validate function name as Kubernetes deployment name - validate secrets and constraints as string array 
* Move Gateway security context from pod to container level - the securityContext doesn't need to be disabled when using Istio 
* Bump Gateway version to 0.9.2 
* Add TLS / SSL link 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 57cd0956..3f616d71 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -9,11 +9,11 @@ securityContext: true
 basic_auth: false
 
 faasnetesd:
-  image: openfaas/faas-netes:0.6.0
+  image: openfaas/faas-netes:0.6.1
   readTimeout : "20s"
   writeTimeout : "20s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
-  httpProbe: false              # Setting to true will use a lock file for readiness and liveness
+  httpProbe: false              # Setting to true will use a lock file for readiness and liveness (incompatible with Istio)
   readinessProbe:
     initialDelaySeconds: 0
     timeoutSeconds: 1
@@ -24,7 +24,7 @@ faasnetesd:
     periodSeconds: 10
 
 gateway:
-  image: openfaas/gateway:0.9.1
+  image: openfaas/gateway:0.9.2
   readTimeout : "20s"
   writeTimeout : "20s"
   upstreamTimeout : "15s"  # Must be smaller than read/write_timeout
@@ -34,7 +34,7 @@ gateway:
   nodePort: 31112
 
 queueWorker:
-  image: openfaas/queue-worker:0.4.9
+  image: openfaas/queue-worker:0.5.2
   ackWait : "30s"
   replicas: 1
 
@@ -43,7 +43,7 @@ openfaasImagePullPolicy: "Always"
 
 # replaces faas-netes with openfaas-operator
 operator:
-  image: openfaas/openfaas-operator:0.8.5
+  image: openfaas/openfaas-operator:0.8.6
   create: false
   # set this to false when creating multiple releases in the same cluster
   # must be true for the first one only
@@ -64,22 +64,10 @@ ingress:
   enabled: false
   # Used to create Ingress record (should be used with exposeServices: false).
   hosts:
-    - host: faas-netes.openfaas.local
-      serviceName: faas-netes
-      servicePort: 8080
-      path: /
     - host: gateway.openfaas.local
       serviceName: gateway
       servicePort: 8080
       path: /
-    - host: prometheus.openfaas.local
-      serviceName: prometheus
-      servicePort: 9090
-      path: /
-    - host: alertmanager.openfaas.local
-      serviceName: alertmanager
-      servicePort: 9093
-      path: /
   annotations:
     kubernetes.io/ingress.class: nginx
   tls:
```

## 1.2.3 

**Release date:** 2018-09-06

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Chart version bumped to 1.2.3 
* Document basic-auth and move to top of instructions 
* Bump gateway to 0.9.1 
* Bump up the gateway to 0.9.0 
* Bump up operator to 0.8.5 in values.yaml and bump charts 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 46f60240..57cd0956 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -24,7 +24,7 @@ faasnetesd:
     periodSeconds: 10
 
 gateway:
-  image: openfaas/gateway:0.8.11
+  image: openfaas/gateway:0.9.1
   readTimeout : "20s"
   writeTimeout : "20s"
   upstreamTimeout : "15s"  # Must be smaller than read/write_timeout
@@ -43,7 +43,7 @@ openfaasImagePullPolicy: "Always"
 
 # replaces faas-netes with openfaas-operator
 operator:
-  image: openfaas/openfaas-operator:0.8.4
+  image: openfaas/openfaas-operator:0.8.5
   create: false
   # set this to false when creating multiple releases in the same cluster
   # must be true for the first one only
@@ -91,4 +91,4 @@ tolerations: []
 
 affinity: {}
 
-kubernetesDNSDomain: cluster.local
\ No newline at end of file
+kubernetesDNSDomain: cluster.local
```

## 1.2.2 

**Release date:** 2018-08-21

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump GW & queue-worker 
* Add missing "svc" in function suffix 
* Add missing dot at the end of function suffix 
* Update function suffix to FQDN 
* Use kube dns domain value to configure addresses 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index f6db2b60..46f60240 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -24,7 +24,7 @@ faasnetesd:
     periodSeconds: 10
 
 gateway:
-  image: openfaas/gateway:0.8.9
+  image: openfaas/gateway:0.8.11
   readTimeout : "20s"
   writeTimeout : "20s"
   upstreamTimeout : "15s"  # Must be smaller than read/write_timeout
@@ -34,7 +34,7 @@ gateway:
   nodePort: 31112
 
 queueWorker:
-  image: openfaas/queue-worker:0.4.8
+  image: openfaas/queue-worker:0.4.9
   ackWait : "30s"
   replicas: 1
 
@@ -89,4 +89,6 @@ nodeSelector: {}
 
 tolerations: []
 
-affinity: {}
\ No newline at end of file
+affinity: {}
+
+kubernetesDNSDomain: cluster.local
\ No newline at end of file
```

## 1.2.1 

**Release date:** 2018-08-01

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Enable read-only file-system 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 6e6a5ae4..f6db2b60 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -9,7 +9,7 @@ securityContext: true
 basic_auth: false
 
 faasnetesd:
-  image: openfaas/faas-netes:0.5.7
+  image: openfaas/faas-netes:0.6.0
   readTimeout : "20s"
   writeTimeout : "20s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
@@ -43,7 +43,7 @@ openfaasImagePullPolicy: "Always"
 
 # replaces faas-netes with openfaas-operator
 operator:
-  image: openfaas/openfaas-operator:0.8.2
+  image: openfaas/openfaas-operator:0.8.4
   create: false
   # set this to false when creating multiple releases in the same cluster
   # must be true for the first one only
```

## 1.2.0 

**Release date:** 2018-07-25

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump charts for refactor 
* Add multiple release support for the same cluster - prefix the faas-netes Account, Role and RoleBinding with the Helm release name - make the Gateway nodePort configurable (must be changed with each install) - remove NATS ClusterIP duplicate service - tested on GKE 1.10 with 4 instances (2x faas-netes and 2x operator) - fix #263 
* Use Role instead of ClusterRole for the Operator - update openfaas-operator to 0.8.2 - remove the Operator ClusterRole - prefix the Operator Account, Role and RoleBinding with the Helm release name - split the RBAC file into operator and controller (faas-netes) - make the CRD creation optional (prep for multi-namespace support) - RBAC tested on GKE 1.10 
* Add nodeSelector, tolerations and affinity to all components - allow OpenFaaS to be scheduled on specific node pools 
* Add readiness and liveness probes to Gateway, Prometheus and Alertmanager - fix Alertmanager container port mapping - ref #261 
* Update OpenFaaS components to latest version - Gateway 0.8.9 (Expose scale-function endpoint via the gateway) - Operator 0.8.1 (Move to openfaas repo on Docker Hub) 
* Do not expose the faas-netes in or outside the cluster - remove internal and external services, faas-netes should be accessed only by the Gateway via localhost 
* Do not expose the monitoring components outside the cluster - remove external services for Prometheus and Alertmanager - fix #260 
* Change mentions from faas-netesd to faas-netes for yaml 
* Fixes issue with scale_from_zero for helm chart 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 0f5ebac9..6e6a5ae4 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -9,7 +9,7 @@ securityContext: true
 basic_auth: false
 
 faasnetesd:
-  image: openfaas/faas-netesd:0.5.7
+  image: openfaas/faas-netes:0.5.7
   readTimeout : "20s"
   writeTimeout : "20s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
@@ -24,12 +24,14 @@ faasnetesd:
     periodSeconds: 10
 
 gateway:
-  image: openfaas/gateway:0.8.7
+  image: openfaas/gateway:0.8.9
   readTimeout : "20s"
   writeTimeout : "20s"
   upstreamTimeout : "15s"  # Must be smaller than read/write_timeout
   replicas: 1
   scaleFromZero: false
+  # change the port when creating multiple releases in the same baremetal cluster
+  nodePort: 31112
 
 queueWorker:
   image: openfaas/queue-worker:0.4.8
@@ -41,8 +43,11 @@ openfaasImagePullPolicy: "Always"
 
 # replaces faas-netes with openfaas-operator
 operator:
-  image: functions/openfaas-operator:0.8.0
+  image: openfaas/openfaas-operator:0.8.2
   create: false
+  # set this to false when creating multiple releases in the same cluster
+  # must be true for the first one only
+  createCRD: true
 
 # monitoring and auto-scaling components
 prometheus:
@@ -59,8 +64,8 @@ ingress:
   enabled: false
   # Used to create Ingress record (should be used with exposeServices: false).
   hosts:
-    - host: faas-netesd.openfaas.local
-      serviceName: faas-netesd
+    - host: faas-netes.openfaas.local
+      serviceName: faas-netes
       servicePort: 8080
       path: /
     - host: gateway.openfaas.local
@@ -80,4 +85,8 @@ ingress:
   tls:
     # Secrets must be manually created in the namespace.
 
+nodeSelector: {}
 
+tolerations: []
+
+affinity: {}
\ No newline at end of file
```

## 1.1.7 

**Release date:** 2018-07-16

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Reduce the readiness checks for functions 
* Bump gateway version to 0.8.7 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 37bc0091..0f5ebac9 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -13,9 +13,18 @@ faasnetesd:
   readTimeout : "20s"
   writeTimeout : "20s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
+  httpProbe: false              # Setting to true will use a lock file for readiness and liveness
+  readinessProbe:
+    initialDelaySeconds: 0
+    timeoutSeconds: 1
+    periodSeconds: 1
+  livenessProbe:
+    initialDelaySeconds: 3
+    timeoutSeconds: 1
+    periodSeconds: 10
 
 gateway:
-  image: openfaas/gateway:0.8.5
+  image: openfaas/gateway:0.8.7
   readTimeout : "20s"
   writeTimeout : "20s"
   upstreamTimeout : "15s"  # Must be smaller than read/write_timeout
```

## 1.1.6 

**Release date:** 2018-07-20

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Change port to 8081 for faasnetesd and version to 1.1.6 

### Default value changes

```diff
# No changes in this release
```

## 1.1.5 

**Release date:** 2018-07-20

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Added scale_from_zero to helm chart 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 0b7e7691..37bc0091 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -20,6 +20,7 @@ gateway:
   writeTimeout : "20s"
   upstreamTimeout : "15s"  # Must be smaller than read/write_timeout
   replicas: 1
+  scaleFromZero: false
 
 queueWorker:
   image: openfaas/queue-worker:0.4.8
```

## 1.1.4 

**Release date:** 2018-07-12

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update images to latest versions 
* Correct typo in helm README 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index c0a98acd..0b7e7691 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -15,7 +15,7 @@ faasnetesd:
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
 
 gateway:
-  image: openfaas/gateway:0.8.4
+  image: openfaas/gateway:0.8.5
   readTimeout : "20s"
   writeTimeout : "20s"
   upstreamTimeout : "15s"  # Must be smaller than read/write_timeout
```

## 1.1.3 

**Release date:** 2018-07-09

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump chart to 1.1.3 
* Using correct faas-neteds image and pullpolicy 
* Removing faasnetesd-dep, which surfaced again after rebase 
* Ensuring operator and faas-netes have their port definition 
* Rebasing against Operator changes 
* Running faasnetesd in same pod as gateway 

### Default value changes

```diff
# No changes in this release
```

## 1.1.2 

**Release date:** 2018-07-07

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump Helm chart to 1.1.2 - add basic auth and security context options 
* Add basic auth setup documentation to Helm readme 
* Add basic auth and security context options to Helm chart - fix for Istio, security context must be disabled for Istio init container to run as root - tested on GKE 1.10 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 7fc6a0a1..c0a98acd 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -5,6 +5,8 @@ async: true
 exposeServices: true
 serviceType: NodePort
 rbac: true
+securityContext: true
+basic_auth: false
 
 faasnetesd:
   image: openfaas/faas-netesd:0.5.7
```

## 1.1.1 

**Release date:** 2018-07-07

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump helm chart to 1.1.1 
* Move to openfaas ns for queue-worker 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 0a6412e2..7fc6a0a1 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -7,7 +7,7 @@ serviceType: NodePort
 rbac: true
 
 faasnetesd:
-  image: functions/faas-netesd:0.5.5
+  image: openfaas/faas-netesd:0.5.7
   readTimeout : "20s"
   writeTimeout : "20s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
@@ -20,7 +20,7 @@ gateway:
   replicas: 1
 
 queueWorker:
-  image: functions/queue-worker:0.4.3
+  image: openfaas/queue-worker:0.4.8
   ackWait : "30s"
   replicas: 1
 
@@ -67,3 +67,5 @@ ingress:
     kubernetes.io/ingress.class: nginx
   tls:
     # Secrets must be manually created in the namespace.
+
+
```

## 1.1.0 

**Release date:** 2018-07-06

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Restructure images format to enable Weave Flux automatic updates - bump chart version to 1.1.0 - tested on GKE 1.10 with https://github.com/stefanprodan/openfaas-flux 
* Bump gateway and operator version - change gateway Docker repo to openfaas and update to 0.8.4 - update openfaas-operator to 0.8.0 
* Update Prometheus and AlertManager versions 
* Document OpenFaaS Operator in helm chart 
* Add openfaas-operator as a faas-netes replacement - run openfaas-operator as a gateway sidecar - enable gateway direct function calls - add openfaas-operator account, RBAC and CRD - make the gateway and worker replicas configurable - tested on GKE using https://github.com/stefanprodan/openfaas-flux/tree/master/charts/openfaas 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 97f834f2..0a6412e2 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -7,29 +7,40 @@ serviceType: NodePort
 rbac: true
 
 faasnetesd:
+  image: functions/faas-netesd:0.5.5
   readTimeout : "20s"
   writeTimeout : "20s"
   imagePullPolicy : "Always"    # Image pull policy for deployed functions
 
 gateway:
+  image: openfaas/gateway:0.8.4
   readTimeout : "20s"
   writeTimeout : "20s"
   upstreamTimeout : "15s"  # Must be smaller than read/write_timeout
+  replicas: 1
 
 queueWorker:
+  image: functions/queue-worker:0.4.3
   ackWait : "30s"
+  replicas: 1
 
 # image pull policy for openfaas components, can change to `IfNotPresent` in offline env
 openfaasImagePullPolicy: "Always"
 
-# images of openfaas components
-images:
-  controller: functions/faas-netesd:0.5.5
-  gateway: functions/gateway:0.8.2
-  prometheus: prom/prometheus:v2.2.0
-  alertmanager: prom/alertmanager:v0.15.0-rc.0
-  nats: nats-streaming:0.6.0
-  queueWorker: functions/queue-worker:0.4.3
+# replaces faas-netes with openfaas-operator
+operator:
+  image: functions/openfaas-operator:0.8.0
+  create: false
+
+# monitoring and auto-scaling components
+prometheus:
+  image: prom/prometheus:v2.3.1
+alertmanager:
+  image: prom/alertmanager:v0.15.0
+
+# async provider
+nats:
+  image: nats-streaming:0.6.0
 
 # ingress configuration
 ingress:
```

## 1.0.21 

**Release date:** 2018-05-21

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Add "openfaasImagePullPolicy" configuration option for openfaas system components 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 94917883..97f834f2 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -19,7 +19,10 @@ gateway:
 queueWorker:
   ackWait : "30s"
 
-# images
+# image pull policy for openfaas components, can change to `IfNotPresent` in offline env
+openfaasImagePullPolicy: "Always"
+
+# images of openfaas components
 images:
   controller: functions/faas-netesd:0.5.5
   gateway: functions/gateway:0.8.2
```

## 1.0.20 

**Release date:** 2018-06-07

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Remove trailing a from helm chart service selector 

### Default value changes

```diff
# No changes in this release
```

## 1.0.19 

**Release date:** 2018-06-06

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Break chart templates into separate files by type 
* Move instructions to use GitHub published helm chart 

### Default value changes

```diff
# No changes in this release
```

## 1.0.18 

**Release date:** 2018-06-06

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump faas-netesd to new release 0.5.5 
* Bump gw/faas-netes + fix label update 
* Add upstream_timeout value to helm for gateway 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 7d464810..94917883 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -9,18 +9,20 @@ rbac: true
 faasnetesd:
   readTimeout : "20s"
   writeTimeout : "20s"
+  imagePullPolicy : "Always"    # Image pull policy for deployed functions
 
 gateway:
   readTimeout : "20s"
   writeTimeout : "20s"
+  upstreamTimeout : "15s"  # Must be smaller than read/write_timeout
 
 queueWorker:
   ackWait : "30s"
 
 # images
 images:
-  controller: functions/faas-netesd:0.5.2
-  gateway: functions/gateway:0.8.1
+  controller: functions/faas-netesd:0.5.5
+  gateway: functions/gateway:0.8.2
   prometheus: prom/prometheus:v2.2.0
   alertmanager: prom/alertmanager:v0.15.0-rc.0
   nats: nats-streaming:0.6.0
```

## 1.0.17 

**Release date:** 2018-05-20

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Enable HTTP livenessProbe on gateway 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index dc4276e0..7d464810 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -20,7 +20,7 @@ queueWorker:
 # images
 images:
   controller: functions/faas-netesd:0.5.2
-  gateway: functions/gateway:0.8.0
+  gateway: functions/gateway:0.8.1
   prometheus: prom/prometheus:v2.2.0
   alertmanager: prom/alertmanager:v0.15.0-rc.0
   nats: nats-streaming:0.6.0
```

## 1.0.16 

**Release date:** 2018-05-16

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Add openfaas-1.0.16.tgz and update install.md with 1.0.16 
* Update faas-netesd version to 0.5.2 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 4eb68d3b..dc4276e0 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -19,7 +19,7 @@ queueWorker:
 
 # images
 images:
-  controller: functions/faas-netesd:0.5.1
+  controller: functions/faas-netesd:0.5.2
   gateway: functions/gateway:0.8.0
   prometheus: prom/prometheus:v2.2.0
   alertmanager: prom/alertmanager:v0.15.0-rc.0
```

## 1.0.15 

**Release date:** 2018-05-16

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Apply direct_functions flag to helm 

### Default value changes

```diff
# No changes in this release
```

## 1.0.14 

**Release date:** 2018-05-14

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump faas-netes to 0.5.1 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index c5ffd0b5..4eb68d3b 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -19,7 +19,7 @@ queueWorker:
 
 # images
 images:
-  controller: functions/faas-netesd:0.5.0
+  controller: functions/faas-netesd:0.5.1
   gateway: functions/gateway:0.8.0
   prometheus: prom/prometheus:v2.2.0
   alertmanager: prom/alertmanager:v0.15.0-rc.0
```

## 1.0.13 

**Release date:** 2018-04-27

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update gateway version to 0.8.0 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index a9492b09..c5ffd0b5 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -20,7 +20,7 @@ queueWorker:
 # images
 images:
   controller: functions/faas-netesd:0.5.0
-  gateway: functions/gateway:0.7.9
+  gateway: functions/gateway:0.8.0
   prometheus: prom/prometheus:v2.2.0
   alertmanager: prom/alertmanager:v0.15.0-rc.0
   nats: nats-streaming:0.6.0
```

## 1.0.12 

**Release date:** 2018-04-24

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump faasnetes and gateway 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 2a6d4439..a9492b09 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -19,8 +19,8 @@ queueWorker:
 
 # images
 images:
-  controller: functions/faas-netesd:0.4.8
-  gateway: functions/gateway:0.7.8
+  controller: functions/faas-netesd:0.5.0
+  gateway: functions/gateway:0.7.9
   prometheus: prom/prometheus:v2.2.0
   alertmanager: prom/alertmanager:v0.15.0-rc.0
   nats: nats-streaming:0.6.0
```

## 1.0.11 

**Release date:** 2018-04-06

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update chart for image_pull_policy 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index d333d3e0..2a6d4439 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -19,7 +19,7 @@ queueWorker:
 
 # images
 images:
-  controller: functions/faas-netesd:0.4.6
+  controller: functions/faas-netesd:0.4.8
   gateway: functions/gateway:0.7.8
   prometheus: prom/prometheus:v2.2.0
   alertmanager: prom/alertmanager:v0.15.0-rc.0
```

## 1.0.10 

**Release date:** 2018-03-29

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update faas-netes and queue-worker versions 
* Update README.md to current values.yaml 
* Update helm chart to Prometheus 2.2 
* Update to mention LoadBalancer and chart folder 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 7a2abcd0..d333d3e0 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -19,12 +19,12 @@ queueWorker:
 
 # images
 images:
-  controller: functions/faas-netesd:0.4.5
-  gateway: functions/gateway:0.7.3
-  prometheus: prom/prometheus:v2.1.0
-  alertmanager: prom/alertmanager:v0.7.1
+  controller: functions/faas-netesd:0.4.6
+  gateway: functions/gateway:0.7.8
+  prometheus: prom/prometheus:v2.2.0
+  alertmanager: prom/alertmanager:v0.15.0-rc.0
   nats: nats-streaming:0.6.0
-  queueWorker: functions/queue-worker:0.4
+  queueWorker: functions/queue-worker:0.4.3
 
 # ingress configuration
 ingress:
```

## 1.0.9 

**Release date:** 2018-03-25

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Removing arm/armhf references from helm chart 
* Bump faas-netesd daemon 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 60d4fdd6..7a2abcd0 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -1,7 +1,6 @@
 functionNamespace:
 
 async: true
-armhf: false
 
 exposeServices: true
 serviceType: NodePort
@@ -20,23 +19,12 @@ queueWorker:
 
 # images
 images:
-  controller: functions/faas-netesd:0.4.4
-  controllerArmhf: functions/faas-netesd-armhf
-
+  controller: functions/faas-netesd:0.4.5
   gateway: functions/gateway:0.7.3
-  gatewayArmhf: functions/gateway:0.7.0-armhf
-
   prometheus: prom/prometheus:v2.1.0
-  prometheusArmhf: alexellis2/prometheus-armhf:1.5.2
-
   alertmanager: prom/alertmanager:v0.7.1
-  alertmanagerArmhf: alexellis2/alertmanager-armhf:0.5.1
-
   nats: nats-streaming:0.6.0
-  natsArmhf:
-
   queueWorker: functions/queue-worker:0.4
-  queueWorkerArmhf:
 
 # ingress configuration
 ingress:
```

## 1.0.8 

**Release date:** 2018-03-14

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Add functions_dns_suffix for future updates and absolute DNS for upstream calls and for NATS Streaming 

### Default value changes

```diff
# No changes in this release
```

## 1.0.7 

**Release date:** 2018-03-01

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Bump GW to 0.7.3 and FaaS-Netes to 0.4.4 
* Bump GW version, use Golang duration for timeouts 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 9123d856..60d4fdd6 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -8,22 +8,22 @@ serviceType: NodePort
 rbac: true
 
 faasnetesd:
-  readTimeout : "20"
-  writeTimeout : "20"
+  readTimeout : "20s"
+  writeTimeout : "20s"
 
 gateway:
-  readTimeout : "20"
-  writeTimeout : "20"
+  readTimeout : "20s"
+  writeTimeout : "20s"
 
 queueWorker:
   ackWait : "30s"
 
 # images
 images:
-  controller: functions/faas-netesd:0.4.2
+  controller: functions/faas-netesd:0.4.4
   controllerArmhf: functions/faas-netesd-armhf
 
-  gateway: functions/gateway:0.7.0
+  gateway: functions/gateway:0.7.3
   gatewayArmhf: functions/gateway:0.7.0-armhf
 
   prometheus: prom/prometheus:v2.1.0
```

## 1.0.6 

**Release date:** 2018-02-25

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Update Chart.yaml 
* Update README.md 
* Ensure that eh ServiceAccout is always created 

### Default value changes

```diff
# No changes in this release
```

## 1.0.5 

**Release date:** 2018-02-12

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* Quote timeout values 
* Swap order of recommended namespaces for helm 
* Fix the mess in fassnetesd and gateway templates Signed-off-by: Rimas Mocevicius <rmocius@gmail.com> 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index b5b31541..9123d856 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -8,8 +8,15 @@ serviceType: NodePort
 rbac: true
 
 faasnetesd:
-  readTimeout : 60
-  writeTimeout : 60
+  readTimeout : "20"
+  writeTimeout : "20"
+
+gateway:
+  readTimeout : "20"
+  writeTimeout : "20"
+
+queueWorker:
+  ackWait : "30s"
 
 # images
 images:
```

## 1.0.4 

**Release date:** 2018-02-06

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* add queue-worker timeouts Signed-off-by: Rimas Mocevicius <rmocius@gmail.com> 
* Upgrade prometheu to v2.1 
* Bump gw & faas-netes versions 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 7a01149b..b5b31541 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -7,15 +7,19 @@ exposeServices: true
 serviceType: NodePort
 rbac: true
 
+faasnetesd:
+  readTimeout : 60
+  writeTimeout : 60
+
 # images
 images:
-  controller: functions/faas-netesd:0.3.9
+  controller: functions/faas-netesd:0.4.2
   controllerArmhf: functions/faas-netesd-armhf
 
   gateway: functions/gateway:0.7.0
   gatewayArmhf: functions/gateway:0.7.0-armhf
 
-  prometheus: prom/prometheus:v1.5.2
+  prometheus: prom/prometheus:v2.1.0
   prometheusArmhf: alexellis2/prometheus-armhf:1.5.2
 
   alertmanager: prom/alertmanager:v0.7.1
```

## 1.0.3 

**Release date:** 2018-02-01

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* make async=true as default Signed-off-by: Rimas Mocevicius <rmocius@gmail.com> 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 50d95929..7a01149b 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -1,6 +1,6 @@
 functionNamespace:
 
-async: false
+async: true
 armhf: false
 
 exposeServices: true
```

## 1.0.2 

**Release date:** 2018-02-01

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* update chart readme.md and helm.md Signed-off-by: Rimas Mocevicius <rmocius@gmail.com> 
* Update to 0.7.0 gateway 

### Default value changes

```diff
diff --git a/chart/openfaas/values.yaml b/chart/openfaas/values.yaml
index 8d1005b6..50d95929 100644
--- a/chart/openfaas/values.yaml
+++ b/chart/openfaas/values.yaml
@@ -1,6 +1,8 @@
 functionNamespace:
+
 async: false
 armhf: false
+
 exposeServices: true
 serviceType: NodePort
 rbac: true
@@ -10,8 +12,8 @@ images:
   controller: functions/faas-netesd:0.3.9
   controllerArmhf: functions/faas-netesd-armhf
 
-  gateway: functions/gateway:0.6.14
-  gatewayArmhf: functions/gateway:0.6.7-armhf
+  gateway: functions/gateway:0.7.0
+  gatewayArmhf: functions/gateway:0.7.0-armhf
 
   prometheus: prom/prometheus:v1.5.2
   prometheusArmhf: alexellis2/prometheus-armhf:1.5.2
```

## 1.0.1 

**Release date:** 2018-01-24

![Helm: v2](https://img.shields.io/static/v1?label=Helm&message=v2&color=inactive&logo=helm)
![Helm: v3](https://img.shields.io/static/v1?label=Helm&message=v3&color=informational&logo=helm)


* add readme.md add owners change {[ template “openfaas.name” . }} ito  {{ template “faas-netesd.name” }} update _helpers.tpl Signed-off-by: Rimas Mocevicius <rmocius@gmail.com> 

### Default value changes

```diff
functionNamespace:
async: false
armhf: false
exposeServices: true
serviceType: NodePort
rbac: true

# images
images:
  controller: functions/faas-netesd:0.3.9
  controllerArmhf: functions/faas-netesd-armhf

  gateway: functions/gateway:0.6.14
  gatewayArmhf: functions/gateway:0.6.7-armhf

  prometheus: prom/prometheus:v1.5.2
  prometheusArmhf: alexellis2/prometheus-armhf:1.5.2

  alertmanager: prom/alertmanager:v0.7.1
  alertmanagerArmhf: alexellis2/alertmanager-armhf:0.5.1

  nats: nats-streaming:0.6.0
  natsArmhf:

  queueWorker: functions/queue-worker:0.4
  queueWorkerArmhf:

# ingress configuration
ingress:
  enabled: false
  # Used to create Ingress record (should be used with exposeServices: false).
  hosts:
    - host: faas-netesd.openfaas.local
      serviceName: faas-netesd
      servicePort: 8080
      path: /
    - host: gateway.openfaas.local
      serviceName: gateway
      servicePort: 8080
      path: /
    - host: prometheus.openfaas.local
      serviceName: prometheus
      servicePort: 9090
      path: /
    - host: alertmanager.openfaas.local
      serviceName: alertmanager
      servicePort: 9093
      path: /
  annotations:
    kubernetes.io/ingress.class: nginx
  tls:
    # Secrets must be manually created in the namespace.
```

---
Autogenerated from Helm Chart and git history using [helm-changelog](https://github.com/mogensen/helm-changelog)
