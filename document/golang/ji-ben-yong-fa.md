

```go
package channel

import (
	"testing"
	"fmt"
	"time"
)

func TestChannel(t *testing.T) {

	str := make(chan string)

	go func() {
		fmt.Println("Start Read")
		for true {
			// 从信道取数据的时候，如果该信道不存在数据则产生阻塞等待
			fmt.Println("Read " + <-str)
		}
	}()

	complate := make(chan bool)
	go func() {
		fmt.Println("Start Write")
		for i := 0; i < 5; i++ {
			s := time.Now().String()
			str <- s
			fmt.Println("Write" + s)
		}
		// 逻辑执行完成之后给阻塞信道赋值，主线程结束
		complate <- true
	}()
	// 信道用于阻塞主线程，不然在协程还没来得及执行主线程就会退出
	<-complate
}

```



