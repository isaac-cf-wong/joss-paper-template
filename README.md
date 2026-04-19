# JOSS paper template

A minimal repository layout for writing a
[Journal of Open Source Software (JOSS)](https://joss.theoj.org/) article in
Markdown with BibTeX references, following the
[official JOSS paper format](https://joss.readthedocs.io/en/latest/paper.html).

[![Draft PDF](https://github.com/YOUR_GITHUB_USERNAME/YOUR_REPO_NAME/actions/workflows/draft-pdf.yml/badge.svg)](https://github.com/YOUR_GITHUB_USERNAME/YOUR_REPO_NAME/actions/workflows/draft-pdf.yml)
[![pre-commit](https://github.com/YOUR_GITHUB_USERNAME/YOUR_REPO_NAME/actions/workflows/pre-commit.yml/badge.svg)](https://github.com/YOUR_GITHUB_USERNAME/YOUR_REPO_NAME/actions/workflows/pre-commit.yml)
[![License](https://img.shields.io/github/license/YOUR_GITHUB_USERNAME/YOUR_REPO_NAME)](LICENSE)
[![JOSS paper docs](https://img.shields.io/badge/docs-JOSS%20paper-blue)](https://joss.readthedocs.io/en/latest/paper.html)

## Repository layout

| Path                               | Purpose                                                                                     |
| ---------------------------------- | ------------------------------------------------------------------------------------------- |
| `paper.md`                         | Manuscript with YAML metadata and required sections (see JOSS docs).                        |
| `paper.bib`                        | BibTeX database referenced by `bibliography:` in the metadata.                              |
| `figures/`                         | Images referenced from `paper.md`.                                                          |
| `.github/workflows/draft-pdf.yml`  | Builds a draft PDF using the Open Journals action.                                          |
| `.github/workflows/pre-commit.yml` | Runs [pre-commit](https://pre-commit.com/) on pushes and pull requests.                     |
| `.pre-commit-config.yaml`          | Local hook configuration (install with `uv run pre-commit install`).                        |
| `pyproject.toml`                   | Declares the Python dev dependency group (pre-commit) for [uv](https://docs.astral.sh/uv/). |
| `uv.lock`                          | Locked versions for reproducible installs (`uv sync` updates this file).                    |

After you create the GitHub repository, replace `YOUR_GITHUB_USERNAME` and
`YOUR_REPO_NAME` in the badge URLs at the top of this file so the status images
resolve correctly.

## Writing the paper

1. Edit the YAML metadata block at the top of `paper.md` (title, tags, authors,
   affiliations, date, bibliography path). The opening and closing `---` lines
   are required.
2. Replace the placeholder sections so they match the
   [current JOSS requirements](https://joss.readthedocs.io/en/latest/paper.html):
   Summary, Statement of need, State of the field, Software design, Research
   impact statement, AI usage disclosure, Acknowledgements, and References.
3. Keep the manuscript roughly between 750 and 1750 words (excluding metadata
   and references).
4. Add citations in Pandoc style (for example `[@smith2024]`) and matching
   entries in `paper.bib`. Use full venue names in bibliographic entries where
   applicable.
5. Set `date:` using the format documented by JOSS (for example
   `19 April 2026`).

The [example paper](https://joss.readthedocs.io/en/latest/example_paper.html) is
a useful concrete reference for metadata, mathematics, figures, and citation
syntax.

## Local checks

Install [uv](https://docs.astral.sh/uv/getting-started/installation/), then
create the virtual environment and install **pre-commit** from `pyproject.toml`:

```bash
uv sync --group dev
uv run pre-commit install
uv run pre-commit run --all-files
```

`uv.lock` pins transitive versions; after changing `pyproject.toml`, run
`uv lock` (or `uv sync`, which updates the lock when needed) and commit the
updated `uv.lock`.

## Draft PDF on GitHub

Pushing changes to `paper.md`, `paper.bib`, or files under `figures/` triggers
the **Draft PDF** workflow. Download `paper.pdf` from the workflow run’s
**Artifacts** section.

You can also run the workflow manually from the **Actions** tab
(**workflow_dispatch**).

## Optional: local PDF with Docker

The Open Journals toolchain is published as the
[`openjournals/inara`](https://github.com/openjournals/inara) container. A
typical invocation from the repository root (paths may vary; see the Inara
documentation for flags such as `JOURNAL=joss`):

```bash
docker run --rm \
  -v "$PWD:/data" \
  -u "$(id -u):$(id -g)" \
  -e JOURNAL=joss \
  openjournals/inara \
  -o pdf \
  paper.md
```

## Submission reminders

- The software under review must be in an open repository with an
  [OSI-approved license](https://opensource.org/licenses).
- Read the
  [review checklist](https://joss.readthedocs.io/en/latest/review_checklist.html)
  and
  [review criteria](https://joss.readthedocs.io/en/latest/review_criteria.html)
  while drafting.
- Submission details and policies are on
  [joss.theoj.org](https://joss.theoj.org/).

## License

The template files in this repository are released under the terms in
[LICENSE](LICENSE). Replace the copyright holder in `LICENSE` if you fork this
template for your own project.
