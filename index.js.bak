const express = require('express');
const bodyParser = require('body-parser');
const request = require('request');
const querystring = require('querystring');

const app = express();

app.all('*', function (req, res, next) {
  res.header('Access-Control-Allow-Origin', '*');
  res.header('Access-Control-Allow-Headers', 'Content-Type, Content-Length, Authorization, Accept, X-Requested-With , yourHeaderFeild');
  res.header('Access-Control-Allow-Methods', 'PUT, POST, GET, DELETE, OPTIONS');
  if (req.method == 'OPTIONS') {
      res.send(200);
  } else {
      next();
  }
});

// parse application/x-www-form-urlencoded
app.use(bodyParser.urlencoded({ extended: false }));

// parse application/json
app.use(bodyParser.json());

app.get('/', (req, res) => {
  res.send('hello world!');
});


let codeCheck;
app.post('/send_back',(req,res)=>{
  console.log(codeCheck);
  if(req.body.code == codeCheck){
    res.json({success:1})
  }else{
    res.json({success:0})
  }
})





app.post('/sms_send', (req, res) => {
  // console.log(req.body.phone);
  let code = ('000000' + Math.floor(Math.random() * 999999)).slice(-6);
  codeCheck = code;
  var queryData = querystring.stringify({
    mobile: req.body.phone, // 接受短信的用户手机号码
    tpl_id: req.body.tpl_id, // 您申请的短信模板ID，根据实际情况修改
    tpl_value: `#code#=${code}`, // 您设置的模板变量，根据实际情况修改
    key: req.body.key // 应用APPKEY(应用详细页查询)
  });

  var queryUrl = 'http://v.juhe.cn/sms/send?' + queryData;

  request(queryUrl, function(error, response, body) {
    if (!error && response.statusCode == 200) {
      console.log(body); // 打印接口返回内容
      var jsonObj = JSON.parse(body); // 解析接口返回的JSON内容
      res.json(jsonObj);
    } else {
      console.log('请求异常');
    }
  });
});

const port = process.env.PORT || 5000;

app.listen(port, () => {
  console.log(`Server is running on port ${port}`);
});
