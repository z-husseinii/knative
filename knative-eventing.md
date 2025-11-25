https://knative.dev/docs/install/yaml-install/eventing/install-eventing-with-yaml/#verify-the-installation

ایمیج های فایل های yaml به رجیستری شخصی اضافه شد چون جایی که قراره پروژه پیاده سازی بشه به اینترنت دسترسی نداره

دانلود فایل ها از رجیستری چون امکان کپی کردن مرتب متن  در سیستم های معاونت وجود ندارد 
‍‍‍```
wget --user=<username> --password=<'password'> <file path in nexus>
wget --user=admin --password='nexusadmin12345' https://repo.xstudioo.ir/repository/moavenat-commands/knative-eventing/config-nats.yaml
```

## Install the Knative Eventing component


```
kubectl apply -f 
kubectl apply -f 
```

check
```
kubectl get pods -n knative-eventing
```


## Install a default Channel (messaging) layer
### Install NATS Streaming for Kubernetes.
```
kubectl apply -f natsjsm.yaml  
kubectl apply -f config-nats.yaml
kubectl apply -f config-br-default-channel-jsm.yaml
```

#### create jetstream channels
kubectl apply -f 
```
 apiVersion: messaging.knative.dev/v1alpha1
 kind: NatsJetStreamChannel
 metadata:
     name: channel-defaults
     namespace: knative-eventing
```



#### edit configmap config-br-defaults
```
default-br-config
clusterDefault:

  brokerClass: MTChannelBasedBroker

  apiVersion: v1

  kind: ConfigMap

  name: config-natjsm-channel

  namespace: knative-eventing
```


kubectl apply -f - <<EOF
apiVersion: eventing.knative.dev/v1
kind: Broker
metadata:
  name: test-broker
  namespace: default
EOF
broker.eventing.knative.dev/test-broker created
> kubectl get broker -n default -o wide
NAME          URL   AGE   READY   REASON   CLASS
test-broker         2s                     MTChannelBasedBroker
> kubectl get broker -n default -o wide
NAME          URL                                                                            AGE   READY   REASON   CLASS
test-broker   http://broker-ingress.knative-eventing.svc.cluster.local/default/test-broker   8s    True             MTChannelBasedBroker

