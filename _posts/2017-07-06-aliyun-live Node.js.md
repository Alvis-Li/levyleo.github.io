---
layout: post
title: 阿里云直播签名机制 Node.js
---

[阿里云直播API](https://help.aliyun.com/document_detail/48207.html?spm=5176.doc29951.6.573.4GiSdl)

主要难处在签名算法：[签名机制](https://help.aliyun.com/document_detail/50286.html?spm=5176.doc48207.6.579.77LDu7)

直接上代码：

```
    const param = requestData;
    let query = Object.assign({}, ApiPublicParam, param);
    let sortQuery = {}
    let pureSortQuery = []
    for (let key of Object.keys(query).sort()) {
      let valueStr = query[key] + '';
      valueStr = valueStr.replace(/:/g, '%3A');
      valueStr = valueStr.replace(/\//g, '%2F');
      valueStr = valueStr.replace(/{/g, '%7B');
      valueStr = valueStr.replace(/}/g, '%7D');
      valueStr = valueStr.replace(/,/g, '%2C');
      let value = utility.encodeURIComponent(valueStr);
      key = utility.encodeURIComponent(key);
      sortQuery[key] = value
      pureSortQuery.push(key + "=" + value);
    }

    let queryStr = pureSortQuery.join("&");
    let stringToSign = config.Method + '&%2F&' + queryStr.replace(/=/g, "%3D").replace(/&/g, "%26");
    let signature = crypto.createHmac('sha1', config.AccessKeySecret + '&');
    let signatureStr = signature.update(new Buffer(stringToSign, 'utf8')).digest('base64');
    let aliUrl = config.GateWay;
    let fullParam = Object.assign(query, {
      Signature: utility.encodeURIComponent(signatureStr),
      Timestamp: query.Timestamp
    })
    let paramList = [];
    for (let key of Object.keys(fullParam)) {
      let value = fullParam[key] + "";
      if (value.match(/:/g)) {
        value = value.replace(/:/g, '%3A');
      } else if (value.match(/\//g) || value.match(/{/g) || value.match(/,/g)) {
        value = utility.encodeURIComponent(value);
      }
      paramList.push(key + "=" + value);
    }
    let paramStr = paramList.join('&');
    let realUrl = aliUrl + '?' + paramStr;

```

URL 鉴权

```
 authKey: function(URI) { //URL鉴权计算逻辑
    URI = url.parse(URI).pathname;
    let Timestamp = moment().unix();
    let rand = 0;
    let uid = 0;
    let PrivateKey = ''; //(即开通鉴权功能时填写的鉴权KEY)
    let sstring = URI + '-' + Timestamp + '-' + rand + '-' + uid + '-' + PrivateKey;
    let HashValue = md5(sstring)
    return {
      auto_key: HashValue
    };
  }
```

待补充；[具体代码](https://github.com/levyleo/AliLive)
