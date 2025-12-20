## test knatiev serving 
```
curl -v -H "Host: farayand-application-service.farayand-knative.knative.baas.ir" http://knative.baas.ir/companies/
```


output
```
*   Trying 194.225.136.127:80...
* Connected to knative.baas.ir (194.225.136.127) port 80 (#0)
> GET /companies/ HTTP/1.1
> Host: farayand-application-service.farayand-knative.knative.baas.ir
> User-Agent: curl/7.81.0
> Accept: */*
> 
* Mark bundle as not supporting multiuse
< HTTP/1.1 200 OK
< allow: GET, HEAD, OPTIONS
< content-length: 48
< content-type: application/json
< cross-origin-opener-policy: same-origin
< date: Sun, 14 Dec 2025 13:01:07 GMT
< referrer-policy: same-origin
< server: envoy
< vary: Accept, origin, Cookie
< x-content-type-options: nosniff
< x-frame-options: DENY
< x-envoy-upstream-service-time: 1085
< 
* Connection #0 to host knative.baas.ir left intact
```

### error 1
```
kubectl describe pod -n farayand-knative
 Warning FailedCreatePodSandBox 28s (x16 over 43s) kubelet (combined from similar events): Failed to create pod sandbox: rpc error: code = NotFound de c = failed to start sandbox "3ae220499314e1b3fb0b35711826fc18182e5afdd36f42422c995dbffe9853f7": failed to get sandbox image "index.docker.io/rancher/mirrored-pause:3. 6": failed to pull image "index.docker.io/rancher/mirrored-pause:3.6": failed to pull and unpack image "docker.io/rancher/mirrored-pause:3.6": failed to resolve refer ence "docker.io/rancher/mirrored-pause:3.6": docker.io/rancher/mirrored-pause:3.6: not found
```
resolve :
ایمپورت کردن ایمیج روی نودی که پاد میخواد ساخته بشه
```
ctr --address /run/k3s/containerd/containerd.sock -n k8s.io images import mirrored-pause-3.6.tar
```

هر بار میخواست بیاد دوباره ایمیج رو بگیره چون اسمش فرق میکرد  با دستور زیر هر دو تگ رو داره 

```
sudo ctr --address /run/k3s/containerd/containerd.sock -n k8s.io images tag \
  docker.io/rancher/mirrored-pause:3.6 \
  index.docker.io/rancher/mirrored-pause:3.6
```

### error 2
```
curl -vk -H "Host: farayand-application-service.farayand-knative.knative.baas.ir" knative.baas.ir/companies/

* Trying 194.225.136.127:80... * Connected to knative.baas.ir (194.225.136.127) port 80 (#0) > GET /companies/ HTTP/1.1 > Host: farayand-application-service.farayand-knative.knative.baas.ir > User-Agent: curl/7.81.0 > Accept: */* > * Mark bundle as not supporting multiuse < HTTP/1.1 400 Bad Request < content-type: text/html; charset=utf-8 < cross-origin-opener-policy: same-origin < date: Sat, 13 Dec 2025 10:23:53 GMT < referrer-policy: same-origin < server: envoy < vary: origin < x-content-type-options: nosniff < x-envoy-upstream-service-time: 2137 < transfer-encoding: chunked < <!DOCTYPE html> <html lang="en"> <head> <meta http-equiv="content-type" content="text/html; charset=utf-8"> <meta name="robots" content="NONE,NOARCHIVE"> <title>DisallowedHost at /companies/</title> <style> html * { padding:0; margin:0; } body * { padding:10px 20px; } body * * { padding:0; } body { font-family: sans-serif; background-color:#fff; color:#000; } body > :where(header, main, footer) { border-bottom:1px solid #ddd; } h1 { font-weight:normal; } h2 { margin-bottom:.8em; }

```
resolve :
Django (یا هر فریم‌ورک مشابه با محافظت Host) قبل از اینکه حتی به ویو برسد می‌گوید:

«این Host را نمی‌شناسم → احتمال حمله Host Header → 400»

1- تغییر ALLOWED_HOSTS در فایل dev.env

2- تعریف متغییر ALLOWED_HOSTS در knative service پروژه
```
spec:
  template:
    spec:
      containers:
        - env:
            - name: ALLOWED_HOSTS
              value: >-
                farayand-application-service.farayand-knative.knative.baas.ir,knative.baas.ir
```

### error 3
```
curl -vk -H "Host: farayand-application-service.farayand-knative.knative.baas.ir" knative.baas.ir/companies/
 
* Trying 194.225.136.127:80... * Connected to knative.baas.ir (194.225.136.127) port 80 (#0) > GET /companies/ HTTP/1.1 > Host: farayand-application-service.farayand-knative.knative.baas.ir > User-Agent: curl/7.81.0 > Accept: */* > * Mark bundle as not supporting multiuse < HTTP/1.1 503 Service Unavailable < allow: GET, HEAD, OPTIONS < content-length: 66 < content-type: application/json < cross-origin-opener-policy: same-origin < date: Sat, 13 Dec 2025 10:50:00 GMT < referrer-policy: same-origin < server: envoy < vary: Accept, origin, Cookie < x-content-type-options: nosniff < x-frame-options: DENY < x-envoy-upstream-service-time: 25 < * Connection #0 to host knative.baas.ir left intact {"error":"در انجام عملیات مشکلی پیش آمد."}****
```


```
kubectl logs farayand-application-service-00002-deployment-859db44b8-plrnp -n farayand-knative
Watching for file changes with StatReloader 
Service Unavailable: /companies/ [13/Dec/2025 04:49:39] "GET /companies/ HTTP/1.1" 503 66 
Service Unavailable: /companies/ [13/Dec/2025 04:49:46] "GET /companies/ HTTP/1.1" 503 66 
Service Unavailable: /companies/ [13/Dec/2025 04:50:00] "GET /companies/ HTTP/1.1" 503 66
```

resolve :
. خود اپلیکیشن دارد 503 برمی‌گرداند

متغییر ها در کد درست تعریف نشده اند و به پایگاه داده لوکال وصل میشود 
1- تعریف متغییر در knative service 

```
   containers:
        - env:
            - name: DB_SERVICE_URL
              value: http://192.168.11.16:32650/mongodb/
            - name: MONGO_HOST
              value: mongodb-instance-svc.mongodb-operator.svc.cluster.local
            - name: MONGO_USERNAME
              value: mms-automation
            - name: MONGO_PASSWORD
              value: HOOoAvEFDr3bRT68T4mP
```

2- تغییر کد و ساخت ایمیج جدید
### error 4
```
kubectl exec -it <>   -n farayand-knative -- /bin/sh
cat logs/app.log

The requested URL was not found on the server. If you entered the URL manually please check your spelling and try again.</p> [ERROR] 2025-12-13 07:18:31,511 django.request Internal Server Error: /companies/ [ERROR] 2025-12-13 07:18:32,597 applications.services.company_service db_service failed for get_companies with filter={}, status_code=404, response=<!doctype html> <html lang=en> <title>404 Not Found</title> <h1>Not Found</h1> <p>The requested URL was not found on the server. If you entered the URL manually please check your spelling and try again.</p> [ERROR] 2025-12-13 07:18:32,597 django.request Internal Server Error: /companies/ [ERROR] 2025-12-13 07:19:00,272 applications.services.company_service db_service failed for get_companies with filter={}, status_code=404, response=<!doctype html> <html lang=en> <title>404 Not Found</title> <h1>Not Found</h1> <p>The requested URL was not found on the server. If you entered the URL manually please check your spelling and try again.</p> [ERROR] 2025-12-13 07:19:00,272 django.request Internal Server Error: /companies/ [ERROR] 2025-12-13 07:19:01,097 applications.services.company_service db_service failed for get_companies with filter={}, status_code=404, response=<!doctype html> <html lang=en> <title>404 Not Found</title> <h1>Not Found</h1> <p>The requested URL was not found on the server. If you entered the URL manually please check your spelling and try again.</p> [ERROR] 2025-12-13 07:19:01,097 django.request Internal Server Error: /companies/
```

resolve :

درخواست به URL دیتابیس سرویس (یا همون API که واسط MongoDB هست) زده می‌شه ولی مسیر درخواست شده وجود نداره

تغییر متغییر DB_SERVICE_URL از http://192.168.11.16:32650/mongodb  به http://192.168.11.16:32650/mongodb/ 

 

تهش / جا افتاده بود


### error 5

```
kubectl describe pod -n farayand-knative
Warning Failed 13s (x4 over 53s) kubelet Error: failed to create containerd task: failed to create shim task: OCI runtime create failed: runc create failed: un able to start container process: error during container init: exec: "ENV_FILE=prod.env": executable file not found in $PATH Warning BackOff 2s (x6 over 52s) kubelet Back-off restarting failed container database in pod database-bb45b4f65-84ddd_baas(4be97900-b116-4ab9-90de-fc044f37bab 9)
```

resolve 
در داکر فایل این خط رو داشتیم 
```
CMD ["ENV_FILE=prod.env", "python", "allApi.py"]
```
که اشتباه است و باید یه صورت زیر نوشته شود  

```
ENV ENV_FILE=prod.env 
CMD ["python", "allApi.py"]
```


### error 6
```
curl -v -H "Host: farayand-application-service.farayand-knative.knative.baas.ir" http://knative.baas.ir/companies/
500 internal error
```


```
kubectl logs database-7bf957599d-vljqh -n baas
pymongo.errors.OperationFailure: Received authentication for mechanism SCRAM-SHA-1 which is not enabled, full error: {'ok': 0.0, 'errmsg': 'Received authentication for mechanism SCRAM-SHA-1 which is not enabled', 'code': 334, 'codeName': 'MechanismUnavailable', '$clusterTime': {'clusterTime': Timestamp(1765642359, 1), 'signature': {'hash': b'Fu^\xc3\xc5\x86m\xe5]\xee{\xbc-\xd2\x0eD\xcb\x9e\xf5\xb9', 'keyId': 7549944330408951815}}, 'operationTime': Timestamp(1765642359, 1)}
```
resolve :
1- تغییر مقدار authSource در کد به admin 

چون یوزر mms-automation در دیتابیس admin ساخته شده 

2- ساخت یک یوزر جدید در دیتابیس application_service_database 

چون مقدار authSource در کد application_service_database  میباشد 

بعد از ساخت یوزر   متغییر یوزر را  در دیپلویمنت دیتابیس تعریف میکنیم .

```
mongosh -u mms-automation -p HOOoAvEFDr3bRT68T4mP

use application_service_database

db.createUser(
  {
    user: "application_user",
    pwd:  "HOOoAvEFDr3bRT68T4mP",   // or cleartext password
    roles: [ { role: "readWrite", db: "application_service_database" }]
  }
)
```
تعریف متغییر در deployment 


```
MONGO_USERNAME=application_user
```



### error 7 
```
curl -v -H "Host: farayand-application-service.farayand-knative.knative.baas.ir" 
http://knative.baas.ir/companies/ Could not resolve host: knative.baas.ir Store negative name resolve for knative.baas.ir:80* shutting down connection #0curl: (6) Could not resolve host: knative.baas.ir
```
resolve :
دامنه تغییر کرده باید موارد زیر برای دامنه جدید اصلاح شود 
1. تغییر kourier mapping
```
spec:
  host: ^([a-z0-9-.]+)\.knative\.dev\.isti\.ir$
```
2. اضافه کردن دامنه جدید به /etc/hosts جایی که تست انجام میشود 
```
194.225.136.127 knative.dev.isti.ir
```
3. تغییر config-domain configmap

```
apiVersion: v1
data:
  knative.dev.isti.ir: ''
```
4. تغییر allowed_hosts در فایل dev.env

```
ALLOWED_HOSTS=127.0.0.1,localhost,.farayand-microservice.knative.dev.isti.ir,knative.dev.isti.ir
```
مقدار .farayand-microservice.knative.baas.ir یعنی قبل از نقطه میتواند هر چیزی باشد .









