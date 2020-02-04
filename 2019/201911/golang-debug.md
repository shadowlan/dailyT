why?
panic: /debug/requests is already registered. You may have two independent copies of golang.org/x/net/trace in your binary, trying to maintain separate state. This may involve a vendored copy of golang.org/x/net/trace.

goroutine 1 [running]:
golang.org/x/net/trace.init.0()
	/WorkSpace/msgHub/src/golang.org/x/net/trace/trace.go:116 +0x1a2

https://blog.csdn.net/wo252618378/article/details/93677059