1. install knative evnting component
2. install kafka cluster
3. install kafka broker layer
4. test send event
- send events with kn :
- send events with curl :
 
   - create broker instance
     ```
     kubectl apply -f kafka broker.yaml
     ```
   - create subscriber service
     ```
     kubectl apply -f event-display.yaml
     ```
   - create trigger
     ```
     kubectl apply -f kafka-trigger.yaml
     ```
     
   - create pod with curl image to send evevnt from that
     ```
     kubectl apply -f event-publisher.yaml
     ```
   - send events
       ```
       kubectl exec -it event-publisher -- /bin/sh
    
       curl -v \
         -X POST \
         -H "Ce-SpecVersion: 1.0" \
         -H "Ce-Type: demo.msg" \
         -H "Ce-Source: curl-demo" \
         -H "Ce-Id: 42" \
         -H "Content-Type: application/json" \
         -d '{"foo":"bar"}' \
        http://kafka-broker-ingress.knative-eventing.svc.cluster.local/default/kafka-broker
       ```
     outputs:
       ```
              
        Note: Unnecessary use of -X or --request, POST is already inferred.
        * Host kafka-broker-ingress.knative-eventing.svc.cluster.local:80 was resolved.
        * IPv6: (none)
        * IPv4: 10.43.136.11
        *   Trying 10.43.136.11:80...
        * Established connection to kafka-broker-ingress.knative-eventing.svc.cluster.local (10.43.136.11 port 80) from 10.42.39.65 port 45578
        * using HTTP/1.x
        > POST /default/kafka-broker HTTP/1.1
        > Host: kafka-broker-ingress.knative-eventing.svc.cluster.local
        > User-Agent: curl/8.16.0
        > Accept: */*
        > Ce-SpecVersion: 1.0
        > Ce-Type: demo.msg
        > Ce-Source: curl-demo
        > Ce-Id: 42
        > Content-Type: application/json
        > Content-Length: 13
        >
        * upload completely sent off: 13 bytes
        < HTTP/1.1 202 Accepted
        < content-length: 0
        <
        * Connection #0 to host kafka-broker-ingress.knative-eventing.svc.cluster.local:80 left intact
       ```
   - check recived events
     ```
     kubectl logs event-display-00001-deployment-6f75f7cbf4-fbw9z
     ```
     output:
     ```
      2025/12/07 11:34:36 failed to parse observability config from env, falling back to default config
      2025/12/07 11:34:36 failed to correctly initialize otel resource, resouce may be missing some attributes: the environment variable "SYSTEM_NAMESPACE" is not set, not
      adding "k8s.namespace.name" to otel attributes
      ☁️  cloudevents.Event
      Context Attributes,
        specversion: 1.0
        type: demo.msg
        source: curl-demo
        id: 42
        datacontenttype: application/json
      Extensions,
        knativekafkaoffset: 0
        knativekafkapartition: 5
      Data,
        {
          "foo": "bar"
        }
     ```
