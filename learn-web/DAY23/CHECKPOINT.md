# DAY23: Checkpoint

1. What does `http.ErrServerClosed` mean?

   <details>
   <summary>Answer</summary>
   It's the expected error returned by `ListenAndServe` after `Shutdown()` is called — not an actual error.
   </details>

2. What's the difference between `os.Signal` handling and ignoring signals?

   <details>
   <summary>Answer</summary>
   Without handling, the process **terminates immediately** on SIGINT/SIGTERM. With handling, the server can **clean up before exiting**.
   </details>

3. What happens to in-flight requests when `Shutdown` is called?

   <details>
   <summary>Answer</summary>
   The server stops accepting new connections but **waits for existing connections** to complete (up to the context timeout).
   </details>

4. Why set a timeout on the shutdown context (e.g., 30 seconds)?

   <details>
   <summary>Answer</summary>
   To prevent the server from hanging forever if a request doesn't complete. After the timeout, the server **force closes** remaining connections.
   </details>

5. What signal does `kill <pid>` send by default?

   <details>
   <summary>Answer</summary>
   **SIGTERM** (signal 15) — the "polite" termination signal.
   </details>
