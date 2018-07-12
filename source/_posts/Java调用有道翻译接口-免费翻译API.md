---
title: Java调用有道翻译接口 - 免费翻译API
date: 2018-07-12 23:32:22
tags: java
---

这个接口为免费的

### 一、接口地址
等号后面的为需要翻译的英文
```
http://fanyi.youdao.com/openapi.do?keyfrom=xinlei&key=759115437&type=data&doctype=json&version=1.1&q=hisen
```

### 二、代码样例
把全世界200+国家和地区的名字翻译为英文，并且入库
```
  @Autowired
  SmsCountryService countryService;

  @Test
  public void updateZhName(){
    // 有道翻译接口
    String url = "http://fanyi.youdao.com/openapi.do?keyfrom=xinlei&key=759115437&type=data&doctype=json&version=1.1&q=";
    // 查询出所有的英文国家名字
    List<SmsCountry> countries = countryService.queryAllName();
    // httpclient
    CloseableHttpClient client = HttpClientBuilder.create().build();
    // 翻译每个国家的名字，并且更新数据库
    for (SmsCountry country:countries) {
      HttpGet request = new HttpGet(url + URLEncoder.encode(country.getName()));
      try {
        CloseableHttpResponse response = client.execute(request);
        String str = EntityUtils.toString(response.getEntity(), "utf-8");
        JSONObject jsonObject = JSON.parseObject(str);
        // 取出json字符串中数组的值
        String s = (String) jsonObject.getJSONArray("translation").get(0);
        System.out.println(">>>>> " + s);
        if (!StringUtil.isEmpty(s)){
        SmsCountry smsCountry = new SmsCountry();
        smsCountry.setId(country.getId());
        smsCountry.setNameZh(s);
        countryService.updateNameById(smsCountry);
        }
      } catch (IOException e) {
        e.printStackTrace();
      }
    }
  }
```
