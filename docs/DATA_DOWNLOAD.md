# Download VELD-QA

The release stores annotations in Git and PDFs and Excel workbooks in Git LFS.
The six JSONL files contain 13,548 questions. Their 5,292 PDFs and 1,244 workbooks
occupy 18,655,900,051 bytes (approximately 18.7 GB) when checked out.

## Complete dataset

Install [Git LFS](https://git-lfs.com/) before cloning. On macOS with Homebrew,
run `brew install git-lfs`; on other systems, use the Git LFS installation
instructions for your platform.

```bash
git lfs install
git clone https://github.com/oracle-samples/veld-qa.git
cd veld-qa
git lfs pull
```

Allow approximately 38 GB of free disk space for the downloaded LFS objects and
checked-out files. Individual PDF and workbook files are stored under their
category directories; no archive extraction is needed.

## Annotations only

To read questions and annotations before downloading the large assets:

```bash
GIT_LFS_SKIP_SMUDGE=1 git clone https://github.com/oracle-samples/veld-qa.git
cd veld-qa
```

The PDF and workbook paths in this checkout initially contain small Git LFS
pointer files. Retrieve all assets later with `git lfs pull`, or download one
category at a time:

```bash
git lfs pull --include="chart_understanding/pdfs/**" --exclude=""
git lfs pull --include="table_understanding/pdfs/**,table_understanding/tables/**" --exclude=""
git lfs pull --include="complex_layout/pdfs/**" --exclude=""
```

Only run the commands for the categories you need. A partial download will not
pass the complete-dataset verification below.

## Verify the complete download

Check the downloaded LFS objects:

```bash
git lfs fsck
```

Then confirm that every annotation points to an actual PDF or workbook, rather
than a missing file or an LFS pointer:

```bash
python3 - <<'PY'
import json
from pathlib import Path

root = Path.cwd()
assets = set()
for category in ("chart_understanding", "table_understanding", "complex_layout"):
    for split in ("train", "dev"):
        with (root / category / f"{split}.jsonl").open(encoding="utf-8") as handle:
            for line in handle:
                example = json.loads(line)
                for key in ("pdf_uri", "table_uri"):
                    if example.get(key):
                        assets.add(example[key])

for relative_path in sorted(assets):
    path = root / relative_path
    assert path.is_file(), f"Missing asset: {relative_path}"
    with path.open("rb") as handle:
        prefix = handle.read(1024)
    assert not prefix.startswith(b"version https://git-lfs.github.com/spec/v1"), (
        f"Git LFS pointer instead of asset: {relative_path}; run git lfs pull"
    )
    if path.suffix == ".pdf":
        assert b"%PDF-" in prefix, f"Unexpected PDF header: {relative_path}"
    elif path.suffix == ".xlsx":
        assert prefix.startswith(b"PK"), f"Unexpected workbook header: {relative_path}"

print(f"Verified {len(assets):,} referenced assets")
PY
```

The expected result is `Verified 6,536 referenced assets`.

## Troubleshooting

- **`git: 'lfs' is not a git command`:** install Git LFS, run `git lfs install`,
  then run `git lfs pull` from the repository root.
- **A PDF contains a few lines of text:** it is probably an LFS pointer. Run
  `git lfs pull` before opening it in a PDF viewer.
- **Repository access denied:** confirm that your GitHub account has access if
  the repository is still private.
- **GitHub reports an LFS quota or bandwidth error:** the repository owner must
  resolve the GitHub LFS allowance before downloads can finish.

Use `git clone` and `git lfs pull` for a complete checkout. GitHub's **Download
ZIP** behavior depends on the repository's setting for including LFS objects.

For the official GitHub behavior, see [Git Large File
Storage](https://docs.github.com/en/repositories/working-with-files/managing-large-files/about-git-large-file-storage).
