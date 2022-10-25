# Golang 实现 Apollo 

本文以 Golang 语言为例，Github 提供了一些客户端的包给开发者调用：

- [philchia/agollo](https://github.com/philchia/agollo)

## 示例

- 开始连接Apollo客户端
```gotemplate
apollo := agollo.New(&agollo.Conf{
        AppID:          "your app id",
        Cluster:        "default",
        NameSpaceNames: []string{"application.properties"},
        MetaAddr:       "your apollo meta addr",
    })

apollo.Start()
```

从源码`github.com\philchia\agollo\v4\agollo.go`来看，"philchia/agollo"还提供以下方法：
```gotemplate
var (
	once          sync.Once
	defaultClient Client
)

// Start agollo client with Conf, start will block until fetch all config to local
func Start(conf *Conf, opts ...ClientOption) error {
	once.Do(func() { defaultClient = NewClient(conf, opts...) })
	return defaultClient.Start()
}

// Stop sync config
func Stop() error {
	return defaultClient.Stop()
}

// OnUpdate get all updates
func OnUpdate(handler func(*ChangeEvent)) {
	defaultClient.OnUpdate(handler)
}

// SubscribeToNamespaces fetch namespace config to local and subscribe to updates
func SubscribeToNamespaces(namespaces ...string) error {
	return defaultClient.SubscribeToNamespaces(namespaces...)
}

// GetString get value from given namespace
func GetString(key string, opts ...OpOption) string {
	return defaultClient.GetString(key, opts...)
}

// GetNameSpaceContent get contents of namespace
func GetContent(opts ...OpOption) string {
	return defaultClient.GetContent(opts...)
}

// GetPropertiesContent for properties namespace
func GetPropertiesContent(opts ...OpOption) string {
	return defaultClient.GetPropertiesContent(opts...)
}

// GetAllKeys return all config keys in given namespace
func GetAllKeys(opts ...OpOption) []string {
	return defaultClient.GetAllKeys(opts...)
}

// GetReleaseKey return release key for namespace
func GetReleaseKey(opts ...OpOption) string {
	return defaultClient.GetReleaseKey(opts...)
}
```