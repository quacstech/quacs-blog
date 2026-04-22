---
title: "Postman, Newman, and a little bash to make it useful"
date: 2026-02-12
description: "How to run Postman collections from the command line with Newman and wrap the output in a bash script that actually tells you what went wrong."
tags: ["postman", "newman", "api", "api testing", "bash"]
---

Most teams write Postman collections, run them manually, and call it a day. This is about taking that collection out of the GUI and into a pipeline — with a bash wrapper that makes the output readable for everyone, not just the person who ran it.

## The problem with just running Newman

Newman works out of the box. You install it, point it at a collection, it runs. But the default output is noisy, exits with a non-zero code on failure without telling you much, and gives you nothing useful to share or log.

This is what a raw Newman run looks like:

```bash
newman run my-collection.json
```

It works. But when it fails at 2am in a CI pipeline, the output is not your friend.

## The setup

You need Node.js and Newman installed:

```bash
npm install -g newman
newman --version
```

Export your Postman collection as a JSON file. If you use environments, export that too:

```bash
# basic run with environment
newman run collection.json -e environment.json
```

## Wrapping it in bash

Here is a script that runs Newman, captures the output, and gives you a clean summary:

```bash
#!/bin/bash

COLLECTION="collection.json"
ENVIRONMENT="environment.json"
REPORT_DIR="./reports"
TIMESTAMP=$(date +"%Y-%m-%d_%H-%M-%S")
REPORT_FILE="$REPORT_DIR/report_$TIMESTAMP.json"

mkdir -p "$REPORT_DIR"

echo "----------------------------------------"
echo "Running API tests: $TIMESTAMP"
echo "----------------------------------------"

newman run "$COLLECTION" \
  -e "$ENVIRONMENT" \
  --reporters cli,json \
  --reporter-json-export "$REPORT_FILE" \
  --color on

EXIT_CODE=$?

echo "----------------------------------------"
if [ $EXIT_CODE -eq 0 ]; then
  echo "RESULT: PASSED"
else
  echo "RESULT: FAILED (exit code $EXIT_CODE)"
  echo "Report saved to: $REPORT_FILE"
fi
echo "----------------------------------------"

exit $EXIT_CODE
```

Save it as `run-tests.sh`, make it executable, and run it:

```bash
chmod +x run-tests.sh
./run-tests.sh
```

## What this gives you

The script does four things the raw Newman command does not:

- timestamps every run so you can tell runs apart
- saves a JSON report for every execution — useful for diffing or feeding into a dashboard
- prints a clean PASSED / FAILED summary at the end
- exits with the correct code so CI pipelines can act on it

## Adding HTML reports

Newman has a separate HTML reporter that generates a readable page you can share with non-engineers:

```bash
npm install -g newman-reporter-htmlextra
```

Update the script to add the HTML reporter:

```bash
newman run "$COLLECTION" \
  -e "$ENVIRONMENT" \
  --reporters cli,json,htmlextra \
  --reporter-json-export "$REPORT_FILE" \
  --reporter-htmlextra-export "$REPORT_DIR/report_$TIMESTAMP.html" \
  --color on
```

Now every run produces both a machine-readable JSON and a human-readable HTML page. Drop the HTML into an S3 bucket or a simple static host and anyone on the team can see the last test run without touching a terminal.

## The point

Newman alone is fine. Newman with a bash wrapper and a report destination is a lightweight API testing pipeline that costs nothing and fits anywhere — local dev, GitHub Actions, Jenkins, whatever you use. No additional tooling, no new platform to learn. Just a script and a folder.