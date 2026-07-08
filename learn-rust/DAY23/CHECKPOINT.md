# DAY23: Checkpoint

**Closed book.** No looking back.

1. What two crates do you need for JSON serialization with serde?
   serde (with derive feature) and serde_json

2. What attributes do you add to a struct to make it serializable?
   #[derive(Serialize, Deserialize)]

3. How do you rename a field when serializing to JSON?
   #[serde(rename = "new_name")]

4. What does `#[serde(default)]` do?
   uses Default::default() for a field when it's missing from the input

5. What does `serde_json::to_string_pretty` do differently from `serde_json::to_string`?
   adds indentation and newlines for human readability

---

**Check your answers against the lesson after you finish.**
