# grsync — golang rsync wrapper

Repository contains some helpful tools:
- raw rsync wrapper https://godoc.org/github.com/zloylos/grsync/rsync
- rsync task — wrapper which provide important information about rsync task: progress, remain items, total items and speed — https://godoc.org/github.com/zloylos/grsyn

## Task wrapper usage

```golang
package main

import (
    "fmt"
    "grsync"
    "grsync/rsync"
    "time"
)

func main() {
    task := grsync.NewTask(
        "username@server.com:/source/folder",
        "/home/user/destination",
        rsync.Options{},
    )

    go func() {
        for {
            state := task.State()
            fmt.Printf(
                "progress: %.2f / rem. %d / tot. %d / sp. %s \n",
                state.Progress,
                state.Remain,
                state.Total,
                state.Speed,
            )
            time.Sleep(time.Second)
        }
    }()

    if err := task.Run(); err != nil {
        panic(err)
    }

    fmt.Println("well done")
    fmt.Println(task.Log())
}
```
