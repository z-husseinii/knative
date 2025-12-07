### error 
boker not ready 
```
kubectl get broker -A
 
NAMESPACE NAME URL AGE READY REASON 
default kafka-broker 17h False Failed to create topic: knative-broker-default-kafka-broker
```
```
kubectl describe broker kafka-broker

Warning InternalError 13m (x84 over 17h) broker-controller failed to create topic: knative-broker-default-kafka-broker: kafka server: Replication-factor is inva lid - Unable to replicate the partition 3 time(s): The target replication factor of 3 cannot be reached because only 1 broker(s) are registered
```

 راه حل 

نغییر default.topic.replication.factor در " kafka.broker.config " config map  از 3 به ۱

kafka برای replication کمتر از تعداد broker واقعی نمیتواند کاری بکند 

ما هم در این جا یک broker داریم 

------------------------------------------------------------------------------------------------------------------------
### error
trigger not ready  
```
kubectl get trigger -A 
 
NAMESPACE NAME BROKER SUBSCRIBER_URI AGE READY REASON 
default event-display-trigger kafka-broker 6s False ReconcileContract
```


```
kubectl describe trigger kafka trigger 
(/)راه حل :

```
 subscriber ساخته نشده اول باید اون رو ساخت 
```
kubectl apply -f event-display.yaml
```
------------------------------------------------------------------------------------------------------------------------
### error
trigger not ready  

```
kubectl get trigger -A

NAMESPACE   NAME                    BROKER         SUBSCRIBER_URI   AGE     READY   REASON
default     event-display-trigger   kafka-broker                    4m46s   False   BindInProgress
```


```
kubectl descibr trigger kafka-trigger 
 
Last Transition Time: 2025-12-07T09:46:42Z Message: Pod "kafka-broker-dispatcher-0" is in phase "Pending" with conditions [PodReadyToStartContainers=False, Initialized=True, Ready=False, Cont ainersReady=False, PodScheduled=True] Reason: BindInProgress Status: False Type: Ready Last Transition Time: 2025-12-07T09:46:42Z Status: True Type: SubscriberResolved Last Transition Time: 2025-12-07T09:46:42Z Status: True Type: SubscriptionReady Observed Generation: 1 Events: Type Reason Age From Message ---- ------ ---- ---- ------- Normal FinalizerUpdate 53s kafka-trigger-controller Updated "event-display-trigger" finalizers (END)
```

```
kubectl describe pod kafka-broker-dispatcher-0 -n knative-eventing

e:3.6": failed to pull image "index.docker.io/rancher/mirrored-pause:3.6": failed to pull and unpack image "docker.io/rancher/mirrored-pause:3.6": failed to resolve r eference "docker.io/rancher/mirrored-pause:3.6": docker.io/rancher/mirrored-pause:3.6: not found Normal SandboxChanged 85s (x204 over 6m26s) kubelet Pod sandbox changed, it will be killed and re-created.
```
 راه حل :

پاد dispatcher ساخته نمیشد چون kubelet نمیتونست ایمیج رو بگیره 
فایل tar اسمیج رو با دستور ctr به ماشینی که پاد روش ساخته شده بود اضافه کردم 

wget --user=admin --password='nexusadmin12345' 
https://repo.xstudioo.ir/repository/moavenat-commands/mirrored-pause-3.6.tar
sudo ctr --address /run/k3s/containerd/containerd.sock -n k8s.io images import mirrored-pause-3.6.tar
------------------------------------------------------------------------------------------------------------------------
### error
 چون پادش روی node master3 که خراب است افتاده نمیتونم اجراش کنم 
```
kubectl exec -it event-publisher -- /bin/sh
error: Internal error occurred: error sending request: Post "https://192.168.11.15:10250/exec/default/event-publisher/curl?command=%!F(MISSING)bin%!F(MISSING)sh&input =1&output=1&tty=1": proxy error from 127.0.0.1:9345 while dialing 192.168.11.15:10250, code 502: 502 Bad Gateway
```
 راه حل :

باید کاری کنم روی یه نود دیگه بیفته پادش (‌ add nodeSelector)
```
#event-publisher.yaml
apiVersion: v1
kind: Pod
metadata:
  name: event-publisher
  namespace: default
spec:
  nodeSelector:
          kubernetes.io/hostname: master-1
  containers:
  - name: curl
    image: docker-hosted.xstudioo.ir/curlimages/curl
    command: ["sh", "-c", "while true; do curl -X POST ... ; sleep 10; done"]
```

kubectl apply -f event-publisher.yaml
------------------------------------------------------------------------------------------------------------------------
### error
 event display پاد هاش رو میذاشت روی node master3 که مشکل داره .برای همین نمیتونم لاگش رو ببینم 

kubectl logs -l serving.knative.dev/service=event-display -c user-container -f

error: Internal error occurred: error sending request: Post "https://192.168.11.15:10250/exec/default/event-publisher/curl?command=%!F(MISSING)bin%!F(MISSING)sh&input =1&output=1&tty=1": proxy error from 127.0.0.1:9345 while dialing 192.168.11.15:10250, code 502: 502 Bad Gateway
 

(/)راه حل :

باید کاری کنیم پاد event-display روی master 3 ساخته نشه 
با دستور زیر scheduling master3 غیر فعال میشه 

kubectl cordon master 3
kubectl get nodes
NAME STATUS ROLES AGE VERSION 
master-1 Ready control-plane,etcd,master,worker 128d v1.32.5+rke2r1 
master-3 Ready control-plane,etcd,master,worker 18d v1.32.5+rke2r1 
master2 Ready control-plane,etcd,master,worker 71d v1.32.5+rke2r1 
master3 Ready,SchedulingDisabled control-plane,etcd,master,worker 70d v1.32.5+rke2r1 
worker-2 Ready control-plane,etcd,master,worker 68d v1.32.5+rke2r1
برای برگرداندن به حالت قبل 

kubectl uncordon master3
