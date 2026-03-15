---
name: run-adk-evals
description: >-
  Run Google Agent Development Kit (ADK) evaluations in sorted order with
  detailed results, failure summaries, and rate limiting between tests.
  Specify the agent folder containing evals/ directory.
allowed-tools:
  - Bash
  - Read
  - Glob
user-invocable: true
argument-hint: "<agent-folder-path>"
---

Run all Google Agent Development Kit (ADK) evaluations in the specified folder in numerical then alphabetical order with detailed results and failure summaries.

**Usage**: Specify the folder containing the ADK agent and evaluations (e.g., `test_agent/`)

**Example**:
```bash
run_adk_evals my_agent/
```
This will run all evaluations in `my_agent/evals/` directory in sorted order.

**Prerequisites**:
- Ensure required environment variables are set (e.g., GOOGLE_API_KEY, MCP server URLs)
- Any required services must be running
- ADK dependencies installed (`pip install -r requirements.txt`)

**Process**:
1. **Validate Environment**: Check required environment variables and dependencies
2. **Discover Evaluations**: Find all *_test.json files in the evals/ directory
3. **Sort Execution Order**: Numerical then alphabetical order (e.g., test_config.json, then alphabetical *_test.json)
4. **Run Each Evaluation**: Execute `adk eval` with `--print-detailed-results` for each test
5. **Summarize Results**: Provide detailed summary of all results and failure reasons

**ADK Eval Command Format**:
For each evaluation, run:
```bash
PYTHONPATH=.:$PYTHONPATH adk eval \
  --config_file_path {agent_folder}/evals/test_config.json \
  --print_detailed_results \
  {agent_folder} \
  {agent_folder}/evals/{test_file}.json
```

**Rate Limiting**: Add 10-second delays between tests to avoid API limits. Use sequential execution only.

**Output Format**:
- Clear section headers with timestamps
- Individual test results with pass/fail status
- Detailed failure analysis for any failed evaluations
- Overall summary with statistics

**Error Handling**: Rate limiting delays between tests, comprehensive error reporting for troubleshooting.
