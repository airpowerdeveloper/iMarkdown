## 验证响应

要验证请求返回的数据，您可以`pm.response`在测试中使用该对象。使用`pm.test`函数定义测试，提供名称和返回布尔值（`true`或`false`）的函数，指示测试是否通过。参考文档：```https://learning.postman.com/docs/writing-scripts/script-references/test-examples/#using-multiple-assertions```

## 解析响应主体数据

要对响应执行断言，您首先需要将数据解析为断言可以使用的 JavaScript 对象。

要解析 JSON 数据，请使用以下语法：

```js
const responseJson = pm.response.json();
```

要解析 XML，请使用以下命令：

```js
const responseJson = xml2Json(pm.response.text());
```

> 如果您正在处理复杂的 XML 响应，您可能会发现[控制台日志记录](https://learning.postman.com/docs/sending-requests/troubleshooting-api-requests/#using-the-console)很有用。

要解析 CSV，请使用[CSV 解析](https://github.com/adaltas/node-csv/tree/master/packages/csv-parse)实用程序：

```js
const parse = require('csv-parse/lib/sync');
const responseJson = parse(pm.response.text());
```

要解析 HTML，请使用[Cheerio](https://cheerio.js.org/)：

```js
const $ = cheerio.load(pm.response.text());
//output the html for testing
console.log($.html());
```

### 处理无法解析的响应

如果您无法将响应正文解析为 JavaScript，因为它的格式不是 JSON、XML、HTML、CSV 或任何其他可解析的数据格式，您仍然可以对数据进行断言。

测试响应正文是否包含字符串：

```js
pm.test("Body contains string",() => {
  pm.expect(pm.response.text()).to.include("customer_id");
});
```

```javascript
pm.test("断言返回数据中是否包含微信特有字段openId,nickname", function () {
    pm.expect(pm.response.text()).to.include("openId");
    pm.expect(pm.response.text()).to.include("nickname");
});

pm.test("断言返回的json数据中是否有fileName字段", function () {
    pm.response.to.have.jsonBody("data.fileName");        
});

tests["断言返回的json数据中是否有fileName字段"] =responseBody.has("fileName");
```

这不会告诉您在哪里遇到该字符串，因为它对整个响应正文进行测试。测试响应是否与字符串匹配（通常仅对短响应有效）：

```js
pm.test("Body is string", function () {
  pm.response.to.have.body("whole-body-text");
});
```

## 对 HTTP 响应进行断言

您的测试可以检查请求响应的各个方面，包括正文、状态代码、标头、cookie、响应时间等。

### 测试响应体

检查响应正文中的特定值：

```js
pm.test("Person is Jane", () => {
  const responseJson = pm.response.json();
  pm.expect(responseJson.name).to.eql("Jane");
  pm.expect(responseJson.age).to.eql(23);
});
```

```javascript
tests["断言模板下载链接是否以//storage.jd.com/nirvana.templates/custom/开头"] = pm.response.json().data.fileName.has("//storage.jd.com/nirvana.templates/custom/");

pm.test("断言响应正文是否包含字符串deliveryId且以JDV开头", function () {
    pm.expect(pm.response.text()).to.include("JDV");
});

pm.test("断言返回中code的值是否是0", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData.code).to.eql(0);
});

pm.test("断言notSellerUser是否是商家工作台的用户, 等于true表示不是商家工作台的用户，一旦不是商家工作台用户，将不再返回其他属性", function () {
    var jsonData = pm.response.json();
    console.log(jsonData.userInfo.notSellerUser);
    pm.expect(jsonData.userInfo.notSellerUser).to.eql(false);
});

const jsonData = pm.response.json();
pm.test("断言data值是否等于53283", () => {
      (m => m.data === 53283);
      //data array should include 53283
      pm.expect(jsonData.data).to.include(53283);
      //data array should include all listed
      pm.expect(jsonData.data).to.have.members([53283]);
});

var jsonData = JSON.parse(responseBody);
console.log(jsonData);
tests["断言返回中code的值是否是0"] = jsonData.code===0;

pm.test("断言code是否为0", () => {
  pm.expect(pm.response.json().code).to.be.oneOf([0]);
});
```

### 测试状态代码

测试响应状态代码：

```js
pm.test("Status code is 201", () => {
  pm.response.to.have.status(201);
});
```

```javascript
pm.test("断言 API 返回的响应代码。如果响应码为200，则测试通过，否则失败。", function () {
    pm.response.to.have.status(200);
});
```

如果您想测试状态代码是否为一组状态代码，请将它们全部包含在一个数组中并使用`oneOf`：

```js
pm.test("Successful POST request", () => {
  pm.expect(pm.response.code).to.be.oneOf([201,202]);
});
```

检查状态码文本：

```js
pm.test("Status code name has string", () => {
  pm.response.to.have.status("Created");
});
```

### 测试标头

检查响应标头是否存在：

```js
pm.test("Content-Type header is present", () => {
  pm.response.to.have.header("Content-Type");
});
```

测试具有特定值的响应标头：

```js
pm.test("Content-Type header is application/json", () => {
  pm.expect(pm.response.headers.get('Content-Type')).to.eql('application/json');
});
```

### 测试cookie

测试响应中是否存在 cookie：

```js
pm.test("Cookie JSESSIONID is present", () => {
  pm.expect(pm.cookies.has('JSESSIONID')).to.be.true;
});
```

测试特定 cookie 值：

```js
pm.test("Cookie isLoggedIn has value 1", () => {
  pm.expect(pm.cookies.get('isLoggedIn')).to.eql('1');
});
```

### 测试响应时间

测试响应时间是否在指定范围内：

```js
pm.test("Response time is less than 200ms", () => {
  pm.expect(pm.response.responseTime).to.be.below(200);
});
```

## 常见断言示例

### 针对变量断言响应值

检查响应属性是否与变量（在本例中为环境变量）具有相同的值：

```js
pm.test("Response property matches environment variable", function () {
  pm.expect(pm.response.json().name).to.eql(pm.environment.get("name"));
});
```

> 有关可用于操作脚本中变量的操作的概述，请参阅使用[变量。](https://learning.postman.com/docs/sending-requests/variables/)

### 断言值长度

```javascript
pm.test("断言msg长度是否为4", () => {
    //parse the response json and test three properties
    const responseJson = pm.response.json();
    pm.expect(responseJson.msg).to.have.lengthOf(4);
});
```

### 断言值类型

测试响应任何部分的类型：

```js
/* response has this structure:
{
  "name": "Jane",
  "age": 29,
  "hobbies": [
    "skating",
    "painting"
  ],
  "email": null
}
*/
const jsonData = pm.response.json();
pm.test("Test data type of the response", () => {
  pm.expect(jsonData).to.be.an("object");
  pm.expect(jsonData.name).to.be.a("string");
  pm.expect(jsonData.age).to.be.a("number");
  pm.expect(jsonData.hobbies).to.be.an("array");
  pm.expect(jsonData.website).to.be.undefined;
  pm.expect(jsonData.email).to.be.null;
});
```

```javascript
pm.test("断言值类型", () => {
    console.log("揽收超时/派送超时列表查询有可能没有异常数据，所以严格匹配返回字段类型");
    console.log("code是否为number");
    pm.expect(pm.response.json().code).to.be.a("number");
    console.log("contents是否为array");
    pm.expect(pm.response.json().data.contents).to.be.an("array");
    console.log("curPage是否为number");
    pm.expect(pm.response.json().data.curPage).to.be.a("number");
    console.log("curPageSize是否为number");
    pm.expect(pm.response.json().data.curPageSize).to.be.a("number");
    console.log("nextPage是否为number");
    pm.expect(pm.response.json().data.nextPage).to.be.a("number");
    console.log("pageSize是否为number");
    pm.expect(pm.response.json().data.pageSize).to.be.a("number");
    console.log("prePage是否为number");
    pm.expect(pm.response.json().data.prePage).to.be.a("number");
    console.log("start是否为number");
    pm.expect(pm.response.json().data.start).to.be.a("number");
    console.log("totalPage是否为number");
    pm.expect(pm.response.json().data.totalPage).to.be.a("number");
    console.log("totalRow是否为number");
    pm.expect(pm.response.json().data.totalRow).to.be.a("number");
    console.log("msg是否为string");
    pm.expect(pm.response.json().msg).to.be.a("string");
});

pm.test("断言name类型是否为string", function () {
    var jsonData = pm.response.json();
    pm.expect(typeof(jsonData.data.name)).to.eql("string");
});
```

### 断言数组属性

检查数组是否为空，以及是否包含特定项目：

```js
/*
response has this structure:
{
  "errors": [],
  "areas": [ "goods", "services" ],
  "settings": [
    {
      "type": "notification",
      "detail": [ "email", "sms" ]
    },
    {
      "type": "visual",
      "detail": [ "light", "large" ]
    }
  ]
}
*/

const jsonData = pm.response.json();
pm.test("Test array properties", () => {
    //errors array is empty
  pm.expect(jsonData.errors).to.be.empty;
    //areas includes "goods"
  pm.expect(jsonData.areas).to.include("goods");
    //get the notification settings object
  const notificationSettings = jsonData.settings.find
      (m => m.type === "notification");
  pm.expect(notificationSettings)
    .to.be.an("object", "Could not find the setting");
    //detail array must include "sms"
  pm.expect(notificationSettings.detail).to.include("sms");
    //detail array must include all listed
  pm.expect(notificationSettings.detail)
    .to.have.members(["email", "sms"]);
});
```

> 顺序`.members`不影响测试。

```javascript
const jsonData = pm.response.json();
pm.test("断言是否包含EXPRESS_WAYBILL运单管理", () => {
    //detail array should include "EXPRESS_DELIVERY_SINGLE"
    pm.expect(pm.response.json().data).to.include("EXPRESS_DELIVERY_SINGLE");
    //detail array should include all listed
    pm.expect(pm.response.json().data).to.have.members(["EXPRESS_DELIVERY_SINGLE", "EXPRESS_WAYBILL","PERSONALCENTER_CONFIGURATION_VERIFICATION",
        "EXPRESS_QUERY_INQUIRE",
        "EXPRESS_WAYBILL_MODIFY",
        "EXPRESS_DELIVERY_BATCH",
        "INTELLIGENT_GUIDE_SHEET_IMPORT",
        "EXPRESS_DELIVERY_PRINTING",
        "EXPRESS_WAYBILL_LETTERHEAD",
        "PERSONALCENTER_QRCODE_PREMADEQRCODE"]);
});

pm.test("断言可拦截验证data为空数组", () => {
    //data array is empty
    pm.expect(pm.response.json().data).to.be.empty;
});

const jsonData = pm.response.json();
pm.test("断言data数组属性不为空", () => {
    //data array is not empty
  pm.expect(jsonData.data).to.be.not.empty;
});

pm.test("断言数组data中的parentNam是否包含特定值北京市", () => {
  const notificationData = jsonData.data.find
      (m => m.parentName === "北京市");
});
```

### 断言对象属性

断言对象包含键或属性：

```js
pm.expect({a: 1, b: 2}).to.have.all.keys('a', 'b');
pm.expect({a: 1, b: 2}).to.have.any.keys('a', 'b');
pm.expect({a: 1, b: 2}).to.not.have.any.keys('c', 'd');
pm.expect({a: 1}).to.have.property('a');
pm.expect({a: 1, b: 2}).to.be.an('object')
  .that.has.all.keys('a', 'b');
```

> 目标可以是`object`、`set`、`array`或`map`。如果在没有or`.keys`的情况下运行，则表达式默认为。由于行为因目标而异，建议在使用with之前检查。`.all``.any``.all``.keys``type``type``.keys``.a`

### 断言某个值在集合中

根据有效选项列表检查响应值：

```js
pm.test("Value is in valid list", () => {
  pm.expect(pm.response.json().type)
    .to.be.oneOf(["Subscriber", "Customer", "User"]);
});
```

```javascript
pm.test("断言内容标题contentTitle在一个集合中", () => {
  pm.expect(pm.response.json().data.itemList[0].contentTitle)
    .to.be.oneOf(["新疆(sendProvince)", "新疆(receiveProvince)","长岛(receiveCounty)", "西藏(receiveProvince","黄山(receiveCity)","海南(receiveProvince)"]);
});
```

### 断言包含一个对象

检查对象是否是父对象的一部分：

```js
/*
response has the following structure:
{
  "id": "d8893057-3e91-4cdd-a36f-a0af460b6373",
  "created": true,
  "errors": []
}
*/

pm.test("Object is contained", () => {
  const expectedObject = {
    "created": true,
    "errors": []
  };
  pm.expect(pm.response.json()).to.deep.include(expectedObject);
});
```

使用导致链中后续的`.deep`所有`.equal`、`.include`、`.members`、`.keys`和断言使用深度相等（松散相等）而不是严格 ( ) 相等。虽然也松散比较，但会导致深度相等比较也可用于链中后续的任何其他断言，而 while 则不会。`.property``===``.eql``.deep.equal``.eql`

```javascript
pm.test("断言对象是否是父对象的一部分", () => {
  const expectedObject = {
    "contentAbstract": "寄往新疆或从新疆发出商品限制规则<br/>1. 法律法规禁运物品类：菜刀、剪刀、餐刀、园艺剪刀、切片刀、斧、镰、铲、刻刀、裁纸刀、螺丝刀、改锥、钳子、尖嘴钳、美工刀、工兵锹、手工锯、锥、带刺或倒刺类针、棍、棒、手枪钻、冲击钻、钻头、钢条、锯条、电锯、切割机、角磨机、手术刀、剪刀、针头、注射器、冰刀、刮皮器、修眉刀、指甲刀（剪指刀）、剃须刀片、玩具枪（含水枪）、模型枪、道具枪、软弹枪、水弹枪、声光枪、棒球棒、双节棍、武术用棍、弓、弩、箭、弹弓、飞镖、钢珠、塑料子弹、健身钢球、玩具炸弹、炸弹模型、无人机、遥控飞机、航空模型、大型风筝、动力三角翼、动力伞、飞艇、滑翔伞、孔明灯、轻型或超轻型飞机、轻型直升机、热气球、三角翼、无人驾驶自由气球、系留气球、猪肉及其制品肉干、腊肉、冷鲜肉、U盘、硬盘、光盘、SD卡、直接存储用途的设备、各类针具、复方地芬诺酯片、外文书籍、其他不可辨认内容的书籍印刷品、弹弓。<br/>2. 生鲜、水果、食品类：猪肉及其制品肉干、腊肉、冷鲜肉。<br/>3. 日用品类：U盘、硬盘、光盘、SD卡、直接存储用途的设备。<br/>4. 医药、器械类：各类针具、复方地芬诺酯片。",
    "contentCode": "banner01537",
    "id": 1667,
    "linkType": 0
  };
  pm.expect(pm.response.json().data.itemList[0]).to.deep.include(expectedObject);
});
```

### 断言当前环境

检查 Postman 中活动的（当前选定的）环境：

```js
pm.test("Check the active environment", () => {
  pm.expect(pm.environment.name).to.eql("Production");
});
```

### 断言包含业务逻辑

```javascript
if (pm.response.json().msg==="调用成功") {
    console.log('msg为调用成功时，则pin有默认地址');
    pm.test("判断用户pin是否是jdjd停不住的浪", function () {
    var jsonData = pm.response.json();
    console.log(jsonData.data[0].josPin);
    pm.expect(jsonData.data[0].josPin).to.eql("jdjd停不住的浪");
    });
} else if (pm.response.json().msg==="未查询到默认地址") {
    console.log('msg为未查询到默认地址时，则pin没有默认地址');
    pm.test("断言code是否为0", () => {
    pm.expect(pm.response.json().code).to.be.oneOf([0]);
    });
    pm.test("断言msg是否为未查询到默认地址", function () {
    pm.expect(pm.response.json().msg).to.eql("未查询到默认地址");
    });
}
```

## 设置变量

```javascript
var interceptWayBillDeliveryId= pm.response.json().data.deliveryId;
console.log(interceptWayBillDeliveryId);
//设置全局变量，用于将interceptWayBillDeliveryId向下传
postman.setGlobalVariable("deliveryId",interceptWayBillDeliveryId);
```

## 预请求脚本

### 设置变量

```javascript
//通用语法
postman.setGlobalVariable("key","value");//设置全局变量
postman.setEnvironmentVariable("key","value"); //说置环境变量 
//postman native app特有语法
pm.globals.get("key","value");//设置全局变量
pm.environment.get("key","value");//设置环境变量
```

### 获取变量

```javascript
// 通用语法
postman.getGlobalVariable("key"); //获取全局变量
postman.getEnvironmentVariable("key"); // 获取环境交量
//postman native app特有语法
pm.globals.get("key");//获取全局变量
pm.environment.get("key");//获取环境变量
```

### 清除变量

```javascript
// 通用语法
postman.clearGlobalVariable("key");//消除全局变量
postman.clearEnvironmentVariable("key"); //清除环境交量
//postman native app特有语法
pm.globals.unset("key");//消除全局变量
pm.environment.unset("key");//清除环境变量
```

### 将数组，嵌套对象储存到全局变量、环境变量中

有时候，我们要将数组，嵌套对象，json 对象储存到环境变量，全局变量，可以使用 postman 提供的 JSON 工具类，将其储存为 json 格式的字符串，如下：

```javascript
//将教组储存到环境变量中
var array = [1, 2, 3, 4];
postman.setEnvironmentVariable("array", JSON.stringify(array));

//将嵌套对象储存到环境变量中
var obj = {a:[1,2,3,4],b:{c:'val'}};
postman.setEnvironmentVariable("obj", JSON.stringify(obj));

//从环境变量中获职数组对象
var array = JSON.parse(postman.getEnvironmentVariable("array"));

// 从环境变量中获取嵌套对象/json对象
var obj=JSON.parse(postman.getEnvironmentVariable("obj"));
```

### 添加cookie

```javascript
pm.request.headers.add({
    key:'cookie',
    value:'pinId=wNuolHUCCOyXCReQK5NYU9RdzdvKdIq8; pin=jdjd%E5%81%9C%E4%B8%8D%E4%BD%8F%E7%9A%84%E6%B5%AA; unick=jdjd%E5%81%9C%E4%B8%8D%E4%BD%8F%E7%9A%84%E6%B5%AA; _tp=4KEOjDLtceeIN9VuJtm985kmqGm9JhYLRAkolPOy0Olxkrew7ovDuklMDzCsNgmXLskdfg4hGoegRv8DXGC4PQ%3D%3D; _pst=jdjd%E5%81%9C%E4%B8%8D%E4%BD%8F%E7%9A%84%E6%B5%AA; ceshi3.com=000; logining=1; ssa.ticket=BJ.16B0D3BDA9F5BA5C1EA56DAE9D209C447320211117095902S; TrackID=1gH-7OwyhCb2KgLa3YHTtxpJYVzqzXDK0Sw-y99P7i1_wK6VbYzNTDoEf4gHc-42i_ZqrrVQuq-GmFcCQVV2cWRfzXBTiYPKgDAYb4Yqx85TvqYIa3Nkn7PO8wPZeF0FI; thor=7B856AE4B1D4B3803DB54AC2B1294A7CBFED7A2D73E66EFEB278E7004BCD6C6FC75C84C2902746A1F30C7B0A9C5F9638D09EAF1A8AD71FB75E1BBE3E77AD5CCC5A9F43DE47CAA4290A87B4E3C0C4F516DE357DCEF44B2C015060C9FE2B55AD70EFA73513CD9221B2CE5AB50DF48EAE59BFF9487B1D91A31E23D58B6359FF2FD8B78251030308C23114D692E7CC56341F'
})
```

### 时间相关

```javascript
// 引用第三方库 ：moment 等同于 import moment from 'moment'
var moment = require('moment');
// 调用 moment()方法获取当前时间，并进行格式化，用变量 endTime 接收
var endTime = moment().format("YYYY-MM-DD H:mm:ss");
// 打印当前时间的值到控制台 
console.log("当前时间endTime：" + endTime);
postman.setGlobalVariable("endTime",endTime);
// 调用 moment()方法获取当前时间，并进行格式化，用变量 beginTime 接收
var beginTime = moment().subtract(7,"days").format("YYYY-MM-DD H:mm:ss");
// 打印当前时间-7d的值到控制台 
console.log("7天前的时间beginTimee：" + beginTime);
postman.setGlobalVariable("beginTime",beginTime);
var pickUpEndTimeForAddWaybillSXTH = moment().add(1, "h").add(1, "d").format("YYYY-MM-DD");
// 打印当前时间+1d1h的值到控制台 
console.log("1日1小时后的时间pickUpEndTimeForAddWaybillSXTH：" + pickUpEndTimeForAddWaybillSXTH);
postman.setGlobalVariable("pickUpEndTimeForAddWaybillSXTH", pickUpEndTimeForAddWaybillSXTH);
var pickUpStartTimeForAddWaybillSXTH = moment().add(1, "d").format("YYYY-MM-DD");
// 打印当前时间+1d的值到控制台 
console.log("1日后的时间pickUpStartTimeForAddWaybillSXTH：" + pickUpStartTimeForAddWaybillSXTH);
postman.setGlobalVariable("pickUpStartTimeForAddWaybillSXTH", pickUpStartTimeForAddWaybillSXTH);
```
