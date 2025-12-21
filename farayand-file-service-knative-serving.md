## test
```
curl -v -H "Host: file-service.farayand-microservice.knative.dev.isti.ir" http://knative.dev.isti.ir/upload-file/ --form 'field="contract_documents"' --form 'file=@"/home/zahra/Downloads/farayand-image.jpeg"'
*   Trying 194.225.136.127:80...
* Connected to knative.dev.isti.ir (194.225.136.127) port 80 (#0)
> POST /upload-file/ HTTP/1.1
> Host: file-service.farayand-microservice.knative.dev.isti.ir
> User-Agent: curl/7.81.0
> Accept: */*
> Content-Length: 7334
> Content-Type: multipart/form-data; boundary=------------------------d84f3211a7f0201f
> 
* We are completely uploaded and fine
* Mark bundle as not supporting multiuse
< HTTP/1.1 200 OK
< allow: POST, OPTIONS
< content-length: 95
< content-type: application/json
< cross-origin-opener-policy: same-origin
< date: Sun, 21 Dec 2025 09:20:36 GMT
< referrer-policy: same-origin
< server: envoy
< vary: Accept, origin, Cookie
< x-content-type-options: nosniff
< x-frame-options: DENY
< x-envoy-upstream-service-time: 43
< 
* Connection #0 to host knative.dev.isti.ir left intact
{"id":"6947bbe4154f2c5d3f53a50f","message":"اطلاعات با موفقیت ذخیره شد."}
```



### errors
error 1
```
curl -v -H "Host: file-service.farayand-microservice.knative.dev.isti.ir" http://knative.dev.isti.ir/upload-file/ --form 'field="contract_documents"' --form 'file=@"/home/zahra/Downloads/farayand-image.jpeg"'
```
error in files-service log:
```
 form to point to file-service.farayand-microservice.knative.dev.isti.ir/upload-file/ (note the trailing slash), or set APPEND_SLASH=False in
our Django settings.
```
resolve :
باید بعد از upload-file / گذاشت 





error2
```
kubectl logs formal-evaluation-service-00001-deployment-6bdfc745b5-wfgm8 -n farayand-microservice
6bdfc745b5-wfgm8 -n farayand-microservice Traceback (most recent call last): File "/usr/local/lib/python3.11/logging/config.py", line 573, in configure handler = self.configure_handler(handlers[name]) ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ File "/usr/local/lib/python3.11/logging/config.py", line 757, in configure_handler result = factory(**kwargs) ^^^^^^^^^^^^^^^^^ File "/usr/local/lib/python3.11/logging/handlers.py", line 155, in __init__ BaseRotatingHandler.__init__(self, filename, mode, encoding=encoding, File "/usr/local/lib/python3.11/logging/handlers.py", line 58, in __init__ logging.FileHandler.__init__(self, filename, mode=mode, File "/usr/local/lib/python3.11/logging/__init__.py", line 1181, in __init__ StreamHandler.__init__(self, self._open()) ^^^^^^^^^^^^ File "/usr/local/lib/python3.11/logging/__init__.py", line 1213, in _open return open_func(self.baseFilename, self.mode, ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ FileNotFoundError: [Errno 2] No such file or directory: '/app/logs/outgoing_requests.log' The above exception was the direct cause of the following exception: Traceback (most recent call last): File "/app/manage.py", line 25, in <module> main() File "/app/manage.py", line 21, in main execute_from_command_line(sys.argv) File "/usr/local/lib/python3.11/site-packages/django/core/management/__init__.py", line 442, in execute_from_command_line utility.execute() File "/usr/local/lib/python3.11/site-packages/django/core/management/__init__.py", line 416, in execute django.setup() File "/usr/local/lib/python3.11/site-packages/django/__init__.py", line 19, in setup configure_logging(settings.LOGGING_CONFIG, settings.LOGGING) File "/usr/local/lib/python3.11/site-packages/django/utils/log.py", line 76, in configure_logging logging_config_func(logging_settings) File "/usr/local/lib/python3.11/logging/config.py", line 823, in dictConfig dictConfigClass(config).configure() File "/usr/local/lib/python3.11/logging/config.py", line 580, in configure raise ValueError('Unable to configure handler ' ValueError: Unable to configure handler 'outgoing_request_file'
```
resolve 
اضافه کردن خط زیر به داکر فایل 
```
RUN mkdir -p /app/logs
```
error3
```
kubectl logs file-service-00003-deployment-69d69fc44-f59sg -n farayand-microservice
AttributeError: 'Settings' object has no attribute 'ROOT_URLCONF' [2025-12-20 08:23:54 -0600] [10] [ERROR] Error handling request /upload-file Traceback (most recent call last): File "/usr/local/lib/python3.11/site-packages/gunicorn/workers/sync.py", line 135, in handle self.handle_request(listener, req, client, addr) File "/usr/local/lib/python3.11/site-packages/gunicorn/workers/sync.py", line 178, in handle_request respiter = self.wsgi(environ, resp.start_response) ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ File "/usr/local/lib/python3.11/site-packages/django/core/handlers/wsgi.py", line 124, in __call__ response = self.get_response(request) ^^^^^^^^^^^^^^^^^^^^^^^^^^ File "/usr/local/lib/python3.11/site-packages/django/core/handlers/base.py", line 139, in get_response
```
resolve 
add this lines to Dockerfile 
```
ENV ENV_FILE=dev.env
ENV DJANGO_SETTINGS_MODULE=core.settings.dev
```
add to core/settings/base.py
```
ROOT_URLCONF = "core.urls"
```
core/settings/dev.py
```
from .base import *

DEBUG = True
```
core/settings/prod.py
```
from .base import *

DEBUG = False
```
error 4
```
File "/usr/local/lib/python3.11/site-packages/django/views/decorators/csrf.py", line 65, in _view_wrapper return view_func(request, *args, **kwargs) ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ File "/usr/local/lib/python3.11/site-packages/django/views/generic/base.py", line 105, in view return self.dispatch(request, *args, **kwargs) ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ File "/usr/local/lib/python3.11/site-packages/rest_framework/views.py", line 515, in dispatch response = self.handle_exception(exc) ^^^^^^^^^^^^^^^^^^^^^^^^^^ File "/usr/local/lib/python3.11/site-packages/rest_framework/views.py", line 475, in handle_exception self.raise_uncaught_exception(exc) File "/usr/local/lib/python3.11/site-packages/rest_framework/views.py", line 486, in raise_uncaught_exception raise exc File "/usr/local/lib/python3.11/site-packages/rest_framework/views.py", line 512, in dispatch response = handler(request, *args, **kwargs) ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ File "/app/apps/views.py", line 57, in post if not serializer.is_valid(): ^^^^^^^^^^^^^^^^^^^^^ File "/usr/local/lib/python3.11/site-packages/rest_framework/serializers.py", line 225, in is_valid self._validated_data = self.run_validation(self.initial_data) ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ File "/usr/local/lib/python3.11/site-packages/rest_framework/serializers.py", line 447, in run_validation value = self.validate(value) ^^^^^^^^^^^^^^^^^^^^ File "/app/apps/serializers.py", line 40, in validate raise serializers.ValidationError({field: self.ERROR_MESSAGES['invalid_file_type']['fa']}) ~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^
```
علت خطا این است که فرمت فایل مدنظر نامعتبر است اما نباید این خطارا بدهد و باید کد درست شود 

error 5
```
curl -v -H "Host: file-service.farayand-microservice.knative.dev.isti.ir" http://knative.dev.isti.ir/upload-file/ --form 'field="contract_documents"' --form 'file=@"/home/zahra/Downloads/images.jpg"'
   Trying 194.225.136.127:80...
 Connected to knative.dev.isti.ir (194.225.136.127) port 80 (#0)
> POST /upload-file/ HTTP/1.1
> Host: file-service.farayand-microservice.knative.dev.isti.ir
> User-Agent: curl/7.81.0
> Accept: /
> Content-Length: 2182
> Content-Type: multipart/form-data; boundary=------------------------6981197d2ac1ff6e
> 
 We are completely uploaded and fine
 Mark bundle as not supporting multiuse
< HTTP/1.1 400 Bad Request
< allow: POST, OPTIONS
< content-length: 70
< content-type: application/json
< cross-origin-opener-policy: same-origin
< date: Sun, 21 Dec 2025 08:36:51 GMT
< referrer-policy: same-origin
< server: envoy
< vary: Accept, origin, Cookie
< x-content-type-options: nosniff
< x-frame-options: DENY
< x-envoy-upstream-service-time: 12
< 
* Connection #0 to host knative.dev.isti.ir left intact
{"contract_documents":["فرمت تصویر باید JPEG باشد."]}
```

error 6
```

curl -v -H "Host: file-service.farayand-microservice.knative.dev.isti.ir" http://knative.dev.isti.ir/upload-file/ --form 'field="contract_documents"' --form 'file=@"/home/zahra/Downloads/farayand-image.jpeg"'
*   Trying 194.225.136.127:80...
* Connected to knative.dev.isti.ir (194.225.136.127) port 80 (#0)
> POST /upload-file/ HTTP/1.1
> Host: file-service.farayand-microservice.knative.dev.isti.ir
> User-Agent: curl/7.81.0
> Accept: */*
> Content-Length: 7334
> Content-Type: multipart/form-data; boundary=------------------------d7570ab777cb0425
> 
* We are completely uploaded and fine
* Mark bundle as not supporting multiuse
< HTTP/1.1 500 Internal Server Error
< content-length: 90182
< content-type: text/html; charset=utf-8
< cross-origin-opener-policy: same-origin
< date: Sun, 21 Dec 2025 08:44:31 GMT
< referrer-policy: same-origin
< server: envoy
< vary: origin, Cookie
< x-content-type-options: nosniff
< x-frame-options: DENY
< x-envoy-upstream-service-time: 49
```

```
kubectl logs file-service-00011-deployment-6b54d6f577-92xsz -n farayand-microservice
Internal Server Error: /upload-file/
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
  File "/app/apps/views.py", line 79, in post
    result = self.file_service.insert_file_meta_data(file_meta_data)
             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/app/apps/services/file_meta_data_service.py", line 57, in insert_file_meta_data
    raise Exception(DB_MESSAGES["save_failed"])
Exception: ذخیره اطلاعات با مشکل مواجه شد
```


resolve :
```
mongosh -u mms-automation -p HOOoAvEFDr3bRT68T4mP
use file_service_database
db.createUser(
  {
    user: "application_user",
    pwd:  "HOOoAvEFDr3bRT68T4mP",   
    roles: [ { role: "readWrite", db: "file_service_database" }]
  }
)
```
