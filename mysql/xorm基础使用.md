## xorm使用

[xorm官方文档](https://xorm.io/docs/index.html)
### 是什么？
**XORM 是一个 Go 语言编写的 ORM 库，支持 MySQL、PostgreSQL、SQLite、Oracle、SQL Server、TiDB、ClickHouse 等数据库。它提供了一套简单易用的 API，可以让你在 Go 语言中轻松地操作数据库**

### 引入xorm库
go get github.com/go-xorm/xorm

### 引入mysql驱动库
go get github.com/go-sql-driver/mysql

### 声明数据库表的结构体
```
type KfTransferConfig struct {
	Id          int32     `xorm:"not null pk autoincr id"`
	Bid         int32     `xorm:"bid"`
	Opened      int32     `xorm:"opened"`       // 1:开启, 2:未开启
	Type        int32     `xorm:"type"`         // 1:找客服意图转人工, 2:富媒体内容转人工, 3:无答案转人工, 4:默认, 5:自定义
	CatalogueID uint64    `xorm:"catalogue_id"` // 技能id
	Catalogue   string    `xorm:"catalogue"`    // 技能名
	Config      string    `xorm:"config"`       // 转人工提示语
	CreatedAt   time.Time `xorm:"created_at"`
	UpdatedAt   time.Time `xorm:"updated_at"`
	DeletedAt   time.Time `xorm:"deleted_at"`
}
```
**结构体名字就是表名字以大写字母开头**

**声明结构体一定要设置一个主键钮！！！不然后面的查询修改都不会起效！pk**

### 创建orm引擎组
```
var engine *xorm.EngineGroup  // 声明orm引擎组

func GetEngine() *xorm.EngineGroup {
	return engine
}

const DBConfigTemplate = "%v:%v@tcp(%v)/%v?charset=%v&loc=Local&parseTime=true"

func InitMysqlConnection() error {
        // 获取DB配置
        user := conf.GlobalConf.Mysql.User
	pwd := conf.GlobalConf.Mysql.Pwd
	devDBName := conf.GlobalConf.Mysql.DevDBName
	onlineDBName := conf.GlobalConf.Mysql.OnlineDBName
	charset := conf.GlobalConf.Mysql.Charset

	// 组装devDB和onlineDB配置
	var devDBConfig []string
	var onlineDBConfig []string
	for _, addr := range conf.GlobalConf.Mysql.Addr {
		devDBConfig = append(devDBConfig, fmt.Sprintf(DBConfigTemplate, user, pwd, addr, devDBName, charset))
		onlineDBConfig = append(onlineDBConfig, fmt.Sprintf(DBConfigTemplate, user, pwd, addr, onlineDBName, charset))
	}

	var err error
	engine, err = connectDevDB(devDBConfig)
	if err != nil {
		return err
	}
	onlineEngine, err = connectOnlineDB(devDBConfig)
	if err != nil {
		return err
	}
	return nil
}

unc connectDevDB(devDBConfig []string) (*xorm.EngineGroup, error) {
	xormEngine, err := xorm.NewEngineGroup("mysql", devDBConfig)
	if err != nil {
		return nil, err
	}
	return xormEngine, nil
}

func connectOnlineDB(onlineDBConfig []string) (*xorm.EngineGroup, error) {
	xormEngine, err := xorm.NewEngineGroup("mysql", onlineDBConfig)
	if err != nil {
		return nil, err
	}
	return xormEngine, nil
}

```

### 使用orm引擎组

