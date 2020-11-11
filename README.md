# Mediawiki EventStreams client for Go

Non blocking execution (with cancel context):
```go
ctx, cancel := context.WithCancel(context.Background())
client := eventstream.NewClient()

stream := client.RevisionCreate(ctx, time.Now(), func(evt *events.RevisionCreate) {
	fmt.Println(evt)
})

go func() {
	time.Sleep(2 * time.Second)
	cancel()
}()

for err := range stream.Sub() {
	fmt.Println(err)
}
```

Blocking the execution:
```go
client := eventstream.NewClient()
stream := client.PageDelete(context.Background(), time.Now(), func(evt *events.PageDelete) {
	fmt.Println(evt.Data)
})

err := stream.Exec()

if err != nil {
	log.Panic(err)
}
```

Form more information about the stream and how to use it visit [EventStreams](https://stream.wikimedia.org/?doc) documentation.


### *Note that we are not supporting all the streams yet, we'll be adding more streams support, feel free to fork the repo or create PR to add new streams.