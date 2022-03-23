# gua-bao
Gin best practices, gin development scaffolding

 web api 開發基礎包
 使用 gin 建構基礎腳手架，主要功能有下：
1.  請求鏈路日誌打印，涵蓋mysql/redis/request
2.  支持多語言錯誤信息提示及自定義錯誤提示。
3.  支持了多配置環境
4.  封裝了 log/redis/mysql/http.client 常用方法
5.  支持swagger文檔生成

### 開始
- 安裝依賴
```
go mod tidy
```

- 確保正確配置 conf/mysql_map.toml、conf/redis_map.toml：

- 運行腳本
```
go run main.go
```

### 測試
創建測試表：
```
CREATE TABLE `area` (
 `id` bigint(20) NOT NULL AUTO_INCREMENT,
 `area_name` varchar(255) NOT NULL,
 `city_id` int(11) NOT NULL,
 `user_id` int(11) NOT NULL,
 `update_at` datetime NOT NULL,
 `create_at` datetime NOT NULL,
 `delete_at` datetime NOT NULL,
 PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8 COMMENT='area';
INSERT INTO `area` (`id`, `area_name`, `city_id`, `user_id`, `update_at`, `create_at`, `delete_at`) VALUES (NULL, 'area_name', '1', '2', '2019-06-15 00:00:00', '2019-06-15 00:00:00', '2019-06-15 00:00:00');
```

測試 demo api
```
curl 'http://127.0.0.1:8880/demo/dao?id=1'
```
收到回覆大致如下
```
{
    "errno": 0,
    "errmsg": "",
    "data": "[{\"id\":1,\"area_name\":\"area_name\",\"city_id\":1,\"user_id\":2,\"update_at\":\"2019-06-15T00:00:00+08:00\",\"create_at\":\"2019-06-15T00:00:00+08:00\",\"delete_at\":\"2019-06-15T00:00:00+08:00\"}]",
    "trace_id": "c0a8fe445d05b9eeee780f9f5a8581b0"
}
```

查看Log
檔案位置：log/gin_scaffold.inf.log
正常情況下可以看到已寫入鏈路日誌

### 文件分層
```
├── README.md
├── conf            配置文件夾
│   └── dev
│       ├── base.toml
│       ├── mysql_map.toml
│       └── redis_map.toml
├── controller      控制器
│   └── demo.go
├── dao             dao
│   └── demo.go
├── docs            swagger文件層
├── dto             dto
│   └── demo.go
├── go.mod
├── go.sum
├── main.go         入口
├── middleware      中間件層
│   ├── panic.go
│   ├── response.go
│   ├── token_auth.go
│   └── translation.go
├── public          公共文件
│   ├── log.go
│   ├── mysql.go
│   └── validate.go
└── router          路由
│   ├── httpserver.go
│   └── route.go
└── services        邏輯處理層
```
層次劃分
控制層 --> 邏輯層 --> db數據層

### swagger 文件生成
修改api資訊後，執行生成
```
swag init
```
啟動服務後
瀏覽地址：http://127.0.0.1:8880/swagger/index.html


### log / redis / mysql / http.client 常用方法

參考文檔：https://github.com/e421083458/golang_common

### 參考來源
https://github.com/e421083458/gin_scaffold
