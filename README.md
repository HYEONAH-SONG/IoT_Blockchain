# IoT BlockChain API 

### Getting Started

Before application apply you must download Django 

```
pip install django
pip install djangorestframework
```

Execute the following command to execute

```
python manage.py runserver
```

<hr>

- #### IoT_Blockchain/setting.py

```
INSTALLED_APPS = [
    'rest_framework',
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'blockchain.apps.BlockchainConfig',
]
```

- #### IoT_Blockchain/urls.py

```
from django.contrib import admin
from django.urls import path
import blockchain.views

urlpatterns = [
    path('admin/', admin.site.urls),
    path('block/', blockchain.views.IoT_BlockChain_block.as_view())
]
```

- #### blockchain/view.py

  - [GET 구현] 해당 채널 ID을 가진 블록들 정보 읽기 API
  - [POST 구현] 채널명, 센서와 기기 값, 타임 스탬프를 기반으로 블록 생성 API

```
    # 블록 생성 API
    def post(self, request):
        global block

        body_unicode = request.body.decode('utf-8')
        body = json.loads(body_unicode)

        req_timestamp = body['time']
        req_channel_name = body['channel_name']
        req_sensor_val = body['sensor_val']
        req_service_val = body['service_val']

        block.addBlock(Block(len(block.chain), req_timestamp, req_channel_name, 		  req_sensor_val, req_service_val))

        # 마지막 블록
        for recent_block in block.chain:
            continue

        return Response(json.dumps(vars(recent_block)), status=200)

    # 해당 채널의 블록 정보 읽기 API
    def get(self, request):
        get_channel = request.GET.get('channel_name') # get은 para로 받을 것
        send_block = []

        for find_block in block.chain:
            json_find_block = json.dumps(vars(find_block))
            dict_block = json.loads(json_find_block)

            if dict_block["channel_name"] == get_channel:
                send_block.append(json.dumps(vars(find_block)))

        if len(send_block) == 0:
            return Response("There is no block in this channel", status=200)
        return Response(json.dumps(send_block), status=200)
```

- #### block.json

```
{
    "satisfaction": "",
    "previous_block_id": "",
    "current_block_id": "",
    "timestamp": "",
    "channel_id": "",
    "sensor_val": "",
    "service_val": "",
    "runtime": ""
}
```

