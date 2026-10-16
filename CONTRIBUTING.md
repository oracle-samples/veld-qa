<!--
VELD-QA: A Large-Scale Dataset for Visual Evidence Grounding and Question Answering in Long Documents

Copyright © 2026, Oracle and/or its affiliates.
Licensed under the Creative Commons Attribution-ShareAlike 4.0 International
License (CC BY-SA 4.0) except where otherwise noted.
Third-party datasets, code, and other materials included in or referenced by
this repository remain subject to their original licenses. See LICENSE.txt
for the repository license, Attribution.txt for source attribution, and
Third-Party-Licenses.txt for third-party license texts.
-->

# Contributing to VELD-QA

We welcome documentation improvements, bug reports, and proposals for dataset
corrections or additions.

## Opening issues

For bugs or enhancement requests, file a GitHub issue unless it is security
related. Include the category, split, `example_id`, affected field, expected
behavior, and enough information to reproduce the problem. Do not include
credentials, personal information, confidential information, or data that you
do not have permission to share.

If you think you have found a security vulnerability, do not raise a GitHub
issue. Follow the instructions in our [security policy](./SECURITY.md).

## Submitting contributions

Before submitting a pull request, you must sign the
[Oracle Contributor Agreement][OCA] (OCA). Each commit must include the
following line, using the name and email address used to sign the OCA:

```text
Signed-off-by: Your Name <you@example.org>
```

Add the line automatically by committing with `--signoff` or `-s`, for example:

```text
git commit --signoff
```

Only pull requests from contributors who can be verified as having signed the
OCA can be accepted.

For dataset corrections, include the affected `example_id` values, source
PDF or workbook paths, and evidence supporting the proposed change so
maintainers can verify it. Keep annotations consistent with the
[data format](docs/DATA_FORMAT.md). Documentation fixes may update the
applicable Markdown files directly.

## Pull request process

1. Create an issue to discuss the fix or enhancement.
2. Fork the repository and create a focused branch.
3. Make the smallest change needed to resolve the issue.
4. Update relevant documentation and attribution information.
5. Confirm that no credentials, confidential information, or unapproved
   third-party material is included.
6. Submit a signed-off pull request that references the issue and describes how
   maintainers can validate the change.

## Code of conduct

Follow the [Golden Rule](https://en.wikipedia.org/wiki/Golden_Rule). For more
specific guidelines, see the [Contributor Covenant Code of Conduct][COC].

[OCA]: https://oca.opensource.oracle.com
[COC]: https://www.contributor-covenant.org/version/1/4/code-of-conduct/
