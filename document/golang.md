```go
package main

import (
	"github.com/jolestar/go-commons-pool"
	"context"
	"fmt"
	"strconv"
	"time"
	"math/rand"
)

var pos int

type Redis struct {
	Pos int
}

func (this Redis) Hellow(flag int) {
	//	fmt.Println(strconv.Itoa(this.Pos) + " Hellow Word ")
}

func main() {
	con := context.Background()

	complate := make(chan bool)

	initFunc := pool.NewPooledObjectFactory(
		func(context.Context) (interface{}, error) {

			f := rand.Int()

			fmt.Println(strconv.Itoa(f) + " create...")

			return &Redis{
				Pos: f,
			}, nil
		},
		// 实例创建
		func(ctx context.Context, object *pool.PooledObject) error {
			redis := object.Object.(*Redis)
			fmt.Println(strconv.Itoa(redis.Pos) + " destroy...")
			return nil
		},
		// 实例销毁
		func(ctx context.Context, object *pool.PooledObject) bool {
			//	redis := object.Object.(*Redis)
			//	fmt.Println(strconv.Itoa(redis.Pos) + " validate...")
			return true
		},
		// 实例验证
		func(ctx context.Context, object *pool.PooledObject) error {
			//	redis := object.Object.(*Redis)
			//	fmt.Println(strconv.Itoa(redis.Pos) + " activate...")

			return nil
		},
		// 实例验证通过
		func(ctx context.Context, object *pool.PooledObject) error {

			//	redis := object.Object.(*Redis)
			//	fmt.Println(strconv.Itoa(redis.Pos) + " passivate...")
			return nil
		},
	)

	redisPool := pool.NewObjectPool(con, initFunc, &pool.ObjectPoolConfig{
		// 申请对象上限
		MaxTotal: 500,
		// 最大空闲对象数量
		MaxIdle:              5,
		MinEvictableIdleTime: 1000 * 15,
		// 创建时验证
		TestOnCreate:         true,
		// 归还时验证
		TestOnReturn:         true,
	})

	type funcChan chan bool

	var chanList map[int]funcChan
	chanList = map[int]funcChan{}

	for num := 0; num < 10; num++ {
		chanList[num] = make(chan bool)
		go func(flag int) {
			var s map[int]interface{}
			s = map[int]interface{}{}
			defer func() {
				chanList[flag] <- true
			}()
			for i := 0; i < 10; i++ {
				var err error
				s[i], err = redisPool.BorrowObject(con)
				if err != nil {
					panic(err)
				}
				(s[i].(*Redis)).Hellow(flag)
				redisPool.ReturnObject(con, s[i])
			}
		}(num)
	}

	for pos := range chanList {
		<-chanList[pos]
	}

	go func() {
		for {
			time.Sleep(time.Second * 2)

			fmt.Print("GetNumActive:")
			fmt.Print(redisPool.GetNumActive())
			fmt.Print(" GetNumIdle:")
			fmt.Print(redisPool.GetNumIdle())
			fmt.Print(" GetDestroyedCount:")
			fmt.Println(redisPool.GetDestroyedCount())

			// 销毁闲置的实例
			redisPool.Clear(con)
		}
	}()

	<-complate
}

```



