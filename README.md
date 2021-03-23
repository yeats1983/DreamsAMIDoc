台電DREAMS-AMI-API規格文件

一、 功能說明：

DREAMS需要預測與呈現再生能源的AMI發電相關數值，在獲得台電提供的AMI資料後，預測中心需有對應儲存資料的表格與API提供獲取相關資料的管道，以進行實際/預測資料的呈現或其他相關應用。

二、 API清單說明：

預測中心儲存的AMI資料分為高壓真實資料和從MDMS收到的低壓真實資料，以及預測產出的資料和準確度資料。
| 協定名稱         | 取得實際發電資料  |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 協定描述         | 取得實際發電資料(撈取結果有5萬筆限制)|
| HTTP Method      | GET  |
| URI | http://192.168.0.23:8883/dreams/v1/ami/fact |
| Request 參數說明 | <font color="#E74C3C">[必填]</font>originTime開始時間start: 例如2021-02-10 00:00:00<br><font color="#E74C3C">[必填]</font>originTime結束時間end: 例如2021-02-10 23:59:59<br><font color="#5499C7">[選填]</font>電號customer_id: 例如 14712931068<br><font color="#5499C7">[選填]</font>高低壓high_vol: 例如 false|
|Request 格式範例|http://192.168.0.23:8883/dreams/v1/ami/fact?start=2021-02-21 08:00:00&end=2021-02-21 08:30:00&high_vol=false<br>http://192.168.0.23:8883/dreams/v1/ami/fact?start=2021-02-21 08:00:00&end=2021-02-21 08:30:00&customer_id=14712931068|
|Response 格式說明|{<br>"customerId": "00859500790",<br>"meternumber": "15001310",<br>"originTime": "2021-02-22 00:15:00",<br>"createTime": "2021-02-23 06:01:15",<br>"kwh": 0.0,<br>"kw": 0.0,<br>"factor": 4000.0,<br>"status": "NM",<br>"highVol": true,<br>"isRnbs": true<br>}|
|Response錯誤說明|回傳碼417表示參數格式錯誤或缺少必填欄位<br>回傳碼404表示伺服器不存在，表示API已中斷服務<br>回傳碼400表示API內部發生錯誤|


| 協定名稱 | 取得預測發電資料 | 
| -------- | -------- |
| 協定描述     | 取得預測發電資料(撈取結果有5萬筆限制) |
| HTTP Method     |GET|
| URI |http://192.168.0.23:8883/dreams/v1/ami/predict |
| Request 參數說明 |<font color="#E74C3C">[必填]</font>originTime開始時間start: 例如2021-02-10 00:00:00<br><font color="#E74C3C">[必填]</font>originTime結束時間end: 例如2021-02-10 23:59:59<br><font color="#5499C7">[選填]</font>電號customer_id: 例如 14712931068<br><font color="#5499C7">[選填]</font>高低壓high_vol: 例如false|
|Request 格式範例|http://192.168.0.23:8883/dreams/v1/ami/predict?start=2021-02-22 08:00:00&end=2021-02-22 08:30:00&high_vol=false<br>http://192.168.0.23:8883/dreams/v1/ami/predict?start=2021-02-22 08:00:00&end=2021-02-22 08:30:00&customer_id=14712931068|
|Response 格式說明|{<br>"customerId": "14712931068",<br>"meternumber": "19406255",<br>"originTime": "2021-02-22 08:00:00",<br>"createTime": "2021-02-22 07:54:40",<br>"kwh": 0.29636263846666666,<br>"kw": 3.5563516616,<br>"factor": 1.0,<br>"status": "NM",<br>"highVol": false,<br>"isRnbs": null,<br>"isInputDelay": false<br>}|
|Response錯誤說明|回傳碼417表示參數格式錯誤或缺少必填欄位<br>回傳碼404表示伺服器不存在，表示API已中斷服務<br>回傳碼400表示API內部發生錯誤|


| 協定名稱 | 取得準確度資料 | 
| -------- | -------- |
| 協定描述   | 取得準確度資料(撈取結果有日平均有5萬筆限制，月平均有5萬筆限制)|
|HTTP Method|GET|
|URI|http://192.168.0.23:8883/dreams/v1/metrics/validationIndex|
|Request 參數說明|<font color="#5499C7">[選填]</font>date日期: 例如2021-02-10<br><font color="#5499C7">[選填]</font>電號customerId: 例如 12562300952<br><font color="#5499C7">[選填]</font>表號meternumber: 例如18081240
|Request 格式範例|http://192.168.0.23:8883/dreams/v1/metrics/validationIndex?date=2021-02-22<br>http://192.168.0.23:8883/dreams/v1/metrics/validationIndex?date=2021-02-22 &customerId=12562300952|
|Response 格式說明|{<br>"dailyData":<br>[{<br>"customerId": "12562300952",<br>"meternumber": "18081240",<br>"startTime": "2021-02-22 00:00:00",<br>"endTime": "2021-02-22 23:59:59",<br>"createTime": "2021-02-23 09:13:59",<br>"numberOfPoints": 96,<br>"mape": 0.9928510835009873,<br>"highVol": true<br>}],<br>"monthlyData":<br>[{<br>"customerId": "12562300952",<br>"meternumber": "18081240",<br>"startTime": "2021-01-24 00:00:00",<br>"endTime": "2021-02-22 23:59:59",<br>"createTime": "2021-02-23 09:45:27",<br>"numberOfPoints": 1496,<br>"mape": 0.9853342305523274,<br>"highVol": true<br>}]<br>}|
|Response錯誤說明|回傳碼417表示參數格式錯誤或缺少必填欄位<br>回傳碼404表示伺服器不存在，表示API已中斷服務<br>回傳碼400表示API內部發生錯誤|



