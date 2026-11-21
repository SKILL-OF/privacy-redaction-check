---
name: privacy-redaction-check
description: Use before any file, commit, pull request, issue or repository is made public, to check it against the no-PII and no-local-path policy.
---

# Skill draft: privacy-redaction-check

Status: draft, not installed. Written by the instruction-architect lane, W2.D3. Proposal only. The role name stays because it names a function, not a person, machine or account. The roster display name is removed because it is an agent identifier, which the policy's "account identifiers" wording covers. The steward decides whether the role name is acceptable. Installation goes through a branch and a pull request, and the owner merges.

## Trigger

Use before any file, commit, pull request, issue, comment or repository is made public, or copied to a public location. Public means any repository or page that a person outside the private workspace can read.

## Policy

The policy is the owner's, recorded in the calendar's privacy policy file, PRIVACY-POLICY.md (recorded 2026-11-20). In brief:
- No local file paths in any public repository.
- No personally identifiable information (PII) in any public repository. This includes names, email addresses, account identifiers and machine names.
- No secrets, tokens or credentials, anywhere.

Who decides: the privacy steward (the privacy-steward class) makes each privacy decision under this policy. This skill runs the check and reports findings. It does not decide whether a finding is a violation. The summoner records the steward's decision with its evidence.

Out of scope: destructive history rewrites. They stay with the owner (OWNER-DECISIONS.md, B1). This skill never rewrites history.

## Check steps

Run each step on the exact content to be published: the file text, the commit message, the pull request body, the issue or comment text, and any file names.

1. **Local paths.** Search for:
   - a drive-letter path (a letter, a colon, a backslash);
   - a Unix-style home or user path (a path beginning with a slash, then a users or home segment);
   - a home-folder reference, and any absolute path to a private folder.
   Each match is a finding. Relative paths inside the repository are not findings.
2. **Personal details.** Search for:
   - an email address;
   - a person's name, a login or account name, or a user name taken from a path or a git identity;
   - a machine name, a host name or an internal workspace name;
   - an account identifier, a session identifier or an agent's private roster id.
   A name that appears only as a public project, product or organisation name is not a finding. The steward decides any borderline case.
3. **Secrets.** Search for:
   - a token, key or password assignment, or a string that matches a known secret format;
   - a connection string or a credential in a URL;
   - a private key block.
   Record the location and the length of any match. Never print, quote or copy the value into the report, the commit, the issue or the chat. A finding that quotes a secret is itself a leak.
4. **Second method.** Repeat step 1 and step 3 with a second search method (a different tool or pattern) before reporting "no finding". One method can miss a match, such as a value split across lines.
5. **Record the result.** Write one line per check: the check, the scope searched, the number of matches, and the location of each match (file and line). No match text.

## On a finding

1. **Stop.** Do not publish, push, merge or post. If the content is already public, report that at once; do not change history.
2. **Report to the privacy steward.** Send the finding record from step 5, with no match text. Include the scope, the locations and the check that found it.
3. **Wait for the steward's decision.** The steward decides one of three outcomes: the content stays public as it is; it is redacted and then stays public; or it is made private. The steward records the reason.
4. **Apply only that decision.** A redaction goes in a new commit on a branch, through a pull request, and the owner merges it. Do not amend or rewrite published commits.
5. **If a secret was found:** tell the steward and the owner at once, because the secret may already be exposed. Rotating a credential is an owner or operations decision, not part of this check.
6. **Re-run the check** on the exact content that will be published, after any redaction.

## What this skill does not do

- It does not decide that a finding is or is not a privacy violation. The steward decides.
- It does not rewrite history, delete a repository or change visibility.
- It does not print or store a secret value.
- It does not extend the policy. A new category needs the owner's policy change first.

## Open points for the owner

- Whether session identifiers and roster ids count as account identifiers under the policy. The check flags them; the steward decides until the owner rules.
- Which search tools the check uses in the workspace. Not fixed here; the check states the pattern, not a tool.

## Source lines

- Policy items: PRIVACY-POLICY.md, lines 4 to 6.
- Who decides: PRIVACY-POLICY.md, lines 8 and 10.
- History rewrites: PRIVACY-POLICY.md, line 12.
