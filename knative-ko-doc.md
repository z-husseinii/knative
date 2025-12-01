## install ko




## install go 


system with internet 
```
wget https://go.dev/dl/go1.22.5.linux-amd64.tar.gz
curl -v -u admin:'nexusadmin12345' --upload-file go1.25.4.linux-amd64.tar.gz  https://repo.xstudioo.ir/repository/moavenat-commands/go1.25.4.linux-amd64.tar.gz
```


system without internet 
```
wget --user=admin --password='nexusadmin12345' https://repo.xstudioo.ir/repository/moavenat-commands/go1.25.4.linux-amd64.tar.gz
sudo tar -C /usr/local -xzf /tmp/go1.22.5.linux-amd64.tar.gz
echo 'export PATH=$PATH:/usr/local/go/bin' >> ~/.bashrc
source ~/.bashrc
go version 
```





## ko errors



---------------------------------------------

```
Error: error processing import paths in "config/jetstream/501-jsm-controller.yaml": error resolving image references: found strict reference but ko://knative.dev/eve ting-natss/cmd/jetstream/controller is not a valid import path: error loading package from knative.dev/eventing-natss/cmd/jetstream/controller: err: exit status 1: st derr: verifying github.com/mxk/go-flowrate@v0.0.0-20140419014527-cca7078d478f/go.mod: github.com/mxk/go-flowrate@v0.0.0-20140419014527-cca7078d478f/go.mod: Get https: //sum.golang.org/lookup/github.com/mxk/go-flowrate@v0.0.0-20140419014527-cca7078d478f: dial tcp: lookup sum.golang.org: Temporary failure in name resolution
```

resolve
GOSUMDB را غیرفعال کن تا Go دنبال sum.golang.org نرود

```
go env -w GOSUMDB=off
go env -w GOPROXY=https://repo.xstudioo.ir/repository/proxy-go
go env | grep -E 'GOPROXY|GOSUMDB'
```

------------------------------------------------
```
go env -w GOPROXY=https://repo.xstudioo.ir/repository/go-proxy root@master-1:/home/worker/knative-eventing/eventing-natss-main# go env -u GOPROXY root@master-1:/home/worker/knative-eventing/eventing-natss-main#
```
مقداری که میخواستم ست نمیشد 
resolve:
```
unset GOPROXY
unset GOPRIVATE
unset GONOPROXY
unset GOSUMDB


go env -w GOPROXY=https://repo.xstudioo.ir/repository/proxy-go
```


```
go env -w GOPROXY=https://repo.xstudioo.ir/repository/go-proxy

```

error:
```
go env -w GOPROXY=https://repo.xstudioo.ir/repository/go-proxy warning: go env -w GOPROXY=... does not override conflicting OS environment variable
```
resolve :
```
unset GOPROXY

go env -w GOPROXY=https://repo.xstudioo.ir/repository/go-proxy
```
----------------------------------------------
```
Error: error processing import paths in "config/webhook/500-webhook-deployment.yaml": error resolving image references: found strict reference but ko://knative.dev/ev enting-natss/cmd/webhook is not a valid import path: error loading package from knative.dev/eventing-natss/cmd/webhook: err: exit status 1: stderr: verifying golang.o rg/x/crypto@v0.37.0/go.mod: golang.org/x/crypto@v0.37.0/go.mod: Get https://sum.golang.org/lookup/golang.org/x/crypto@v0.37.0: dial tcp: lookup sum.golang.org: Tempor ary failure in name resolution
```
باید یه ریپوی دیگه هم اضافه کنم برای sum.golang.org 
چون دو تا شدن باید یه go group repository ساخت و هر دو رو به اون اضافه کرد و متغییر GOPROXY به go group اشاره کند 








----------------------------------------------
```
Error: error processing import paths in "config/jetstream/501-jsm-controller.yaml": error resolving image references: found strict reference but ko://knative.dev/eve ting-natss/cmd/jetstream/controller is not a valid import path: error loading package from knative.dev/eventing-natss/cmd/jetstream/controller: err: exit status 1: st derr: go: github.com/cloudevents/sdk-go/protocol/nats_jetstream/v2@v2.15.2: reading https://repo.xstudioo.ir/repository/go-proxy/github.com/cloudevents/sdk-go/protoco l/nats_jetstream/v2/@v/v2.15.2.mod: 401 Unauthorized
```


resolve 
ریپازیتوری پروکسی که در nexus تعریف کردیم باید بدون احراز هویت کار کند 
setting > security > roles >  annanymous-more > اضافه کردن دسترسی های ریپازیتوری مد نظر 


---------------------------------------------

```
Error: error processing import paths in "config/webhook/500-webhook-deployment.yaml": error resolving image references: fetching base image: pulling gcr.io/distroless /static:nonroot: Get "https://gcr.io/v2/": dial tcp: lookup gcr.io on 127.0.0.53:53: server misbehaving
``` 

resolve 
تغییر base image تا از ریپوی شخصی خودمون بتونه پول کنه 

- پیدا کردن فایلی که base image در آن تعریف شده است 

```
grep -R "gcr.io/distroless" . 
```

تغییر فایل ./.ko.yaml


```
docker-hosted.xstudioo.ir/moavenat/distroless/static:nonroot
```


--------------------------------------------------
```
build knative.dev/eventing-natss/cmd/webhook: cannot load cmp: malformed module path "cmp": missing dot in first path element malformed module path "cmp": missing dot in first path element
```

or 
```
Error: error processing import paths in "config/webhook/500-webhook-deployment.yaml": error resolving image references: build: go build: exit status 1: build knative. dev/eventing-natss/cmd/webhook: cannot load cmp: malformed module path "cmp": missing dot in first path element
```

resolve :
add this line end of go.mod file 

```
replace cmp => github.com/google/go-cmp/cmp  v0.7.0
```

```
go mod tidy
```



-----------------------------------------------
log of jetstream-ch-dispatcher 
```
"Exiting, no JSM servers available"
```



resolve 
nats درست نصب نشده بود و خطای image pull back off داشت 
بعد از بالا اومدن nats در nats-io namespace دیپلویمنت dispatcher هم درست شد 


------------------------------------------------
```
kubectl apply -f natsJETStreamChannel.yaml 
Error from server (NotFound): error when creating "natsJETStreamChannel.yaml": the server could not find the requested resource (post natsjetstreamchannels.messaging. knative.dev)
```

هنگام اجرای  دستور ko apply -R -f ./config کامل اجرا نشده بود 
باید دوباره دستور رو اجرا کرد تا CRD مربوط به NatsJetStreamChannel ساخته شود 

------------------------------------------------


```
NAME READY REASON URL AGE 
natsjetstreamchannel.messaging.knative.dev/channel-defaults False DispatcherNotReady http://channel-defaults-kn-jsm-channel.knative-eventing.svc.cluster.local 89s
```


jetstream-ch-dispatcher  مشکل داره باید خطاش حل بشه تا ready بشه 




