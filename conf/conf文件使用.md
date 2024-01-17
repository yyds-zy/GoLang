## conf.yml 使用

👻👻👻
### 编写yaml
```
mysql:
  addr:
    - 127.0.0.1:3306
    - 127.0.0.2:3306
    - 127.0.0.3:3306
  db: "test"
  user: "admin"
  pwd: "123456A"
  charset: utf8

redis:
  host: "127.0.0.1:6379"
  password: "123456"

elastic_v7:
  addr:
    - "http://127.0.0.1:9200"
  health_check_interval: 10
  gzip: true
  user: elastic
  pass: 123456
```

### 解析yaml
```
type Conf struct {
	Mysql Mysql `yaml:"mysql"`
	Redis Redis `yaml:"redis"`
}

type Mysql struct {
	Addr      []string `yaml:"addr"`
	DevDBName string   `yaml:"db"`
	User      string   `yaml:"user"`
	Pwd       string   `yaml:"pwd"`
	Charset   string   `yaml:"charset"`
}

type Redis struct {
	Host     string `yaml:"host"`
	Password string `yaml:"password"`
}

var GlobalConf Conf
var ConfPath = "./deploy/etc/"

func init() {
	//取消线上用户判断
	if _, err := os.Open(ConfPath); err != nil {
		ConfPath = "../deploy/etc/"
	}
	mgr, err := LoadFromYAML(ConfPath)
	if err != nil {
		panic(err)
	}
	GlobalConf = mgr
}

func LoadFromYAML(cfgFile string) (mgr Conf, err error) {
	var f *os.File
	f, err = os.Open(cfgFile)
	if err != nil {
		return
	}
	decoder := yaml.NewDecoder(f)
	decoder.Decode(&mgr)
	return
}
```

