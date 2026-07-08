# DAY28 Drill — The Flight Recorder & Waiter

## Task

Write a CLI tool (`drill.py`) that uses logging and argparse to process a text file.

### Requirements

1. **Argparse interface**:
   ```
   usage: drill.py [-h] [-o OUTPUT] [-v] [-q] input

   Process a text file with statistics.

   positional arguments:
     input                 Input text file

   optional arguments:
     -h, --help            show this help message and exit
     -o OUTPUT, --output OUTPUT
                           Output file (default: report.txt)
     -v, --verbose         Enable debug logging
     -q, --quiet           Suppress all output except errors
   ```

2. **Processing** — Read the input file and compute:
   - Total line count
   - Total word count
   - Total character count
   - Most common word
   - Average words per line

3. **Logging**:
   - Normal mode (`-v` not set): log at `INFO` level
   - Verbose mode (`-v`): log at `DEBUG` level
   - Quiet mode (`-q`): log at `ERROR` level only
   - Log to console with format: `[%(levelname)s] %(message)s`
   - Log milestones: file opened, lines read, stats computed, report written

4. **Output** — Write a formatted report to the specified output file

### Expected usage

```bash
python3 drill.py sample.txt -o report.txt -v
```

### Expected output (console with -v)

```
[DEBUG] Starting file processing
[INFO] Processing file: sample.txt
[DEBUG] File sample.txt opened successfully
[DEBUG] Read 42 lines from file
[INFO] Computing statistics...
[DEBUG] Word count: 356, Char count: 2142
[INFO] Most common word: "the" (12 occurrences)
[DEBUG] Writing report to report.txt
[INFO] Report written successfully
```

### Expected `report.txt`

```
=== FILE REPORT ===
File: sample.txt
Lines: 42
Words: 356
Characters: 2142
Average words/line: 8.48
Most common word: "the" (12 times)
```

### Hints

- Use `collections.Counter` for word frequency
- `words = line.split()` for word count
- `len(line)` for character count (including spaces)
- Set log level based on args: `DEBUG` if verbose, `ERROR` if quiet, `INFO` otherwise

### How to run

Create a test file first:
```bash
echo "hello world hello python" > sample.txt
echo "python is great" >> sample.txt
python3 drill.py sample.txt -o report.txt -v
```

## TODOs

- [ ] Set up argparse with input, output, verbose, quiet
- [ ] Configure logging based on verbosity
- [ ] Read and process the input file
- [ ] Compute statistics (lines, words, chars, most common word)
- [ ] Write formatted report to output file
- [ ] Log milestones at appropriate levels
