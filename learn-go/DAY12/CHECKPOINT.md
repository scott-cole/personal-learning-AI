# DAY12: Checkpoint

**Closed book.**

1. What's the difference between `json.Marshal` and `json.NewEncoder`? When would you use each?
   Marsh is when reading from a file, this will convert to json
   Ne

   You had the right idea but swapped them. Marshal returns []byte (good for files/strings), NewEncoder writes to a writer (good for HTTP streaming). Both convert Go → JSON.wEncoder is when usign HTTP will do the same

2. You have this struct:

   ```go
   type User struct {
       ID    int
       Name  string `json:"name"`
       Email string `json:"email,omitempty"`
   }
   ```

   What does `omitempty` do?

if there is a zero value dont include the field at all

3. What would `json.Unmarshal([]byte(input), &p)` do vs `json.Unmarshal([]byte(input), p)` (missing `&`)?

   Without &, it doesn't silently copy — it returns an error at runtime: json: Unmarshal(non-pointer ...). Unmarshal needs a pointer to write into.

4. What does `json.NewDecoder(r.Body).Decode(&v)` return if the body is empty?

It returns io.EOF (an error), not zero values. The decoder expects at least one JSON token. An empty body hits EOF immediately.

5. Why do we set `Content-Type: application/json` before writing JSON in an HTTP response?
   to fill the headers?

The client needs to know the response is JSON so it parses it correctly instead of treating it as plain text or HTML.
