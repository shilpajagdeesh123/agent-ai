# Mainframe Veracode Skill

## Skill Identity

**Skill name:** `mainframe-veracode-skill`

**Skill file:** `.github/SKILL/mainframe-veracode-skill.md`

**Invoked by:** `.github/mainframe-veracode-agent.md`

---

## Purpose

This skill provides detailed analysis, classification, remediation, testing, and CSV reporting rules for Veracode findings in mainframe applications.

Use this skill only when a Veracode finding, report, finding ID, CWE ID, or Veracode-exported artifact has been supplied.

---

## Scope Enforcement

Before analysis:

1. Confirm that the issue came from Veracode.
2. Confirm the application or module in scope.
3. Restrict analysis to supplied findings.
4. Open additional source files only when required for source-to-sink tracing.
5. Do not conduct general vulnerability discovery.
6. Do not report unrelated defects.
7. Do not modify code automatically.
8. Do not assume missing runtime controls.
9. Do not invent missing metadata.
10. Generate one report row per finding.

---

## Supported Input Artifacts

- Veracode PDF
- Veracode XML
- Veracode JSON
- Veracode CSV
- Veracode SARIF
- Veracode screenshot
- Finding ID
- CWE ID
- COBOL source
- JCL
- Copybook
- CICS program
- DB2 program
- IMS program
- VSAM program
- MQ program
- Calling program
- Called program
- Mitigation text
- Previous remediation proposal

---

## Detailed Analysis Procedure

### Step 1: Extract Finding Metadata

Capture:

- Application name
- Application version
- Scan name
- Scan type
- Scan date
- Finding ID
- CWE ID
- CWE name
- Severity
- Policy status
- Finding status
- Module name
- Program name
- File name
- Paragraph
- Section
- Line number
- Source
- Sink
- Data path
- Existing mitigation
- Veracode recommendation

Use `Not Provided` for absent user data.

Use `Not Found` when searched but not found.

### Step 2: Locate Reported Code

Locate the exact statement identified by Veracode.

Record:

- Program
- File
- Paragraph
- Section
- Line
- Statement
- Variables
- Copybooks
- Caller
- Callee

Do not expand beyond the required call hierarchy.

### Step 3: Identify the Source

Potential mainframe sources include:

- CICS RECEIVE
- CICS COMMAREA
- CICS channels
- CICS containers
- Terminal input
- Linkage-section input
- JCL PARM
- SYSIN
- Sequential file
- VSAM record
- DB2 result
- IMS message
- MQ message
- External program response
- Copybook input field
- HTTP gateway input reaching CICS
- File-transfer input
- Batch control record

Classify the source as:

- User Controlled
- Externally Controlled
- Application Controlled
- Database Controlled
- System Controlled
- Unknown

### Step 4: Identify the Sink

Potential sinks include:

- Dynamic SQL
- PREPARE
- EXECUTE IMMEDIATE
- CICS LINK
- CICS XCTL
- CICS START
- CALL
- MQPUT
- File WRITE
- File REWRITE
- Dataset-name construction
- Program-name construction
- Queue-name construction
- Command construction
- Log output
- DISPLAY
- Error-message construction
- Response construction
- Temporary storage
- Transient data queue
- IMS call
- External utility invocation

### Step 5: Trace Data Flow

Trace:

1. Entry source
2. Receiving field
3. Working-storage field
4. Group move
5. REDEFINES
6. OCCURS table
7. STRING
8. UNSTRING
9. INSPECT
10. Reference modification
11. Copybook field
12. Called program
13. Returned field
14. Validation routine
15. Sanitization routine
16. Final sink

Record only steps that are relevant to the finding.

### Step 6: Review Existing Controls

Evaluate:

- Numeric validation
- Alphabetic validation
- Alphanumeric validation
- Allow-list validation
- Deny-list validation
- Length validation
- Range validation
- Format validation
- Transaction authorization
- RACF authorization
- CICS resource authorization
- DB2 authorization
- File access control
- Data masking
- Control-character removal
- SQL host-variable usage
- Fixed program names
- Fixed transaction IDs
- Fixed queue names
- Error handling
- Compensating operational controls

Classify each control as:

- Effective
- Partially Effective
- Ineffective
- Not Visible to Veracode
- Unverified

### Step 7: Determine Exploitability

Evaluate:

- External reachability
- Input control
- Validation bypass
- Sink behavior
- Execution context
- Privilege level
- RACF restrictions
- CICS restrictions
- DB2 restrictions
- Dataset restrictions
- Batch scheduling controls
- Production-only controls
- Dead code
- Unreachable code
- Environment-specific behavior
- Compensating controls

### Step 8: Classify Finding

Use exactly one:

- True Positive
- False Positive
- Mitigated by Existing Control
- Requires Manual Review
- Insufficient Evidence
- Analysis Error

Do not classify by severity alone.

### Step 9: Determine Root Cause

Explain:

- Why Veracode reported the issue
- Which field is tainted
- Which statement is vulnerable
- Why validation is missing or insufficient
- Whether the issue is design-related or implementation-related
- Whether a shared copybook contributes
- Whether the problem crosses program boundaries
- Whether the problem depends on runtime configuration

### Step 10: Produce Remediation

The remediation must:

- Address the root cause
- Preserve business behavior
- Preserve interfaces
- Preserve layout
- Preserve return codes
- Preserve transaction flow
- Preserve database flow
- Preserve batch restartability
- Use compiler-compatible syntax
- Minimize the changed code
- Avoid redesign unless explicitly requested

### Step 11: Explain Veracode Closure

Explain:

- Which tainted flow is removed
- Which validation is added
- Which sink is protected
- Whether the data becomes allow-listed
- Whether a static resource replaces a dynamic resource
- Whether the finding needs a rescan
- Whether a mitigation comment is still required
- Whether the finding may move to another line

### Step 12: Define Tests

Include:

- Positive tests
- Negative tests
- Boundary tests
- Invalid-character tests
- Invalid-length tests
- Invalid-range tests
- Batch tests
- CICS tests
- DB2 tests
- IMS tests
- VSAM tests
- MQ tests
- Regression tests
- Veracode rescan verification

---

## COBOL Rules

### Input Validation

Prefer explicit allow-list validation.

Validate:

- Length
- Character set
- Numeric range
- Signed values
- Decimal scale
- Date format
- Code values
- Transaction identifiers
- Program identifiers
- Queue names
- Dataset qualifiers

Do not rely only on character replacement.

### Data Movement

Review:

- MOVE
- MOVE CORRESPONDING
- STRING
- UNSTRING
- INSPECT
- REDEFINES
- Reference modification
- Group moves
- Truncation
- Sign handling
- COMP
- COMP-3
- Zoned decimal
- Edited fields

### Shared Copybooks

Before recommending a copybook change:

1. Identify all known consumers.
2. Evaluate field offsets.
3. Evaluate REDEFINES.
4. Evaluate OCCURS.
5. Evaluate binary compatibility.
6. Evaluate batch and online impact.
7. Prefer local validation over layout change when possible.

---

## DB2 Rules

### SQL Injection

1. Identify static SQL versus dynamic SQL.
2. Identify concatenated SQL fragments.
3. Trace all values entering SQL text.
4. Prefer static SQL with host variables.
5. Use parameter markers where supported.
6. Allow-list dynamic table names.
7. Allow-list dynamic column names.
8. Allow-list sort direction.
9. Allow-list SQL keywords.
10. Do not rely only on escaping.
11. Preserve SQLCODE handling.
12. Preserve SQLSTATE handling.
13. Preserve cursor behavior.
14. Preserve commit and rollback behavior.

### Sensitive Data

Review:

- Sensitive column retrieval
- Logging of SQL errors
- SQL text exposure
- Account data
- Authentication data
- Personal data
- Masking requirements

---

## CICS Rules

Review:

- COMMAREA
- EIBCALEN
- Channels
- Containers
- RECEIVE
- SEND
- LINK
- XCTL
- START
- RETURN
- Temporary storage
- Transient data
- Transaction IDs
- Program IDs
- File IDs
- Queue IDs
- RESP
- RESP2

Use fixed or allow-listed resource names.

Validate lengths before moving data.

Do not expose raw system errors to users.

---

## IMS Rules

Review:

- PCB usage
- I/O areas
- Message segments
- Transaction codes
- DL/I calls
- Status codes
- Segment lengths
- Sensitive-data handling

Preserve PCB and message-layout contracts.

---

## VSAM Rules

Review:

- Record keys
- Record lengths
- Alternate indexes
- Dynamic file names
- READ
- WRITE
- REWRITE
- DELETE
- START
- Status codes
- Input validation

Preserve file status handling and restart behavior.

---

## MQ Rules

Review:

- MQGET input
- MQPUT output
- Queue names
- Reply-to queues
- Correlation IDs
- Message IDs
- Message length
- Message format
- Sensitive content
- Logging
- Error handling

Use fixed or allow-listed queue names.

Validate message length and structure.

---

## JCL Rules

Review JCL only when involved in the finding.

Check:

- PARM
- SYSIN
- Dataset names
- Symbolic parameters
- Temporary datasets
- Credentials
- Sensitive values
- Executed program
- Utility invocation
- Output destination
- Cleanup behavior
- Return-code handling

Do not perform a general JCL standards review.

---

## Logging and CRLF Rules

For CWE-117, CWE-113, or related output-neutralization findings:

1. Identify user-controlled log fields.
2. Reject or replace carriage return.
3. Reject or replace line feed.
4. Reject or replace low-value control characters.
5. Use fixed-format log messages.
6. Mask sensitive values.
7. Do not log passwords.
8. Do not log tokens.
9. Do not log credentials.
10. Do not log complete sensitive records.
11. Ensure sanitization occurs before the log sink.
12. Verify all intermediate moves.

---

## Hardcoded Credential Rules

Review:

- USERID
- PASSWORD
- TOKEN
- API key
- RACF ID
- Database credential
- MQ credential
- FTP credential
- SFTP credential
- Dataset-stored secret

Do not reproduce real credentials in the report.

Recommend externalized secure storage or approved credential-management mechanisms.

---

## Path and Resource Injection Rules

Review dynamic construction of:

- Dataset names
- Program names
- Transaction IDs
- Queue names
- File names
- Utility names
- Command strings
- Environment qualifiers

Prefer fixed values or strict allow lists.

---

## Error Handling Rules

Review:

- DISPLAY statements
- SQL error output
- CICS error output
- MQ error output
- File-status output
- Abends
- Diagnostic records
- Sensitive details
- Internal program names
- Dataset names
- SQL text

Use generic user-facing messages and controlled internal diagnostics.

---

## Remediation Format

For each finding, produce:

### Original Code

```cobol
Relevant original code
```

### Secure Code

```cobol
Relevant corrected code
```

### Explanation

Explain what changed.

### Veracode Rationale

Explain why the data flow is no longer vulnerable.

### Regression Risk

Use:

- Low
- Medium
- High

Explain the reason.

---

## CSV Report Rules

### Output Path

```text
.github/output/mainframe-veracode-agent-findings-<YYYY-MM-DD>.csv
```

### Required Columns

1. Analysis Sequence
2. Application Name
3. Application Version
4. Scan Name
5. Scan Date
6. Finding ID
7. CWE ID
8. CWE Name
9. Severity
10. Policy Status
11. Finding Status
12. Module Name
13. Program Name
14. File Name
15. Paragraph
16. Section
17. Line Number
18. Source Type
19. Source Variable
20. Sink Type
21. Sink Statement
22. Data Flow Summary
23. Existing Validation
24. Existing Sanitization
25. Compensating Control
26. Classification
27. Confidence
28. Exploitability
29. Business Impact
30. Root Cause
31. Recommended Fix
32. Original Code
33. Secure Code
34. Regression Risk
35. Affected Programs
36. Affected Copybooks
37. Test Recommendations
38. Manual Validation Required
39. Mitigation Required
40. Ready for Rescan
41. Analysis Status
42. Analysis Notes

### Data Rules

- One finding per row.
- Use UTF-8.
- Use comma delimiter.
- Use RFC 4180-compatible quoting.
- Double embedded quotes.
- Keep header order exact.
- Never omit failed findings.
- Never add unrelated findings.
- Use `Not Provided` for absent data.
- Use `Not Found` for unsuccessful searches.
- Use `Not Applicable` only when valid.
- Use `Requires Manual Review` when runtime evidence is missing.
- Do not include secrets.
- Do not include complete sensitive records.
- Keep code snippets concise.
- Remove unnecessary multiline formatting.
- Ensure the final row count matches the supplied finding count.

---

## Duplicate Handling

Findings are duplicates only when all of the following match:

- Finding ID
- Application
- File
- Line
- CWE

If duplicate records exist:

- Keep one primary row.
- Record duplicate references in `Analysis Notes`.
- Do not silently discard duplicates with conflicting metadata.

---

## Missing Input Handling

When source code is unavailable:

- Set classification to `Insufficient Evidence` unless other evidence is conclusive.
- Set `Analysis Status` to `Incomplete`.
- Set `Ready for Rescan` to `No`.
- State required source files in `Analysis Notes`.

When runtime configuration is unavailable:

- Use `Requires Manual Review`.
- Identify the exact control to verify.

When the Veracode finding cannot be parsed:

- Use `Analysis Error`.
- Record the error.
- Continue processing remaining findings.

---

## Report Validation Checklist

Before completing:

1. Output directory exists.
2. Filename uses current date.
3. Header has exactly 42 columns.
4. Every row has exactly 42 fields.
5. Every supplied finding has one row.
6. No unrelated finding is included.
7. No secret is exposed.
8. CSV parses successfully.
9. Quoting is valid.
10. Date format is `YYYY-MM-DD`.
11. Classification is valid.
12. Analysis status is present.
13. Ready-for-rescan field is present.
14. Output path is reported to the user.

---

## Final Restrictions

Do not use this skill for:

- General code review
- General vulnerability discovery
- Modernization
- Performance tuning
- Java
- Spring Boot
- Maven
- Gradle
- Dependency scanning
- Infrastructure scanning
- CI/CD scanning
- Cloud scanning
- Non-Veracode findings

Use this skill exclusively for Veracode findings in mainframe applications.
