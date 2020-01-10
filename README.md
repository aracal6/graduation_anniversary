## 毕业纪念册APP


### 价值主张设计


#### 加值宣言
- 毕业是青春难以避免的事情，是人生难以忘记的一段时光，人们都想把这段时光永远的保留下来。所以人们用图像的形式来保存下回忆
- 但是随着时代的发展，毕业相册的传统纸张形式已经不能满足现如今用户的需求，人们需要更多智能化的交互来丰富自己的生活。智能毕业纪念册的出现满足了用户社交的智能化需求。

###### 优先级：api接口实现人脸识别，回忆旧时好友。人脸识api返回信息值，如情绪，表情等，供用户回忆当时欢乐情景。
###### 次优先级：场景识别回忆旧时美好时光，识别地标，提供地点及路线，帮助想故地重游的用户。



#### 核心价值
本产品致力于给用户回忆美好的最佳体验，人脸识别的情绪、表情等信息返回勾起用户当初的情绪感受，加上地标识别的地点反馈，让用户从精神上身临其境般的体验旧时美好。云分享满足用户当代的社交需求。

#### 核心价值与用户痛点
- 人脸信息反馈
用户忘记当初的自己与同学之间的情绪状态，用表情、情绪的反馈来最大程度地帮助用户回忆

- 场景识别地点
用户回忆不起当时拍照的具体位置，场标识别api识别出当初拍照的地点并联合地图导航给用户规划路线。

- 电子相册
用户之前的老照片早已丢失或保存损坏，电子版相册能够方便用户储存并且不会像传统照片一样损坏

- 云分享功能
用户使用传统相册，携带不方便，不能实时与好友分享在照片上的感悟与想法。云分享功能支持照片、语音、文字上传，给用户一个新的社交方式。

#### 需求列表与人工智能API加值
|   用户需求  |  API接口 | 重要程度 |
| --- | --- |--- |
| 回忆照片中的情感  |百度人脸识别api |重要 | 
| 回到当初拍照的地方寻找青春 | 地标识别api| 重要| 

#### api调用
- 百度--人脸识别
输入
```
import requests
import pprint
import json

def baidu_api(image):
    
    rdata={
        'grant_type':'client_credentials',
        'client_id':'WmpfQoLhZ5x3RGSZixLzKOGt',
        'client_secret':'NtzU3nnLrageTLZLqRwuvzx48hpkSmrs'
    }
   
    r=requests.post('https://aip.baidubce.com/oauth/2.0/token',data=rdata).json()
    access_token=r['access_token']
    api_url='https://aip.baidubce.com/rest/2.0/face/v3/detect'+'?access_token='+access_token
    
    Header={
        'Content-Type':'application/json'
    }
    data={ 'image':image,
        'image_type':'URL',
        'face_field':'gender,age,emotion',
        'max_face_num':10,
         }
    r=requests.post(api_url,data=data).json()
    return r

baidu_api('https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1568634172081&di=1501f8a43f3a099910a9f27f8368bdb6&imgtype=0&src=http%3A%2F%2Fi0.hdslb.com%2Fbfs%2Farticle%2F9c3112ad34d2e010d8d137b88ca0f14704308cbd.jpg')
```
输出
```
{'error_code': 0,
 'error_msg': 'SUCCESS',
 'log_id': 747956986453063891,
 'timestamp': 1568645306,
 'cached': 0,
 'result': {'face_num': 3,
  'face_list': [{'face_token': '5a93616d60d0c629c72125ee093e2ccd',
    'location': {'left': 635.87,
     'top': 156.77,
     'width': 107,
     'height': 101,
     'rotation': -35},
    'face_probability': 1,
    'angle': {'yaw': 26.36, 'pitch': 1.9, 'roll': -36.92},
    'gender': {'type': 'male', 'probability': 1},
    'age': 32,
    'emotion': {'type': 'neutral', 'probability': 0.9}},
   {'face_token': 'c24acdea30cf419320a252abbea5ef06',
    'location': {'left': 316.39,
     'top': 186.63,
     'width': 102,
     'height': 103,
     'rotation': 39},
    'face_probability': 1,
    'angle': {'yaw': -17.96, 'pitch': 6.16, 'roll': 37.74},
    'gender': {'type': 'male', 'probability': 1},
    'age': 30,
    'emotion': {'type': 'happy', 'probability': 0.77}},
   {'face_token': '35eadcd732ab669f8ab3a25696afad63',
    'location': {'left': 446.89,
     'top': 145.46,
     'width': 101,
     'height': 92,
     'rotation': -6},
    'face_probability': 1,
    'angle': {'yaw': 37.79, 'pitch': 22.37, 'roll': -20.33},
    'gender': {'type': 'male', 'probability': 1},
    'age': 31,
    'emotion': {'type': 'happy', 'probability': 0.9}}]}}
```
- 地标识别api
输入
```
model_url = vision_base_url + "models"
headers   = {'Ocp-Apim-Subscription-Key': subscription_key}
models    = requests.get(model_url, headers=headers).json()
[model["name"] for model in models["models"]]
image_url = "https://upload.wikimedia.org/wikipedia/commons/f/f6/" + \
    "Bunker_Hill_Monument_2005.jpg"
landmark_analyze_url = vision_base_url + "models/landmarks/analyze"
print(landmark_analyze_url)
headers  = {'Ocp-Apim-Subscription-Key': subscription_key}
params   = {'model': 'landmarks'}
data     = {'url': image_url}
response = requests.post(landmark_analyze_url, headers=headers, params=params, json=data)
response.raise_for_status()

analysis      = response.json()
print(json.dumps(response.json()))
assert analysis["result"]["landmarks"] is not []

landmark_name = analysis["result"]["landmarks"][0]["name"].capitalize()
image = Image.open(BytesIO(requests.get(image_url).content))
plt.imshow(image)
plt.axis("off")
_ = plt.title(landmark_name, size="x-large", y=-0.1)
```
输出
```
https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/models/landmarks/analyze
{"result": {"landmarks": [{"name": "Bunker Hill Monument", "confidence": 0.976844847202301}]}, "requestId": "eabc4892-07dc-4d80-b48f-4b657d3b8fae", "metadata": {"width": 1200, "height": 1600, "format": "Jpeg"}}
```
#### 价值主张练习
##### 一句话版本
- 智能毕业纪念册，用情感分析与地标识别，帮助你回忆青春，回到过去。

##### 一分钟版本
- 智能毕业纪念册使用电子的形式，帮助用户减去传统相册保存难，易丢失的烦恼。人脸识别技术用数值反馈，给用户情感上最大程度的回忆，回想当初的欢声笑语。地标识别api能够给用户识别照片中的场景，并规划好路线，给用户提供最优化的方案。回到青春的地方打卡，我们说走就走。同时相册还支持云分享，旧时好友加入相册组，分享更多照片，满足现代社会的社交需求。
