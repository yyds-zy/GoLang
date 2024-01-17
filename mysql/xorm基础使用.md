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
