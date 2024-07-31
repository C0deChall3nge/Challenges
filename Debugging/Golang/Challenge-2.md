Code Challenge: Debugging a Complex Web Server in Go

Challenge Description

You are given a more complex HTTP web server written in Go. The server is intended to handle three endpoints: /hello, /goodbye, and /data. The /hello endpoint should respond with "Hello, World!", the /goodbye endpoint should respond with "Goodbye, World!", and the /data endpoint should return a JSON response with a message and a timestamp.

Recently, this code was deployed to production, and users have reported several issues:

    The /hello endpoint sometimes returns an empty response.
    The /goodbye endpoint crashes the server.
    The /data endpoint returns malformed JSON.

Your task is to identify and fix the issues causing this behavior.

```
package main

import (
	"encoding/json"
	"fmt"
	"net/http"
	"time"
)

type Response struct {
	Message   string `json:"message"`
	Timestamp string `json:"timestamp"`
}

func main() {
	http.HandleFunc("/hello", helloHandler)
	http.HandleFunc("/goodbye", goodbyeHandler)
	http.HandleFunc("/data", dataHandler)

	fmt.Println("Starting server at port 8080")
	err := http.ListenAndServe(":8080", nil)
	if err != nil {
		fmt.Println("Error starting server:", err)
	}
}

func helloHandler(w http.ResponseWriter, r *http.Request) {
	if r.Method != http.MethodGet {
		http.Error(w, "Invalid request method", http.StatusMethodNotAllowed)
		return
	}
	// Intentional bug: Conditional response without else
	if time.Now().Unix()%2 == 0 {
		fmt.Fprintln(w, "Hello, World!")
	}
}

func goodbyeHandler(w http.ResponseWriter, r *http.Request) {
	if r.Method != http.MethodGet {
		http.Error(w, "Invalid request method", http.StatusMethodNotAllowed)
		return
	}
	// Intentional bug: Dereferencing a nil pointer
	var msg *string
	fmt.Fprintln(w, *msg)
}

func dataHandler(w http.ResponseWriter, r *http.Request) {
	if r.Method != http.MethodGet {
		http.Error(w, "Invalid request method", http.StatusMethodNotAllowed)
		return
	}
	response := Response{
		Message:   "Here is your data",
		Timestamp: time.Now().Format(time.RFC3339),
	}
	// Intentional bug: Incorrect JSON marshalling
	resp, err := json.Marshal(response.Message)
	if err != nil {
		http.Error(w, "Error generating JSON", http.StatusInternalServerError)
		return
	}
	w.Header().Set("Content-Type", "application/json")
	w.Write(resp)
}
```