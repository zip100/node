* 测试用例 curd\_test.go 

> 注：测试用例文件名称必须以 `_test.go` 结尾

```go
package curd_test

import(
    "fmt"
    "testing"
)

func TestAdd(t *testing.T) {
    fmt.Println("haha")
}
```

* 运行用例

```
cd your_test_document/
go test

PASS
ok      _/Users/mike/Documents/develop-environment/htdocs/rc_refactor_go/test/model     0.005s
```



