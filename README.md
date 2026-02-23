# Squidler MCP Server

## Squidler.io

AI coding agents excel at the **Build** phase of a web app, but struggle with the **Verify** phase. Squidler is designed to validate your web app as a human based on natural language use cases, and without writing brittle, DOM-dependent tests.

Squidler is not just a testing tool; it enables your coding agent to analyze its own work from a user perspective, decoupled from the implementation. Squidler closes the autonomous **Build → Verify → Fix** loop, and enables you to act as the true human-in-the-loop who manages the outcome rather than the output.

### Squidler MCP Server

With the Squidler MCP server you enable you AI coding agents, like Cursor, Claude Code, Replit, Lovable etc to work with Squidler. This means that you can have your coding agent build a feature, describe the use case in natural language for Squidler, ask Squidler to test it, to get the details of how the test run went, and details of all problems or friction that were found when using your web app.

Below are details and example for how to get started with Squidler via MCP. If you have any questions, don't hesitate to reach out to mattias@squidler.io.

## Configuration

Add to your MCP client configuration:

```json
{
  "mcpServers": {
    "squidler": {
      "transport": "http",
      "url": "https://mcp.squidler.io",
      "headers": {
        "Authorization": "Bearer YOUR_SQUIDLER_API_TOKEN"
      }
    }
  }
}
```

Get your API token from [squidler.io/integrations/api-keys](https://squidler.io/integrations/api-keys).

## Built-in Agent Guidance

The MCP server includes built-in instructions that teach your AI agent a **Test-After-Feature Workflow**. When connected, the agent automatically knows how to use Squidler without you having to explain it. The workflow it follows:

1. **Run existing tests** — verify nothing is broken by the new feature.
2. **Investigate failures** — use test run outcomes and events to understand what failed. Fix the code if it's a bug, or update/delete the test if it's obsolete.
3. **Label the feature** — create a label for the completed feature (e.g., "User Login", "Checkout Flow") to organize your test catalog.
4. **Cover the new feature** — create test cases for the new behavior, tag them with the feature label, and run them to confirm they pass.
5. **Confirm green** — re-run until all tests pass, then move on to the next feature.

Over time this builds a growing catalog of tests organized by feature that prevents regressions and documents how the system is meant to work.

## Resources

| Resource URI       | Description                                     |
| ------------------ | ----------------------------------------------- |
| `squidler://help`  | Server documentation and available capabilities |
| `squidler://sites` | List of all available sites                     |

## Tools

### Sites

| Tool           | Description                                            |
| -------------- | ------------------------------------------------------ |
| `sites_list`   | List all available sites                               |
| `site_details` | Get site information (owner, URL, name, frontend link) |
| `site_create`  | Create a new site                                      |

### Problems

| Tool               | Description                                       |
| ------------------ | ------------------------------------------------- |
| `problems_summary` | Overview of problems (total count by type)        |
| `problems_list`    | List problems with pagination and filtering       |
| `problem_get`      | Get detailed information about a specific problem |
| `problem_resolve`  | Mark a problem as resolved                        |
| `problem_dismiss`  | Mark a problem as false positive                  |

### Test Cases

| Tool                       | Description                           |
| -------------------------- | ------------------------------------- |
| `test_cases_list`          | List all test cases                   |
| `test_case_get`            | Get test case details                 |
| `test_cases_list_by_label` | List test cases with a specific label |
| `test_case_create`         | Create a new test case                |
| `test_case_update`         | Update an existing test case          |
| `test_case_delete`         | Delete a test case                    |
| `test_case_run`            | Run a single test case                |
| `test_cases_run_all`       | Run all test cases                    |
| `test_cases_run_by_label`  | Run test cases with a specific label  |

### Test Runs

| Tool               | Description                                              |
| ------------------ | -------------------------------------------------------- |
| `test_runs_list`   | List execution history for a test case                   |
| `test_case_poll`   | Long-poll for test execution status updates and events   |
| `test_run_outcome` | Get pass/fail outcome and goal results for a test run    |
| `test_run_events`  | Get detailed execution events and screenshots            |
| `test_run_stop`    | Stop a currently running test                            |

### Labels

| Tool                     | Description                     |
| ------------------------ | ------------------------------- |
| `labels_list`            | List all labels                 |
| `label_get`              | Get label details               |
| `label_create`           | Create a new label              |
| `label_update`           | Update a label                  |
| `label_delete`           | Delete a label                  |
| `test_case_labels_add`   | Add labels to a test case       |
| `test_case_label_remove` | Remove a label from a test case |

### Headers

| Tool            | Description                    |
| --------------- | ------------------------------ |
| `headers_list`  | List custom headers for a site |
| `header_add`    | Add or update a custom header  |
| `header_delete` | Remove a custom header         |

### Credentials

| Tool                      | Description                                                 |
| ------------------------- | ----------------------------------------------------------- |
| `credentials_list`        | List available login credentials                            |
| `credentials_create`      | Create username/password credentials for site authentication |
| `credentials_create_oidc` | Create OIDC credentials for sites using OpenID Connect      |

## Prompts

| Prompt                             | Description                                                              |
| ---------------------------------- | ------------------------------------------------------------------------ |
| `create-test-case`                 | Guided test case creation with goals, steps, and failure strategies      |
| `site-health-check`               | Quick overview of site quality status with actionable recommendations    |
| `run-test-suite`                   | Run all or labeled tests and monitor results                             |
| `get-started-with-squidler`       | Complete onboarding: create site, generate initial tests, run them       |
| `suggest-initial-tests`           | Analyze codebase to suggest 2-4 initial test cases                       |
| `enhance-tests-from-runs`         | Analyze test run events to identify and fix test weaknesses              |
| `identify-ux-improvements`        | Discover usability issues from AI tester friction patterns               |
| `correlate-accessibility-and-ux`  | Cross-reference accessibility problems with test execution friction      |
| `migrate-legacy-tests`            | Migrate V1 legacy test cases to V2 format                               |

## Example Prompts

| What you can say                                                    | What it does                                                                                               |
| ------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| "Create a site for https://myapp.com called 'My App'"               | Register your website with Squidler to start monitoring and testing                                        |
| "Analyze the codebase and create test cases for the key user flows" | Analyzes your code to create comprehensive test cases covering happy flows, edge cases, and negative tests |
| "Run all my test cases"                                             | Execute your test cases in a real browser with goal validation and issue detection                         |
| "Show me the results of my latest test runs"                        | See which goals passed or failed, and understand what happened during execution                            |
| "Get the detailed test events for test case #1 and analyze the UX"  | Review execution logs to identify UX friction and usability issues                                         |
| "Fix the issues preventing test case #1 from passing"               | Analyzes test failures and fixes the underlying code issues                                                |
| "Get problem #42 and help me fix it"                                | Address specific accessibility, functionality, or content issues                                           |
| "Fix all accessibility issues in my site"                           | Batch-fix WCAG compliance issues by iterating through problems                                             |
