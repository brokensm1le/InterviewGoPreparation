# Middleware

Взято из статьи: https://habr.com/ru/companies/ruvds/articles/566198/

В исходном варианте сервера в начале каждого обработчика присутствует вызов log.Printf, предназначенный для логирования
обрабатываемого запроса. Это — одна из задач, которую можно решить средствами middleware, и при этом обойтись меньшими
объёмами повторяющегося кода. Вот простой пример кода такого ПО, решающего задачу логирования:

```go
func Logging(next http.Handler) http.Handler {
  return http.HandlerFunc(func(w http.ResponseWriter, req *http.Request) {
    start := time.Now()
    next.ServeHTTP(w, req)
    log.Printf("%s %s %s", req.Method, req.RequestURI, time.Since(start))
  })
}
```

Этот код, в дополнение к логированию метода и URI запроса, подсчитывает и логирует время, необходимое обработчику на
решение его задачи.

Для того чтобы подключить это ПО к нашим обработчикам, приведём код main к следующему виду:

```go
func main() {
  mux := http.NewServeMux()
  server := NewTaskServer()
  mux.HandleFunc("/task/", server.taskHandler)
  mux.HandleFunc("/tag/", server.tagHandler)
  mux.HandleFunc("/due/", server.dueHandler)

  handler := middleware.Logging(mux)

  log.Fatal(http.ListenAndServe("localhost:"+os.Getenv("SERVERPORT"), handler))  // Применено ко всем handler-ам
}
```

```go
func main() {
  mux := http.NewServeMux()
  server := NewTaskServer()
  mux.HandleFunc("/task/", server.taskHandler)
  mux.Handle("/tag/", middleware.Logging(http.HandlerFunc(server.tagHandler)))  // к конкретному
  mux.HandleFunc("/due/", server.dueHandler)

  log.Fatal(http.ListenAndServe("localhost:"+os.Getenv("SERVERPORT"), mux))
}
```