+++ 
date = "2014-09-28"
title = "Gracefully Shutdown"
slug = "gracefully-shutdown" 
categories = ["go","web"]
+++

Let's say you mant to shutdown a running application for a time.
However you don't want to cut running processes suddenly. What you want is, "finish your jobs then shut down".

This is the default way to create web server in Go.

``` go
	srv := http.Server{
		Addr:         ":9090",
		Handler:      mux,
		IdleTimeout:  120 * time.Second,
		ReadTimeout:  1 * time.Second,
		WriteTimeout: 1 * time.Second,
    }
    
    err := srv.ListenAndServe()
		if err != nil {
			l.Fatal(err)
		}
```
If you implement gracefully shutdown

``` go
	go func() {
		err := srv.ListenAndServe()
		if err != nil {
			l.Fatal(err)
		}
	}()

	sigChan := make(chan os.Signal)
	signal.Notify(sigChan, os.Interrupt)
	signal.Notify(sigChan, os.Kill)

	sig := <-sigChan
	l.Println("Received termination, gracefullt shutting down", sig)

	tc, _ := context.WithTimeout(context.Background(), 30*time.Second)
	srv.Shutdown(tc)
```

In this code, the webserver will work in another goroutine. The variable named sig blocks the main 
routine in here. If it receives the messages we set up before, it won't block anymore.