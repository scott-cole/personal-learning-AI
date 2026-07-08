# DAY28 Checkpoint

## Questions

1. What log levels does Python's logging module provide, in order of severity?
2. How do you set the logging level from command-line arguments in argparse?
3. What does `action="store_true"` do in `add_argument()`?
4. How do you add a handler that writes logs to a file?
5. What is the difference between `args = parser.parse_args()` and `args = parser.parse_args(["--help"])`?

<details>
<summary>Answers</summary>

1. DEBUG (10), INFO (20), WARNING (30), ERROR (40), CRITICAL (50).
2. Map the `-v`/`-q` flags to log levels: `level = logging.DEBUG if args.verbose else (logging.ERROR if args.quiet else logging.INFO)`.
3. It creates a boolean flag that is `False` by default and becomes `True` when the flag is present.
4. `handler = logging.FileHandler("app.log"); logger.addHandler(handler)`
5. `parse_args()` reads from `sys.argv` (or the provided list). `parse_args(["--help"])` prints the help message and exits.

</details>
