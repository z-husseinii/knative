## test 



### errors
error
```
197 static files copied to '/app/staticfiles'.
sh: 1: gunicorn: not found
```

resolve 
add gunicorn==21.2.0 to requrements.txt 



error
```
197 static files copied to '/app/staticfiles'.
[2025-12-21 11:24:34 +0000] [9] [INFO] Starting gunicorn 21.2.0
[2025-12-21 11:24:34 +0000] [9] [INFO] Listening at: http://0.0.0.0:8002 (9)
[2025-12-21 11:24:34 +0000] [9] [INFO] Using worker: sync
[2025-12-21 11:24:34 +0000] [10] [INFO] Booting worker with pid: 10
[2025-12-21 05:24:50,185] INFO request_trace: {"type": "request", "timestamp": "2025-12-21T11:24:50.184845+00:00", "trace_id": "formal-ed6e52d598ab", "method": "GET",
 "path": "/brokerages/", "query_params": {}, "client_ip": "10.42.133.250", "user_agent": "curl/7.81.0", "body": null}
[2025-12-21 05:24:50,839] ERROR django.request: Internal Server Error: /brokerages/
Traceback (most recent call last):
  File "/usr/local/lib/python3.11/site-packages/django/core/handlers/exception.py", line 55, in inner
    response = get_response(request)
               ^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/site-packages/django/core/handlers/base.py", line 197, in _get_response
    response = wrapped_callback(request, *callback_args, **callback_kwargs)
               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/site-packages/django/views/decorators/csrf.py", line 65, in _view_wrapper
    return view_func(request, *args, **kwargs)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/site-packages/django/views/generic/base.py", line 105, in view
    return self.dispatch(request, *args, **kwargs)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/site-packages/rest_framework/views.py", line 515, in dispatch
    response = self.handle_exception(exc)
               ^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/site-packages/rest_framework/views.py", line 475, in handle_exception
    self.raise_uncaught_exception(exc)
  File "/usr/local/lib/python3.11/site-packages/rest_framework/views.py", line 486, in raise_uncaught_exception
:
raise exc
  File "/usr/local/lib/python3.11/site-packages/rest_framework/views.py", line 512, in dispatch
    response = handler(request, *args, **kwargs)
               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/app/applications/views.py", line 97, in get
    data = json.loads(response.text, object_hook=json_util.object_hook)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/json/__init__.py", line 359, in loads
    return cls(**kw).decode(s)
           ^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/json/decoder.py", line 337, in decode
    obj, end = self.raw_decode(s, idx=_w(s, 0).end())
               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/json/decoder.py", line 355, in raw_decode
    raise JSONDecodeError("Expecting value", s, err.value) from None
json.decoder.JSONDecodeError: Expecting value: line 1 column 1 (char 0)
[2025-12-21 05:24:50,841] ERROR request_trace: {"type": "response", "timestamp": "2025-12-21T11:24:50.841725+00:00", "trace_id": "formal-ed6e52d598ab", "method": "GET
", "path": "/brokerages/", "status_code": 500, "duration_ms": 656.92, "response_body": null}
[2025-12-21 05:24:51,984] INFO request_trace: {"type": "request", "timestamp": "2025-12-21T11:24:51.984671+00:00", "trace_id": "formal-0e2a5348d17b", "method": "GET",
 "path": "/brokerages/", "query_params": {}, "client_ip": "192.168.60.17", "user_agent": "curl/7.81.0", "body": null}
[2025-12-21 05:24:52,027] ERROR django.request: Internal Server Error: /brokerages/
Traceback (most recent call last):
  File "/usr/local/lib/python3.11/site-packages/django/core/handlers/exception.py", line 55, in inner
    response = get_response(request)
               ^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/site-packages/django/core/handlers/base.py", line 197, in _get_response
    response = wrapped_callback(request, *callback_args, **callback_kwargs)
               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/site-packages/django/views/decorators/csrf.py", line 65, in _view_wrapper
    return view_func(request, *args, **kwargs)
       ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/site-packages/django/views/generic/base.py", line 105, in view
    return self.dispatch(request, *args, **kwargs)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/site-packages/rest_framework/views.py", line 515, in dispatch
    response = self.handle_exception(exc)
               ^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/site-packages/rest_framework/views.py", line 475, in handle_exception
    self.raise_uncaught_exception(exc)
  File "/usr/local/lib/python3.11/site-packages/rest_framework/views.py", line 486, in raise_uncaught_exception
    raise exc
  File "/usr/local/lib/python3.11/site-packages/rest_framework/views.py", line 512, in dispatch
    response = handler(request, *args, **kwargs)
               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/app/applications/views.py", line 97, in get
    data = json.loads(response.text, object_hook=json_util.object_hook)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/json/__init__.py", line 359, in loads
    return cls(**kw).decode(s)
           ^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/json/decoder.py", line 337, in decode
    obj, end = self.raw_decode(s, idx=_w(s, 0).end())
               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/json/decoder.py", line 355, in raw_decode
    raise JSONDecodeError("Expecting value", s, err.value) from None
json.decoder.JSONDecodeError: Expecting value: line 1 column 1 (char 0)
```
resolve :

change  FILE_SERVICE_BASE_URL, APPLICATION_SERVICE_URL, TECHNICAL_EVALUATION_SERVICE_URL in dev.env file



error
```
[2025-12-21 05:43:12,239] INFO request_trace: {"type": "request", "timestamp": "2025-12-21T11:43:12.238980+00:00", "trace_id": "formal-1a5dc9db7f7a", "method": "GET"
 "path": "/brokerages/", "query_params": {}, "client_ip": "10.42.133.250", "user_agent": "curl/7.81.0", "body": null}
[2025-12-21 05:43:12,261] ERROR django.request: Internal Server Error: /brokerages/
Traceback (most recent call last):
  File "/usr/local/lib/python3.11/site-packages/django/core/handlers/exception.py", line 55, in inner
    response = get_response(request)
               ^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/site-packages/django/core/handlers/base.py", line 197, in _get_response
    response = wrapped_callback(request, *callback_args, **callback_kwargs)
               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/site-packages/django/views/decorators/csrf.py", line 65, in _view_wrapper
    return view_func(request, *args, **kwargs)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/site-packages/django/views/generic/base.py", line 105, in view
    return self.dispatch(request, *args, **kwargs)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/site-packages/rest_framework/views.py", line 515, in dispatch
    response = self.handle_exception(exc)
               ^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/site-packages/rest_framework/views.py", line 475, in handle_exception
    self.raise_uncaught_exception(exc)
  File "/usr/local/lib/python3.11/site-packages/rest_framework/views.py", line 486, in raise_uncaught_exception
    raise exc
  File "/usr/local/lib/python3.11/site-packages/rest_framework/views.py", line 512, in dispatch
    response = handler(request, *args, **kwargs)
               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/app/applications/views.py", line 97, in get
    data = json.loads(response.text, object_hook=json_util.object_hook)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/json/__init__.py", line 359, in loads
```
resolve :
قبل از متغییر APPLICATION_SERVICE_URL یک اسپیس اضافه هست که باید پاک شود

error:
```
[2025-12-21 05:49:49,554] ERROR django.request: Internal Server Error: /brokerages/
Traceback (most recent call last):
  File "/usr/local/lib/python3.11/site-packages/django/core/handlers/exception.py", line 55, in inner
    response = get_response(request)
               ^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/site-packages/django/core/handlers/base.py", line 197, in _get_response
    response = wrapped_callback(request, *callback_args, **callback_kwargs)
               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/site-packages/django/views/decorators/csrf.py", line 65, in _view_wrapper
    return view_func(request, *args, **kwargs)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/site-packages/django/views/generic/base.py", line 105, in view
    return self.dispatch(request, *args, **kwargs)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/site-packages/rest_framework/views.py", line 515, in dispatch
    response = self.handle_exception(exc)
               ^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/site-packages/rest_framework/views.py", line 475, in handle_exception
    self.raise_uncaught_exception(exc)
  File "/usr/local/lib/python3.11/site-packages/rest_framework/views.py", line 486, in raise_uncaught_exception
    raise exc
  File "/usr/local/lib/python3.11/site-packages/rest_framework/views.py", line 512, in dispatch
    response = handler(request, *args, **kwargs)
               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/app/applications/views.py", line 97, in get
    data = json.loads(response.text, object_hook=json_util.object_hook)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/json/__init__.py", line 359, in loads
    return cls(**kw).decode(s)
           ^^^^^^^^^^^^^^^^^^^
 File "/usr/local/lib/python3.11/json/decoder.py", line 337, in decode
    obj, end = self.raw_decode(s, idx=_w(s, 0).end())
               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.11/json/decoder.py", line 355, in raw_decode
    raise JSONDecodeError("Expecting value", s, err.value) from None
json.decoder.JSONDecodeError: Expecting value: line 1 column 1 (char 0)
[2025-12-21 05:49:49,555] ERROR request_trace: {"type": "response", "timestamp": "2025-12-21T11:49:49.555654+00:00", "trace_id": "formal-984be9498e84", "method": "GET
", "path": "/brokerages/", "status_code": 500, "duration_ms": 29.54, "response_body": null}
```

resolve :
chnge code application/view.py
```
def get(self, request):
        query_params = request.GET

        filter = {}
        if "_id" in query_params:
            filter["_id"] = query_params["_id"]

        sort = {}
        if "sort_by" in query_params and "order" in query_params:
            order_map = {"asc": 1, "desc": -1}
            sort = query_params["sort_by"]
            order = query_params["order"]
            sort = {sort: order_map[order]}

        page, page_size = None, None
        if "page" in query_params and "page_size" in query_params:
            page = query_params["page"]
            page_size = query_params["page_size"]
            try:
                page = int(page)
                page_size = int(page_size)
            except ValueError:
                return Response({"error": "Invalid page/page_size values"}, status=status.HTTP_400_BAD_REQUEST)

        response = call_db_service("findSpecificDocument", {
            "database": config('MONGO_DB_NAME'),
            "collection": "brokerages",
            "filter": filter,
            "sort": sort,
            "page": page,
            "page_size": page_size
        })



        if response is None:
            return Response({"error": DB_MESSAGES["general_error"]}, status=status.HTTP_503_SERVICE_UNAVAILABLE)

        if response.status_code != 200:
            return Response({"error": DB_MESSAGES["get_failed"]}, status=status.HTTP_500_INTERNAL_SERVER_ERROR)


        try:
            data = json.loads(response.text, object_hook=json_util.object_hook)
        except ValueError:
            return Response(
                {
                    "error": "پاسخ سرویس دیتابیس JSON معتبر نیست",
                    "raw_response": response.text,
                },
                status=status.HTTP_502_BAD_GATEWAY
            )

        cleaned_results = [{
            "_id": str(item["_id"]),
            "name": item.get("name")
        } for item in data["results"]]
        serializer = BrokerageListSerializer(data=cleaned_results, many=True)
        serializer.is_valid(raise_exception=True)
        data["results"] = serializer.validated_data

        
        return Response(data)
       
```


error
```
curl -v -H "Host: formal-evaluation-service.farayand-microservice.knative.dev.isti.ir" http://knative.dev.isti.ir/brokerages/
500 internal error 
```
logs of database in baas ns 
```
 "POST /mongodbfindSpecificDocument HTTP/1.1" 404 - 
```
resolve:
در env.dev انتهای DB_SERVICE_URL باید / گذاشت 
جا انداخته بودم



error
```
 raise OperationFailure(errmsg, code, response, max_wire_version)
pymongo.errors.OperationFailure: Received authentication for mechanism SCRAM-SHA-1 which is not enabled, full error: {'ok': 0.0, 'errmsg': 'Received authentication for mechanism SCRAM-SHA-1 which is not enabled', 'code': 334, 'codeName': 'MechanismUnavailable', '$clusterTime': {'clusterTime': Timestamp(1766325338, 1), 'signature': {'hash': b'7a8M;\n\xadmko*\xbd\x12\x92\x17\xfff\x18V\xe1', 'keyId': 7549944330408951815}}, 'operationTime': Timestamp(1766325338, 1)}
```


resolve :
باید در connection-string مقدار uthMechanism= را تغییر داد 
```
connection_string = "mongodb://application_user:HOOoAvEFDr3bRT68T4mP@mongodb-instance-svc.mongodb-operator.svc.cluster.local:27017/application_service_database?authSource=application_service_database&authMechanism=SCRAM-SHA-256"
```
