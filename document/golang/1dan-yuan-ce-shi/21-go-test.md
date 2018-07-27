* 测试用例 curd\_test.go 

> 注：测试用例文件名称必须以 `_test.go` 结尾，测试 func 必须以 Test 开头

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

* 运行测试用例

> go test 会运行当前目录下所有以 `_test.go` 结尾的文件

```
cd your_test_document/
go test

PASS
ok      _/Users/mike/Documents/develop-environment/htdocs/rc_refactor_go/test/model     0.005s
```



