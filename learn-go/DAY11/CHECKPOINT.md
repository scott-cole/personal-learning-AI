# DAY11: Checkpoint

**Closed book.**

1. What does `http.ResponseWriter` do? What does `*http.Request` contain?
   reponseWriter is where you write the response, request contains body, headers, method and URL

2. Write a handler that reads a `name` query parameter and responds with "Hello, [name]!".
   func helloHandler(w http.ResponseWriter, r \*http.Request){
   name := URL.Query().Get("name")
   fmt.Fprintf(w, "Hello, %s!", name)
   }

3. What does `http.Redirect(w, r, url, http.StatusFound)` do?
   It tells the browser/client: "the thing you asked for is at this other URL". StatusFound (302) means "temporary redirect" — the browser goes to the new URL but keeps using the old one next time. Use StatusMovedPermanently (301) if it should never ask the old one again.

4. You get a POST request with JSON body `{"url": "https://example.com"}`. How would you read the body? (Just describe the steps — you don't need exact JSON parsing yet)

Declare a struct with json:"url" tag
json.NewDecoder(r.Body).Decode(&struct)
Check the error

8. What happens if you return after `http.Error(w, msg, code)` without calling `return`?

http.Error writes the response but doesn't stop execution. Without return, the code keeps running and tries to write again — which works but logs a warning ("superfluous response.WriteHeader call"). More importantly, if the next code writes different content, you get a garbled response.
