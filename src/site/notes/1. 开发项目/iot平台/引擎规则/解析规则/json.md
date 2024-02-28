---
{"dg-publish":true,"dg-path":"iot平台/引擎规则/解析规则/json.md","permalink":"/iot平台/引擎规则/解析规则/json/","created":"2024-02-26T17:54:38.523+08:00","updated":"2024-02-28T14:14:53.443+08:00"}
---

**v1**

```json
{
    "fields": [ //字段列表
        {
            "type": 3,
            "path": "/data/[1]/deviceId",
            "tag": "deviceId"
        },
        {
            "type": 1,
            "path": "/data/code",
            "tag": "code"
        }
    ]
}
```

**v2**

1. 优化数组取值方式, 直接使用`[]`代表数组
2. 增加snFlag用于标识字段是否为设备id
3. 增加checkValue用于校验报文是否可用, 校验失败, 报文忽略

```json
{
    "fields": [ //字段列表
        {
            "type": 3,
            "path": "/data/[]/deviceId",
            "tag": "deviceId",
            "snFlag": true,
            "checkValue": ""
        },
        {
            "type": 1,
            "path": "/data/code",
            "tag": "code",
            "snFlag": false,
            "checkValue": "200"
        }
    ]
}
```

**20240226**

1. 将snFlag移出字段内部
2. 将checkValue移出字段内部, 改为check, 赋值为对象
3. 增加merge用于报文分包处理, 根据不同的type配置不同的值

```json
{
    "snFlag": "deviceId", //设备唯一标识
    "check": {
        "key": "code", //标识字段
        "value": 200 //校验值
    },
    "merge": {
        "type": 1, //分包类型, 1-mqtt
        "data": {
            "key": "xxx", //key字段
            "index": "xxx", //页码字段
            "total": "xxx", //总数字段
            "check": "xxx" //完整性校验字段
        }
    },
    "fields": [ //字段列表
        {
            "type": 3,
            "path": "/data/deviceId",
            "tag": "deviceId"
        },
        {
            "type": 1,
            "path": "/data/code",
            "tag": "code"
        }
    ]
}
```
