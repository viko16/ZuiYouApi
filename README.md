# ZuiYouApi
最右app API 分析 20151010


## 0. 总览

基于最右 Android 客户端 2.4.4 版本抓包后分析得出

注意：

- 暂时没发现需要带上 `refer` 或 `cookies` 的鉴权
- 已经忽略非必须参数
- 以下分析的 API 大部分都是无需登录状态

> 感谢[最右app](http://izuiyou.com)~~提供~~那么好的内容


## 1. Token
`POST http://tbapi.ixiaochuan.cn/account/register_guest`

#### 参数
__h_did__: 用户手机 mac 地址，可以随便填

#### 示例
```
$ curl -X "POST" "http://tbapi.ixiaochuan.cn/account/register_guest" \
	-d "{\"h_did\":\"123\"}"
```

#### 返回值
```json
{
  "ret": 1,
  "data": {
    "mid": 1851207,
    "pw": "f17a19c4cd1d6834",
    "isnew": 1,
    "token": "5618cac1c4407b72788b456d"
  }
}
```


## 2. 配置项
`POST http://tbapi.ixiaochuan.cn/config/get`

#### 参数
无

#### 示例
```
$ curl -X "POST" "http://tbapi.ixiaochuan.cn/config/get"
```

#### 返回值
```json
{
	"ret": 1,
	"data": {
		"version": 16,
		"config": {
			"imgdns": ["tbfile.ixiaochuan.cn", "115.28.138.114"],
			"apidns": ["tbapi.ixiaochuan.cn", "115.28.177.105"],
			"check_resp_header": "0",
			"tpoicadm_like_times": "2",
			...
		}
	}
}
```


## 3. 关注话题分类列表
`POST http://tbapi.ixiaochuan.cn/topic/attention_list`

#### 参数
__token__:  鉴权密码

__offset__: 分页，非必须，默认0

#### 示例
```
$ curl -X "POST" "http://tbapi.ixiaochuan.cn/topic/attention_list" \
	-d "{\"offset\":0,\"token\":\"5618c8fbc4407b31748b4573\"}"
```

#### 返回值
```json
{
  "ret": 1,
  "data": {
    "total": 6,
    "list": [
      {
        "id": 105549,
        "topic": "#每天为生活拍张照#",
        "cover": 7321408,
        "posts": 4375,
        "partners": 84153,
        "atts": 410968,
        "addition": "410968人关注",
        "share": -1,
        "ut": 1444466895,
        "newcnt": 2,
        "top": 0,
        "attinfo": {
          "up": 0
        }
      },
      ...
    ]
  }
}
```

## 4. 全部话题列表
`POST http://tbapi.ixiaochuan.cn/topic/recommend_new`

#### 参数
__h_dt__: 未知参数，默认0

__offset__: 分页，非必须，默认0

#### 示例
```
$ curl -X "POST" "http://tbapi.ixiaochuan.cn/topic/recommend_new" \
	-d "{\"h_dt\":0,\"offset\":0}"
```

#### 返回值
```json
{
  "ret": 1,
  "data": {
    "more": 1,
    "offset": 20,
    "list": [
      {
        "id": 102250,
        "topic": "#我来问你来答#",
        "cover": 8659344,
        "posts": 5704,
        "partners": 214563,
        "atts": 48337,
        "addition": "48337人关注",
        "share": -1,
        "ut": 1444465242
      },
      ...
    ]
  }
}
```

## 5. 话题详细（帖子列表
`POST http://tbapi.ixiaochuan.cn/topic/detail`

#### 参数
__tid__: 话题id，从列表获取

__h_dt__: 未知参数，默认0

#### 示例
```
curl -X "POST" "http://tbapi.ixiaochuan.cn/topic/detail" \
	-d "{\"tid\":100198,\"h_dt\":0}"
```

#### 返回值
```json
{
  "ret": 1,
  "data": {
    "topic": {
      "id": 100198,
      "topic": "#汪了个喵#",
      "cover": 7877329,
      "posts": 4623,
      "partners": 665375,
      "atts": 43794,
      "addition": "43794人关注",
      "share": -1,
      "ut": 1444461138,
      "brief": "你猜我是汪，是喵？我奏是这么的可爱，分享一切萌萌的狗狗和喵，来这里找一些一起爱这些小可爱的人吧。请大家随时关注本周热门栏哟～😘🐶🐰🐮🐨🐒🐳🐤🐠🐥小动物俱乐部"
    },
    "more": 1,
    "t": 1444391233,
    "list": [
      {
        "id": 2225448,
        "mid": 1054391,
        "content": "喵星人的小背影 像小天使一样 萌化了 🐱",
        "likes": 4,
        "up": 4,
        "ct": 1444461138,
        "imgs": [
          {
            "id": 8918291,
            "w": 440,
            "h": 293,
            "fmt": "jpeg"
          },
          ...
        ],
        "share": 0,
        "member": {
          "id": 1054391,
          "isreg": 1,
          "name": "醐荼荼",
          "gender": 2,
          "avatar": 970793,
          "sign": "我即将放弃做一个有趣的人"
        },
        "topic": {
          "id": 100198,
          "topic": "#汪了个喵#",
          "cover": 7877329,
          "posts": 4623,
          "partners": 665043,
          "atts": 43786,
          "addition": "43786人关注",
          "share": -1,
          "ut": 1444461138
        }
      },
      ...
    ],
    "attedusers": [
      {
        "id": 794447,
        "isreg": 1,
        "name": "り﹏。小小小亲爱、",
        "gender": 2,
        "avatar": 965092,
        "sign": "😊^_^😊",
        "trank": 1,
        "up": 124084,
        "isadm": 1
      },
      ...
    ],
    "attedcnt": 43794,
    "hotstamp": 1444233600,
    "hasnewhot": 1
  }
}
```

## 6. 帖子列表
`POST http://tbapi.ixiaochuan.cn/topic/posts`

#### 参数
___tid___: 话题id，从列表获取

__h_dt__: 未知参数，默认为0

__offset__: 分页，默认0

__t__: 时间戳，貌似会跟 offset 关联

#### 示例
```
curl -X "POST" "http://tbapi.ixiaochuan.cn/topic/posts" \
	-d "{\"offset\":60,\"tid\":100198,\"t\":1444365904,\"h_dt\":0}"
```

### 返回值
跟`topic/detail"`差不多...


## 7. 图片获取
`GET http://tbfile.ixiaochuan.cn/img/view/id/{imgs.id}`

__images.id__: 在列表中获取

> 顺便总结一下缩略图规则，再后面补 /sz/{size}
- `src` 或 `0` 为原图
- `228` 多图情况下的缩略图（宽度压缩成228px
- `480x120` 单图情况下的缩略图（宽度或高度压缩
- 其他值为一定大小的缩略图

#### 示例
```
$ curl -X "GET" "http://tbfile.ixiaochuan.cn/img/view/id/8916463/sz/src"
```

## 8. 视频获取
`GET http://tbfile.ixiaochuan.cn/img/mp4/id/{mp4.id}`

__mp4.id__: 在列表中获取

#### 示例
```
$ curl -X "GET" "http://tbfile.ixiaochuan.cn/img/mp4/id/8916085"
```


## X. 未完待续
