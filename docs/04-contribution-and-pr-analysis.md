# 4. Contribution Workflow and Pull Request Analysis

**Pull request:** https://github.com/esphome/esphome/pull/16273  
**Title:** “[substitutions] Fix sibling references inside dict-valued substitutions”  
**Author:** `bdraco`  
**Merged:** 6 May 2026 into the `dev` branch  
**Merge commit:** `caaa1ae`  
**Checks:** GitHub showed 43 of 44 checks passed at merge time. Report this accurately; do not say every check passed.

## 4.1 What the PR changes
The PR restores references from one member of a dictionary-valued substitution to sibling members in the same dictionary. It changes the substitution resolver and adds an input fixture plus an approved-output fixture.

Files changed:
1. `esphome/components/substitutions/__init__.py`
2. `tests/unit_tests/fixtures/substitutions/18-dict_self_reference.input.yaml`
3. `tests/unit_tests/fixtures/substitutions/18-dict_self_reference.approved.yaml`

## 4.2 Fix in plain language
The resolver now makes the dictionary-valued substitution available in the resolved-variable collection before recursively processing its contents. The resolver can then fall back to that partially resolved value when a reference is made during recursive processing. After recursion finishes, the entry is replaced with the fully substituted dictionary.

In simple terms: the parent dictionary was temporarily unavailable while its children were being resolved. The fix makes the dictionary visible during that process, so one child can use sibling values.

## 4.3 Regression test
The PR added a fixture named `18-dict_self_reference`. It covers the issue's `device` pattern and a second `network` dictionary, checking both sibling resolution and later access to `${device.name}`. The input fixture expresses the scenario; the approved-output fixture records the expected resolved output. This is important because it turns the bug report into a repeatable automated check, helping prevent the same regression from returning.

## 4.4 Review and merge evidence
The public PR page shows:
- The PR was marked merged on 6 May 2026.
- A maintainer approval was recorded.
- The PR was merged into `dev` with commit `caaa1ae`.
- GitHub showed 43 of 44 checks passed.
- The PR was linked to issue #16254.
- The PR was included in the ESPHome 2026.4.5 release notes.

## 4.5 How this illustrates the open-source contribution process
A typical contribution workflow is:
1. A user reports a reproducible problem with version details, configuration, and logs.
2. A maintainer/contributor analyses the failure and proposes a focused change.
3. The change includes a regression test.
4. Automated checks run.
5. A reviewer evaluates the change.
6. The change is merged and included in a release.
7. Users can verify the fix by testing the affected and fixed versions.

This is the workflow evidenced by this particular issue and PR. Our group has analysed it; we have not authored or submitted the upstream fix.

## 4.6 Contribution guidance from ESPHome
Official developer documentation says contributions are managed through GitHub, changes should be focused, and automated checks must pass before maintainers review and merge a PR. The code guide documents running project checks, including the full test/lint command in a Docker image:

```bash
docker run --rm -v "${PWD}/:/esphome" -it ghcr.io/esphome/esphome-lint script/fulltest
```

Run project-wide checks only after following the current developer setup guide and from a suitable ESPHome source checkout. This is not a command to run from the student case-study repository.

Sources:
- Code contribution guide: https://developers.esphome.io/contributing/code/
- Submitting work: https://developers.esphome.io/contributing/submitting-your-work/
- Development environment: https://developers.esphome.io/contributing/development-environment/
