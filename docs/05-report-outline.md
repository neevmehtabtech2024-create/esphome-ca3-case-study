# 5. Report Outline and Viva Preparation

Use the sequence required by your course guidelines. Add the group names/IDs and actual evidence before submission.

## Suggested report sections
1. Title page
2. Introduction to open-source software and ESPHome
3. Project and repository overview
4. Issue #16254 investigation
5. Bug reproduction: method, environment, results, screenshots
6. Root-cause analysis based on PR #16273
7. Contribution workflow and pull-request review
8. Testing and regression fixture analysis
9. Observations and limitations
10. Conclusion
11. References

## Draft conclusion
This case study examines a configuration-parser regression in ESPHome 2026.4.0 and later versions. The public issue reports that a member inside a dictionary-valued substitution could no longer reference sibling values. The linked PR explains the failure as a resolution-order problem introduced during a substitution refactor. The fix makes the dictionary available while recursively resolving its members and adds a regression fixture to test the behaviour. The PR was merged into the development branch on 6 May 2026 and listed for ESPHome 2026.4.5. The case demonstrates why detailed issue reports, small targeted fixes, regression tests, review, automated checks, and release notes matter in open-source maintenance. The final report must separately state whether our group reproduced the issue locally and include the real test output.

## Viva questions and short answers

### 1. What is ESPHome?
An open-source system for configuring microcontroller firmware with YAML and integrating supported devices with home-automation systems.

### 2. What is a regression bug?
A defect where previously working behaviour stops working after a change.

### 3. What are substitutions?
Reusable values in ESPHome YAML configuration, referenced using syntax such as `${device.name}`.

### 4. What was the failure?
A value inside a dictionary-valued substitution referenced a sibling key, but the resolver could not resolve the parent dictionary during recursion.

### 5. What caused it?
According to the PR, a refactor introduced a pre-resolution pass that removed the parent from unresolved variables before processing its dictionary members.

### 6. How did the fix work?
It published the dictionary into resolved variables before recursive processing, allowed the resolver to use that partially resolved dictionary, and then stored the final resolved dictionary.

### 7. Why add a regression test?
To make the failure repeatable and check that future changes do not break the same behaviour again.

### 8. Did our group write PR #16273?
No. It is an upstream PR by `bdraco`. Our work is a case-study analysis unless we separately make and document our own contribution.

### 9. Why compare two versions?
The affected version is expected to show the reported failure; the fixed version is expected to accept the same configuration. The comparison tests whether the fix addresses the regression.

### 10. What if we cannot reproduce it?
Report the exact environment, commands, and output. Clearly distinguish the original reporter's evidence from the group's own result; do not fabricate a successful reproduction.
