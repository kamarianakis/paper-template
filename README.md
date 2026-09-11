# Collaborative LaTeX + Git (IEEE sample)

This repository teaches a **workflow**, not a single journal style. The IEEE `.tex` file is only a working example so you can compile on day one.

Use the same setup for **any** manuscript: Elsevier, Springer, ACM, a thesis, a grant, or your group’s own class file. Swap the template; keep Git, VS Code, LaTeX Workshop, and [LaTeX Diff](https://github.com/NoeSilva13/latex-diff).

**What stays the same for every paper**

1. Edit and build **locally** in VS Code (PDF in `build/`).
2. Share work through **one GitHub repository** (pull → edit → commit → push).
3. Make a **colored revision PDF** between two commits or tags (reviewer replies, co-author checks).

**Browse without installing:** [compiled sample](build/LaTeX_Git_IEEE_Paper_Template.pdf) and [highlighted revision](diffs/diff-sample.pdf) (GitHub’s file view). The [LaTeX Diff demo](https://github.com/NoeSilva13/latex-diff/blob/master/media/demo.gif) shows generating that PDF from the editor.

---

## What you will learn

- Install LaTeX + Git on **Windows**, **macOS**, or **Linux**
- Build any `.tex` project with **LaTeX Workshop**
- Work with several people on the **same** repo
- Generate a revision PDF with [LaTeX Diff](https://github.com/NoeSilva13/latex-diff) (or `git latexdiff`)

## Repository layout

| Path | Purpose |
|------|---------|
| `LaTeX_Git_IEEE_Paper_Template.tex` | **Sample** main file (IEEE journal). Replace or add your own `.tex`. |
| `refs.bib` | Sample bibliography (any project can use a `.bib`) |
| `figures/` | Figures; keep stable filenames across revisions |
| `build/LaTeX_Git_IEEE_Paper_Template.pdf` | **Sample** compiled manuscript (tracked so GitHub can show it). Auxiliaries in `build/` are ignored. |
| `diffs/diff-sample.pdf` | **Sample** colored revision PDF (Results + Conclusion edits). Other files in `diffs/` are ignored. |
| `IEEEtran.cls` / `IEEEtran.bst` | IEEE class files for this sample only |
| `.vscode/` | Shared editor settings (Workshop → `build/`) |
| `.latexmkrc` | Tells `latexmk` to write into `build/` (even from a terminal). Leave it as is. |

On GitHub, open those two PDFs to see the compiled paper and a highlighted revision without installing anything.

**On your own papers**, commit sources (`.tex`, `.bib`, figures, class files). Do not commit every rebuild: auxiliaries (`.aux`, `.bbl`, `.log`, `.synctex.gz`) stay ignored, and extra PDFs in `build/` or `diffs/` stay ignored unless you whitelist them. This starter only tracks the two sample PDFs above.

---

## Prerequisites checklist

After installing, every machine should succeed with:

```bash
git --version
perl --version
pdflatex --version
latexmk --version
latexdiff --version
git latexdiff --help
```

Then install the **LaTeX Diff** extension from a `.vsix` (not yet on the Marketplace). See [Revision PDFs](#4-revision-pdfs-latex-diff).

---

## 1. Install by operating system

### Windows

1. **Git:** [Git for Windows](https://git-scm.com/). Verify: `git --version`.
2. **Perl:** [Strawberry Perl](https://strawberryperl.com/) (needed by `latexdiff`). Verify: `perl --version`.
3. **LaTeX:** [MiKTeX](https://miktex.org/). Enable automatic package installs for all users.
4. **git-latexdiff:** MiKTeX Console package, **or** [git-latexdiff](https://gitlab.com/git-latexdiff/git-latexdiff) → `windows_install.cmd`.
5. Verify: `git latexdiff --help`.

### macOS

1. **Git:** `xcode-select --install` or `brew install git`.
2. **Perl:** usually preinstalled. Verify: `perl --version`.
3. **LaTeX:** MacTeX, for example `brew install --cask mactex`. Open a **new** terminal so `/Library/TeX/texbin` is on `PATH`.
4. **git-latexdiff:** `brew install git-latexdiff` (`latexdiff` is typically already in MacTeX).
5. If VS Code cannot find `latexmk`, see [Troubleshooting](#7-troubleshooting).

### Linux (Ubuntu / Debian)

1. **Git:** `sudo apt update && sudo apt install git`
2. **Perl:** `sudo apt install perl` if needed
3. **LaTeX:** `sudo apt install texlive-full` for workshops (fewer missing-package surprises)
4. **latexdiff:** often in TeX Live; otherwise `sudo apt install latexdiff`
5. **git-latexdiff:** TeX Live, or:
   ```bash
   git clone https://gitlab.com/git-latexdiff/git-latexdiff.git
   cd git-latexdiff
   sudo make install
   ```

---

## 2. VS Code + LaTeX Workshop

1. Install [Visual Studio Code](https://code.visualstudio.com/) (or Cursor).
2. **File → Open Folder** on this project (or any other LaTeX + Git folder).
3. Install recommended extensions when prompted, or add them manually:
   - [LaTeX Workshop](https://marketplace.visualstudio.com/items?itemName=James-Yu.latex-workshop) — build, PDF preview, SyncTeX
   - [LTeX+](https://marketplace.visualstudio.com/items?itemName=ltex-plus.vscode-ltex-plus) ([GitHub](https://github.com/ltex-plus/vscode-ltex-plus)) — grammar/spell checking
4. Install **[LaTeX Diff](https://github.com/NoeSilva13/latex-diff)** from a GitHub [Release](https://github.com/NoeSilva13/latex-diff/releases) `.vsix` (**Extensions: Install from VSIX…**). It will not appear under recommended extensions until it is on the Marketplace.
5. Open the main `.tex` file and build (save, or **Build LaTeX project**). The PDF should appear under `build/`.

`.vscode/settings.json` sends Workshop output to `build/`. `.latexmkrc` does the same for `latexmk` on the command line so the two tools do not disagree.

**TeXstudio** can edit `.tex` files, but this guide only configures VS Code.

### Using a different template

Copy `.vscode/`, `.gitignore`, and `.latexmkrc` into your paper’s folder (or clone this repo and replace the sample `.tex` / `.bib` / figures). Point LaTeX Diff at your main file. You do not need `IEEEtran.*` unless you are writing for IEEE.

---

## 3. Several people, one repository

Everyone works on the **same GitHub repo**. There is no “send the .tex by email” step.

```text
  clone once
       │
       ▼
  ┌─────────────┐
  │ pull        │  get the latest commits
  │ edit+build  │  only sources; PDF is local
  │ commit      │  small, clear messages
  │ push        │  share with the group
  └─────────────┘
       │
       └── repeat (always pull before you start)
```

### First-time setup (one person)

1. Create a GitHub repository (private is typical for unpublished papers).
2. Push this project (or your own template) to that remote.
3. **Settings → Collaborators** (or a team) and invite co-authors.

### Everyone else

1. Command Palette → **Git: Clone** → paste the GitHub URL. Do **not** run `git init` on a project that already has a remote.
2. Open the cloned folder in VS Code, install extensions, compile once to confirm the stack works.

### Daily loop (VS Code Source Control)

Prefer the Source Control view over memorizing commands.

1. **Pull** (or Sync) before you type. If you skip this, you will fight merge conflicts later.
2. Edit `.tex`, `.bib`, or figures. Save and check the PDF in `build/`.
3. Stage, write a message such as `Add results for Figure 2`, **Commit**, **Push**.

For a short paper, working on `main` is enough. Use a branch only if someone will rewrite a whole section for several days.

### Who edits what

Git can merge most text. It cannot merge two people rewriting the **same paragraph** at the same time without a conflict.

- Agree informally (“I take Introduction, you take Methods”) when possible.
- Pull often. Small commits are easier to merge than one giant commit at the end of the week.
- If Git marks a conflict, VS Code shows both versions. Keep the combined text, save, **rebuild the PDF**, then commit the resolution.

In day-to-day writing, do not commit every new PDF. They change on every machine and clutter history. This repository is an exception: it keeps one manuscript PDF and one revision PDF so the GitHub page is a working demo.

### Tags for journal milestones

Tag versions you may need to compare later (submission, revision, accepted):

```bash
git tag submission-v1
git tag revision-r1
git push origin submission-v1 revision-r1
```

Compare those tags in LaTeX Diff when you write the response to reviewers.

---

## 4. Revision PDFs (LaTeX Diff)

### Option A — Extension (recommended)

[LaTeX Diff](https://github.com/NoeSilva13/latex-diff) runs `git latexdiff` from the editor. It does **not** install TeX or `git-latexdiff`; those must already be on `PATH`.

1. Download `latex-diff-*.vsix` from [Releases](https://github.com/NoeSilva13/latex-diff/releases) (or [build from source](https://github.com/NoeSilva13/latex-diff#build-the-vsix-from-source)).
2. **Extensions: Install from VSIX…** → reload.
3. Open a folder that is a Git repository.
4. Click the **LaTeX Diff** icon in the Activity Bar.
5. Set **Old commit**, **New commit**, and **Main .tex**. Keep `--whole-tree`, `--latexmk`, and `--ignore-latex-errors` on when the project has figures.
6. **Generate PDF**. Output goes to `diffs/` and opens in a tab.

Both revisions must already contain the main `.tex`. Comparing the repo’s first commit (README only) with a later commit will fail.

A ready-made example is in [`diffs/diff-sample.pdf`](diffs/diff-sample.pdf) (the last two manuscript commits: Results, then Conclusion). Recreate it in LaTeX Diff by picking those two commits under Options. **Last Commit** compares `HEAD` with your uncommitted edits.

Details: [LaTeX Diff README](https://github.com/NoeSilva13/latex-diff#use).

### Option B — Terminal / VS Code task

Previous commit vs `HEAD`:

```bash
git latexdiff --main LaTeX_Git_IEEE_Paper_Template.tex \
  --whole-tree --latexmk --build-dir build --ignore-latex-errors \
  --output diffs/diff-last.pdf HEAD~1 HEAD
```

`--build-dir build` is required because `.latexmkrc` writes PDFs under `build/`. Or run the VS Code task **Diff last commit**.

Change `--main` to your own file name when you are not using the IEEE sample.

---

## 5. Teaching tip (90-minute pilot)

1. Install and pass the checklist (20–30 min).
2. Clone, open in VS Code, build (15 min).
3. Edit one paragraph, commit, push; a second person pulls (15 min).
4. Another commit; generate a PDF with LaTeX Diff (20 min).
5. Optional: two people edit the same sentence, then resolve the conflict (10–15 min).

---

## 6. Customizing this sample

- Title, authors, and sections live in `LaTeX_Git_IEEE_Paper_Template.tex`.
- Add BibTeX entries to `refs.bib` and cite them with `\cite{key}`.
- Put graphics in `figures/` (`\graphicspath` is already set).
- To start a non-IEEE paper, replace the sample sources; keep `.gitignore`, `.vscode/`, and `.latexmkrc`.

---

## 7. Troubleshooting

| Symptom | What to try |
|---------|-------------|
| VS Code: `spawn latexmk ENOENT` (especially macOS) | Put `/Library/TeX/texbin` on `PATH`, restart the editor, or launch it from a terminal where `which latexmk` works. |
| Aux files (`.bbl`, `.synctex.gz`) appear in the project root | Leftovers from a compile that did not use `build/`. Delete them. Workshop + `.latexmkrc` should write new ones only under `build/`. |
| MiKTeX missing package | Allow on-the-fly installs, or install the package in MiKTeX Console. |
| `git latexdiff` / figures fail | Enable `--whole-tree` (default in LaTeX Diff). If the command says no PDF was generated, add `--build-dir build`. |
| Windows: Perl / latexdiff errors | Reinstall Strawberry Perl; `perl --version` in the same terminal you use for Git. |
| `git-latexdiff is not available` | Install it, confirm `git latexdiff --help`, restart the editor. |
| File does not exist in old revision | Old commit predates that `.tex`. Pick a later commit. |
| Bibliography empty | Full `latexmk` build; `refs.bib` next to the main `.tex`. |

---

## License / credits

The IEEE `IEEEtran` files are a **sample** for IEEE-style papers. `git-latexdiff` is maintained upstream ([GitLab](https://gitlab.com/git-latexdiff/git-latexdiff)). The [LaTeX Diff](https://github.com/NoeSilva13/latex-diff) extension is a separate repository.
