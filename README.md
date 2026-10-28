# eMudhra certificate incident: developer update guide

**Guidance checked: 26 October 2026.** For macOS and Windows, aimed at the Python/JavaScript projects we do in destiny.

> [!IMPORTANT]
> **Before running code that connects to the internet—especially in a virtual environment (venv)—check whether that environment still has tools or packages that need updating.**
>
> Updating tools globally does not necessarily update the copies inside each environment. Follow the relevant steps below for every environment you use.

## Part 1 — What to do

> [!IMPORTANT]
> You will not have all these tools. Only follow items for software you already use. Skip the rest. Do not install a program just because it appears in this guide.

Read the operating-system and browser guidance, then find your tools in the list below and work through those items. Project/CI/server items are for whoever maintains those systems. If you do not recognise a tool or know how it was installed, ask the person supporting that setup rather than trying every command.

### Order of work

Each row is a separate item. Sections 01–20 and 22–25 have matching **A-numbers in Part 2**, explaining the safety of the update method, its place in the order, and what it fixes. Section 26 lists other tools without update instructions.

| Step | Tool or task | Only applies if… |
| --- | --- | --- |
| [01](#01-operating-system-certificate-protection) | Operating-system certificate protection | Everyone. |
| [02](#02-browsers) | Browsers | Everyone: check the browser you use. Do not install all of these browsers. |
| [03](#03-macos-command-line-certificates) | macOS command-line certificates | You use Apple’s bundled curl, Git, Python or similar tools, supplied with macOS or Xcode Command Line Tools. |
| [04](#04-homebrew) | Homebrew | You already use Homebrew on macOS. |
| [05](#05-git-for-windows) | Git for Windows | Windows users with Git installed, including portable copies. |
| [06](#06-conda-miniconda-anaconda-and-miniforge) | Conda, Miniconda, Anaconda and Miniforge | You use any of these Conda distributions. |
| [07](#07-uv) | uv | You use uv. |
| [08](#08-python-installations) | Python installations | You run Python, including through Conda, uv, pyenv, Homebrew or python.org. |
| [09](#09-pip-in-each-python-environment) | pip in each Python environment | You install packages with pip. Repeat in each environment you use. |
| [10](#10-python-certificate-packages-in-each-environment) | Python certificate packages in each environment | An environment uses certifi or Conda ca-certificates for downloads/API calls. |
| [11](#11-nodejs-npmnpx-yarn-pnpm-and-corepack) | Node.js, npm/npx, Yarn, pnpm and Corepack | You use Node, npm/npx, Yarn, pnpm or Corepack, including project-selected copies. |
| [12](#12-virtualenv-the-tool-that-creates-python-environments) | virtualenv: the tool that creates Python environments | People who use pre-commit, tox or nox (which normally use virtualenv), or virtualenv directly. |
| [13](#13-pre-commit-recreate-saved-hook-environments) | pre-commit: recreate saved hook environments | You use pre-commit in a project or CI. |
| [14](#14-project-version-files-and-saved-environments) | Project version files and saved environments | You maintain project requirements, lockfiles, runtime settings or CI caches. |
| [15](#15-code-editors-and-ai-coding-tools) | Code editors and AI coding tools | You use a code editor or AI coding tool, such as VS Code, Codex or Claude Code. |
| [16](#16-github-cli) | GitHub CLI | You use GitHub CLI. |
| [17](#17-azure-cli) | Azure CLI | You use Azure CLI. |
| [18](#18-docker-desktop) | Docker Desktop | You use Docker Desktop. |
| [19](#19-github-actions) | GitHub Actions | You maintain GitHub Actions workflows. |
| [20](#20-notebook-kernels) | Notebook kernels | You run Python notebooks. |
| [21](#21-other-areas-to-remember-to-update) | Other areas to remember to update | You use containers, WSL or Azure environments. |
| [22](#22-electron-apps-you-develop) | Electron apps you develop | People who develop and distribute Electron apps that make network connections. |
| [23](#23-python-publishing-and-signing-tools) | Python publishing and signing tools | People who use Python Sigstore or PyPA's GitHub Action for publishing to PyPI. |
| [24](#24-hugging-face) | Hugging Face | You use Hugging Face’s Python tools to transfer models or datasets. |
| [25](#25-uv-tools) | uv tools | You use `uv tool install`, `uvx` or `uv tool run`. |
| [26](#26-other-tools) | Other tools | You use one of the additional tools listed. |

### Before running commands

- **Choose your existing installation method.** For example, update a Conda-installed Node with Conda. Installing another Node with Homebrew can leave the old copy in use.
- **Read the “Before” line.** Some methods need another item first. Where a section requires a confirmed download route, pause that item for the maintainer if it is still unconfirmed. Do not bypass certificate or signature errors.
- **Replace example names.** `myenv`, `APP`, `NAME`, filenames and image tags mean your actual environment, tool or file. Activate the intended Python environment before its Python/pip commands.
- **Use the right terminal.** Python/Conda/Node commands work in macOS Terminal or your normal Windows PowerShell/Conda prompt. macOS-only and Linux-only steps are labelled. Choose one matching installation method, not every alternative. Run separate command lines one at a time.
- **Keep the same supported release line.** For example, use the fixed Node 24.x version for Node 24. **≥ means “at least”.** A supplier-confirmed backport is also acceptable. Do not downgrade an already fixed newer version.

For official downloads, follow the browser guidance in 02. A normal update command is not automatically proof that an old downloader can safely obtain its own fix; any remaining condition is stated in its item/A-entry.

### 01. Operating-system certificate protection

**Applies to:** Everyone.

**No manual action is normally needed on Windows or macOS.** Certificate-blocking updates arrive automatically.

For command-line tools supplied with macOS or Xcode Command Line Tools, see [03](#03-macos-command-line-certificates).

[Evidence for automatic certificate protection → A01](#a01-operating-system-certificate-protection)

### 02. Browsers

**Applies to:** Everyone: check the browser you use. Do not install all of these browsers.

**Before:** Keep normal browser/security updates enabled.

| Browser | Update guidance |
| --- | --- |
| Chrome | Certificate-blocking updates arrive automatically. |
| Firefox | Certificate-blocking updates arrive automatically. |
| Safari | Certificate-blocking updates arrive automatically. |
| Microsoft Edge | Certificate-blocking updates arrive automatically. |
| Other browser | Check your browser vendor’s update guidance. |

If you’d like to double-check, see the [optional browser checks in the appendix](#connection-tests).

[Why this update method is safe, and what it covers → A02](#a02-browsers)

### 03. macOS command-line certificates

**Applies to:** macOS users who use Apple’s bundled versions of **curl, Git or Python (`python3`)**, including those supplied with Xcode Command Line Tools.

These bundled copies are separate from versions you install yourself, for example through Conda or uv. **Use the commands below only if you use the Apple-provided copies.** If you only use separately installed copies with their own certificate lists, skip this section and follow their update steps below.

**Virtual environments:** one created from Apple’s Python still uses that bundled Python; creating an environment alone does not replace it with a separately installed Python.

**Status — 26 October 2026:** Apple still describes the software fix as pending. These commands are its optional interim workaround.

**Before:** You need an administrator account. Removing either root can disrupt some unrelated websites in these tools. Keep the backup and stop if any command reports an error.

**1. Keep a backup:**

```bash
[ -e /etc/ssl/cert.pem.orig ] || sudo cp -p /etc/ssl/cert.pem /etc/ssl/cert.pem.orig
```

**2. Remove the emSign G3 root:**

```bash
t=$(mktemp) && awk -v s='=== /C=IN/OU=emSign PKI/O=eMudhra Technologies Limited/CN=emSign ECC Root CA - G3' '/^(===|###) /{skip=0} $0==s{skip=1} !skip' /etc/ssl/cert.pem > "$t" && [ -s "$t" ] && sudo install -m 0644 -o root -g wheel "$t" /etc/ssl/cert.pem; rm -f "$t"
```

**3. Optional: remove AAA Certificate Services to block the affected cross-signed certificates too:**

```bash
t=$(mktemp) && awk -v s='=== /C=GB/ST=Greater Manchester/L=Salford/O=Comodo CA Limited/CN=AAA Certificate Services' '/^(===|###) /{skip=0} $0==s{skip=1} !skip' /etc/ssl/cert.pem > "$t" && [ -s "$t" ] && sudo install -m 0644 -o root -g wheel "$t" /etc/ssl/cert.pem; rm -f "$t"
```

**4. Check:**

```bash
openssl crl2pkcs7 -nocrl -certfile /etc/ssl/cert.pem | openssl pkcs7 -print_certs -noout | grep -E 'emSign ECC Root CA - G3|AAA Certificate Services'
```

**Expected:** no output if both roots were removed. If you skipped 3, AAA Certificate Services may still appear. A certificate-file parsing error is a problem: follow Apple’s backup-restoration instructions. **Recheck after macOS updates until Apple’s permanent fix is installed.**

[Evidence for this workaround and its limitations → A03](#a03-macos-command-line-certificates)

### 04. Homebrew

**Applies to:** macOS users who already use Homebrew.

**Before:** Complete **both root-removal steps in [03](#03-macos-command-line-certificates)** before downloading with Homebrew. This protects Apple’s curl, which Homebrew uses on a standard macOS setup. For a setup using another downloader or a custom certificate list, verify that route’s protection instead; see [A04](#a04-homebrew).

Run:

```bash
brew update && brew upgrade ca-certificates
```

Homebrew upgrades an older package and skips one that is already current. If it reports **`ca-certificates not installed`**, there is no package to upgrade; continue to the next applicable section. For other errors or a skipped pinned package, stop and resolve the problem. Follow the later sections for other tools you use.

[Evidence for this update method and its prerequisites → A04](#a04-homebrew)

### 05. Git for Windows

**Applies to:** Windows users who use a standard Git for Windows installation.

Run in Command Prompt or PowerShell:

```bash
git update-git-for-windows
```

Accept the update and follow the installer prompts.

[Evidence for the update methods and download protection → A05](#a05-git-for-windows)

### 06. Conda, Miniconda, Anaconda and Miniforge

**Applies to:** People who use Conda through Miniconda, Anaconda or Miniforge.

> [!WARNING]
> **We have not independently verified that this update download is protected against the certificate incident.**
>
> An unupdated Conda installation can still trust the affected certificates while downloading the update. This is the update recommended by Conda’s maintainers. [Source](https://conda.org/blog/2026-10-21-emudhra-security-updates/#conda-2694).

To update Conda, run in your usual Conda terminal:

```bash
conda install --name base "conda>=26.9.4"
```

Repeat for separate Conda installations. For other environments, follow [10](#10-python-certificate-packages-in-each-environment).

[Update evidence and verification limits → A06](#a06-conda-miniconda-anaconda-and-miniforge)

### 07. uv

**Applies to:** People who use uv.

**Before:** For a direct installation on macOS, complete both root removals in [03](#03-macos-command-line-certificates). Windows uses the automatic protection in [01](#01-operating-system-certificate-protection). For Homebrew, complete [04](#04-homebrew); for pip, complete [09](#09-pip-in-each-python-environment) first.

For direct installations, check `uv --version` first. If it is below **0.11.2**, see [A07’s older-version note](#a07-uv).

| How uv was installed | Update — choose one |
| --- | --- |
| Direct installation — macOS or Windows | `uv self update --system-certs` |
| Homebrew | `brew upgrade uv` |
| pip (complete [09](#09-pip-in-each-python-environment) first) | `python -m pip install --upgrade "uv>=0.13.6"` |

**Check:** `uv --version` should now report **0.13.6 or later**.

[Update evidence and download protection → A07](#a07-uv)

### 08. Python installations

> [!IMPORTANT]
> **These instructions do not apply to Python bundled with macOS or Xcode Command Line Tools.** Apple’s certificate fix for those command-line tools is still awaiting a macOS update. See [A03’s explanation and workaround guidance](#a03-macos-command-line-certificates).

**Applies to:** People who use Python installed through Conda, uv, pyenv, Homebrew or python.org.

**Before:** Complete the relevant update first: [Conda 06](#06-conda-miniconda-anaconda-and-miniforge), [Homebrew 04](#04-homebrew), or [uv 07](#07-uv).

**For pyenv on macOS:** Complete **both root removals in [03](#03-macos-command-line-certificates)** first if you use Apple’s Git or curl. Complete [04](#04-homebrew) before updating a Homebrew-installed pyenv.

Update each Python installation you use, keeping the same release line (for example, 3.12 stays on 3.12).

| Python line | Minimum version with the certificate-key block |
| --- | --- |
| 3.11 | 3.11.18 |
| 3.12 | 3.12.16 |
| 3.13 | 3.13.18 |
| 3.14 | 3.14.10 |
| 3.15 | 3.15.2 |

| How Python was installed | Update — choose the matching method |
| --- | --- |
| Downloaded by uv | **Run once to update all Python versions downloaded by uv.**<br><br>`uv python upgrade` |
| Conda — Python 3.12 example | **Run for each Conda environment you use, including `base`.** Replace `myenv` with its name.<br><br>`conda install --name myenv "python>=3.12.16,<3.13"` |
| pyenv — Python 3.12 example | **Repeat for each Python release line you use**, replacing `3.12` with that line.<br><br>First update pyenv once using your existing method: **Homebrew:** `brew upgrade pyenv`; **pyenv installer or Git:** `git -C "$(pyenv root)" pull --ff-only`.<br><br>Then install the latest patch for that line: `pyenv install -s 3.12`. |
| Homebrew — example for existing Python 3.12 | `brew upgrade python@3.12` |
| python.org — Python 3.14 or 3.15 | Run the official [3.14.10 installer](https://www.python.org/downloads/release/python-31410/) or [3.15.2 installer](https://www.python.org/downloads/release/python-3152/), matching your existing release line. |

**Existing uv environments:** Those tied to an exact older Python version may keep using it and need a separate environment update. Project version settings and lockfiles are covered in [14](#14-project-version-files-and-saved-environments).

**Existing pyenv projects and environments:** The command installs a new Python, but your existing projects may still use an old one. Recreate affected virtual environments using the updated Python, reinstall their packages and switch your projects to those replacements.

Python 3.10 or older no longer receives upstream fixes. Get help choosing a supported build if your existing supplier cannot provide the fix; do not change release lines just to copy an example.

[Update evidence and download protection → A08](#a08-python-installations)

### 09. pip in each Python environment

> [!IMPORTANT]
> **These instructions do not apply to pip bundled with Apple/Xcode’s Python.** Apple’s macOS certificate update for command-line tools is still pending; its notice does not confirm a fix for bundled pip. See [A09’s explanation](#apples-bundled-pip).

**Applies to:** People who use pip in separately managed Python installations or environments. **Repeat in each environment you use.**

**Before:** Activate the environment and complete its Python update in [08](#08-python-installations). If that environment’s Python has not received the fix, or you are unsure, use [A09’s alternative update](#pip-update-before-the-python-fix) instead.

**Run in each Python environment you use:**

```bash
python -m pip install --upgrade pip
```

[Update evidence and download protection → A09](#a09-pip-in-each-python-environment)

### 10. Python certificate packages in each environment

**Applies to:** People whose Python environments use `certifi` or Conda’s `ca-certificates` to connect to websites or services. **Repeat for each environment you use.** Skip environments that use neither package.

**Before:** For pip, complete [09](#09-pip-in-each-python-environment) in that environment. For Conda, complete [06](#06-conda-miniconda-anaconda-and-miniforge). Use the same installer and channel as the existing package.

**pip-installed certifi — activate the environment, then run:**

```bash
python -m pip install --upgrade "certifi>=2026.10.19"
```

**Conda — choose the environment’s existing channel and replace `myenv` with its name. Run for each environment:**

```bash
# conda-forge
conda install --name myenv --override-channels --channel conda-forge "certifi>=2026.10.19" "ca-certificates>=2026.10.19"
```

```bash
# Anaconda main / defaults
conda install --name myenv --override-channels --channel defaults "certifi>=2026.10.19" "ca-certificates>=2026.10.20"
```

Do not replace a Conda-installed package with pip. If an older Python or unsupported platform prevents the update, see [A10’s compatibility note](#a10-python-certificate-packages-in-each-environment).

[Update evidence and download protection → A10](#a10-python-certificate-packages-in-each-environment)

### 11. Node.js, npm/npx, Yarn, pnpm and Corepack

**Applies to:** People who use Node.js, npm/npx, Yarn, pnpm or Corepack. Update each Node installation or environment you use.

**Before:** Complete [04](#04-homebrew) for Homebrew or [06](#06-conda-miniconda-anaconda-and-miniforge) for Conda. For NVM, follow [01](#01-operating-system-certificate-protection) and, if [03](#03-macos-command-line-certificates) applies, complete **both root removals** there first. Download official installers through the browser described in [02](#02-browsers).

| Existing Node line | Minimum fixed version |
| --- | --- |
| 22 | 22.23.4 |
| 24 | 24.22.1 |
| 26 | 26.12.1 |

Use your existing installation method. The command examples below are for **Node 24**; use the corresponding fixed version for your existing line. Older, unsupported Node lines need a supported replacement.

| Installed with | Update |
| --- | --- |
| Official installer | Run the installer for your system from the fixed release: [Node 22](https://nodejs.org/en/blog/release/v22.23.4/), [Node 24](https://nodejs.org/en/blog/release/v24.22.1/) or [Node 26](https://nodejs.org/en/blog/release/v26.12.1/). |
| Conda — repeat for each Node environment; replace `myenv` | `conda install --name myenv "nodejs>=24.22.1,<25"` |
| Homebrew `node@24` | `brew upgrade node@24` |
| NVM | `nvm install 24` then `nvm use 24` |

For fnm or Volta, have the maintainer confirm the downloader’s protection first; see [A11](#a11-nodejs-npmnpx-yarn-pnpm-and-corepack).

Restart programs using Node. In each environment or editor terminal, check `node --version` against the minimum above. npm/npx, Yarn, Corepack and the JavaScript versions of pnpm inherit the fix **when they run on that updated Node**.

**Standalone pnpm needs its own update.** For pnpm 10/11 with a bundled Node runtime, use **10.34.8 / 11.28.8 or later** in the same line. For native pnpm 12, use **12.15.0 or later**; A11 explains the source evidence for this target. Use your original installation method:

| pnpm installation | Update |
| --- | --- |
| pnpm 12 installed with npm — update Node above first | `npm install -g "pnpm@^12.15.0"` |
| pnpm 12 installed with Homebrew — complete 04 first | `brew upgrade pnpm` |
| Standalone installer | Rerun [pnpm’s official installer](https://github.com/pnpm/get.pnpm.io#configuring), setting `PNPM_VERSION` to the matching version above. Apple’s curl needs both root removals in [03](#03-macos-command-line-certificates) first; Windows PowerShell uses [01](#01-operating-system-certificate-protection). Other downloaders need their own confirmed protection. |

Reopen the terminal and check `pnpm --version` if you updated pnpm. Project version pins are covered separately in [14](#14-project-version-files-and-saved-environments).

[Update evidence and download protection → A11](#a11-nodejs-npmnpx-yarn-pnpm-and-corepack)

### 12. virtualenv: the tool that creates Python environments

**Applies to:** People who use pre-commit, tox or nox (which normally use virtualenv), or virtualenv directly.

**Before:** Update your package manager, for example [09](#09-pip-in-each-python-environment) for pip or [06](#06-conda-miniconda-anaconda-and-miniforge) for Conda.

**Repeat for each environment where pre-commit, tox, nox or virtualenv is installed.** Activate that environment, then use your existing installer to update virtualenv to **21.14.12 or later**. For example, with pip:

```bash
python -m pip install --upgrade "virtualenv>=21.14.12"
```

[Update evidence and download protection → A12](#a12-virtualenv-the-tool-that-creates-python-environments)

### 13. pre-commit: recreate saved hook environments

**Applies to:** People who use pre-commit in a project or CI. Skip this section if you do not.

**Before:** Complete the applicable Git steps in [03](#03-macos-command-line-certificates)/[05](#05-git-for-windows), and update pre-commit’s own Python and virtualenv using [08](#08-python-installations) and [12](#12-virtualenv-the-tool-that-creates-python-environments). Correct old runtime settings using [14](#14-project-version-files-and-saved-environments).

**1. Clear pre-commit’s shared cache once.** This removes saved hook environments for all projects using that cache:

```bash
pre-commit clean
```

**2. In each project you use that has `.pre-commit-config.yaml`, rebuild its hook environments:**

```bash
pre-commit install-hooks
```

Keep the existing hook revisions. If you use pre-commit in CI, refresh its saved cache too; see [19](#19-github-actions).

[Rebuild evidence and version checks → A13](#a13-pre-commit-recreate-saved-hook-environments)

### 14. Project version files and saved environments

**Applies to:** You maintain project requirements, lockfiles, runtime settings or CI caches.

**Before:** Fix the installer/runtime that will regenerate the files first. For tools installed or run with uv, complete [25](#25-uv-tools) before using them here.

1. Correct active requirements that would reinstall an affected version. For Python, update any old version pins in `.python-version`, `pyproject.toml` or environment definitions to allow a [fixed version from 08](#08-python-installations). Refresh affected lockfiles with the project’s usual tool.
2. Use the existing project tool to update affected packages, not every dependency:

| Project tool | Existing certifi example |
| --- | --- |
| uv | `uv lock --upgrade-package certifi`, then `uv sync --locked` |
| pip-tools | `pip-compile --upgrade-package certifi`, with the project’s normal file arguments; then its normal sync |
| Conda | Update the existing environment definition and regenerate it through the project’s normal workflow. |

3. Refresh affected tool/image caches after fixing their source settings. Keep unrelated data and historical experiment records.

[Why this update method is safe, and what it covers → A14](#a14-project-version-files-and-saved-environments)

### 15. Code editors and AI coding tools

**Applies to:** People who use a code editor or AI coding tool, such as **VS Code, Cursor, Windsurf, Codex or Claude Code**.

**Update the coding tools you already use**, following their official update instructions and keeping your existing installation method, then fully restart them. If they have already updated automatically, you do not need to reinstall them.

[Why application updates matter, and confirmed fixes → A15](#a15-code-editors-and-ai-coding-tools)

### 16. GitHub CLI

**Applies to:** People who use GitHub CLI (`gh` commands, not ordinary `git` commands).

Update GitHub CLI using your existing installation method. For example:

- **macOS — Homebrew:** `brew upgrade gh`
- **Windows — WinGet:** `winget upgrade --id GitHub.cli --source winget`

[Update evidence → A16](#a16-github-cli)

### 17. Azure CLI

**Applies to:** People who use Azure CLI (`az` commands).

**UPDATE PENDING — no fixed release is available yet.** The certificate fix is expected on **Tuesday 3 November 2026**.

[Release status → A17](#a17-azure-cli)

### 18. Docker Desktop

**Applies to:** People who use Docker Desktop.

**Update Docker Desktop to 4.96.3 or later**, then restart it. Download the official [macOS](https://docs.docker.com/desktop/setup/install/mac-install/) or [Windows](https://docs.docker.com/desktop/setup/install/windows-install/) installer through your browser.

[Update evidence → A18](#a18-docker-desktop)

### 19. GitHub Actions

**Applies to:** People who maintain GitHub Actions workflows.

1. Apply the relevant earlier updates, in the same order, to software your workflows use. Correct saved version settings using [14](#14-project-version-files-and-saved-environments) so future runs use the fixes.
2. After updating, delete affected GitHub Actions caches, including saved pre-commit environments: open the repository’s [**Actions → Caches**](https://docs.github.com/en/actions/how-tos/manage-workflow-runs/manage-caches#deleting-cache-entries) and click the trash icon beside each affected entry.

[Why workflow updates and cache refreshes matter → A19](#a19-github-actions)

### 20. Notebook kernels

**Applies to:** People who run Python notebooks.

1. Complete [08](#08-python-installations)–[10](#10-python-certificate-packages-in-each-environment) in each environment your notebooks use.
2. If any notebook kernels were already running when you made the updates, save your work and restart those kernels.

[Why the selected environment and restart matter → A20](#a20-notebook-kernels)

### 21. Other areas to remember to update

**Applies to:** People who run code in, or maintain, the environments listed below.

Apply the relevant earlier updates, in the same order, to software used in:

- Docker images and development containers
- WSL environments
- Azure environments

**Linux:** WSL and Linux containers also need their distribution's certificate/security updates. Follow the distribution's incident guidance; Linux-specific steps are not included here.

### 22. Electron apps you develop

**Applies to:** People who develop and distribute Electron apps that make network connections.

**Before:** Complete [11](#11-nodejs-npmnpx-yarn-pnpm-and-corepack) to protect the downloads.

Update the project's Electron dependency to a fixed release, keeping your existing major version where supported:

| Electron line | First fixed stable release |
| --- | --- |
| 42 | 42.11.16 |
| 43 | 43.8.1 |
| 44 | 44.9.1 |

Example for an npm project using Electron 43:

```bash
npm install --save-dev electron@43.8.1
```

Later releases on these lines also include the fix. Update the lockfile, rebuild and test the app, then sign and distribute it through your normal release process.

[Why this update method is safe, and what it covers → A22](#a22-electron-apps-you-develop)

### 23. Python publishing and signing tools

**Applies to:** People who use Python Sigstore to sign or verify releases, or PyPA's GitHub Action to publish to PyPI.

**Before:** For Sigstore, complete 08–10 in its Python environment. For the publishing Action, follow [19](#19-github-actions).

| Tool | Do this |
| --- | --- |
| Python Sigstore | Update to **4.5.1 or later** using its existing installer. For pip: `python -m pip install --upgrade "sigstore>=4.5.1"`. |
| PyPA publishing Action | Update the workflow to `pypa/gh-action-pypi-publish@v1.14.3` or a later fixed release. Keep using a verified full commit SHA if your team requires it. |

[Why this update method is safe, and what it covers → A23](#a23-python-publishing-and-signing-tools)

### 24. Hugging Face

**Applies to:** People who use Hugging Face’s Python tools to download or upload models or datasets.

> [!IMPORTANT]
> **UPDATE PENDING — no fixed release is available yet.**
>
> Hugging Face has prepared the fix. You will need to update once it is released.

[What the fix covers and its release status → A24](#a24-hugging-face)

### 25. uv tools

**Applies to:** People who install tools with `uv tool install` or run them with `uvx` or `uv tool run`. Skip this section if you only use uv for project environments.

**Before:** Complete the uv update in [07](#07-uv) and the Python updates in [08](#08-python-installations).

**Run both commands once, in this order:**

```bash
uv tool upgrade --all
uv cache clean
```

> [!NOTE]
> If you don’t want to clear your uv cache, use [the alternative in A25](#alternative-without-clearing-the-uv-cache).

[Update evidence and download protection → A25](#a25-uv-tools)

### Final Checklist

- [ ] Relevant tools updated; unused tools marked **not applicable**.
- [ ] The actual program/environment was checked, not a different installed copy.
- [ ] Current project files/caches will not bring the old version back.
- [ ] Running apps/jobs were restarted or redeployed where needed.
- [ ] Relevant application/service connection tests pass; unconfirmed vendor/runtime checks remain **pending confirmation**.

### 26. Other tools

I wasn’t sure whether anyone on the team uses these tools, so I haven’t included update instructions. They also need updating; let me know if you’d like me to add instructions.

- [AWS CLI v2](https://raw.githubusercontent.com/aws/aws-cli/v2/CHANGELOG.rst) — certificate fix in **2.38.4 or later**.
- [Google Cloud CLI](https://cloud.google.com/sdk/docs/release-notes) — certificate fixes in **590.0.1 or later**.
- [Poetry](https://python-poetry.org/docs/#installation)
- [Pipenv](https://pipenv.pypa.io/en/latest/changelog.html#pipenv-2026-10-2-2026-10-21)
- [pipx](https://pipx.pypa.io/stable/explanation/how-pipx-works.html)
- [Mamba](https://conda.org/blog/2026-10-21-emudhra-security-updates/#mamba)
- [Conda’s libmamba solver](https://conda.org/blog/2026-10-21-emudhra-security-updates/#conda-libmamba-solver-and-conda-pypi)
- [Micromamba](https://mamba.readthedocs.io/en/latest/installation/micromamba-installation.html)
- [Bun](https://github.com/oven-sh/bun/security/advisories/GHSA-26q8-h657-pf84)
- [Deno](https://github.com/denoland/deno/releases/tag/v2.9.8)
- [Pixi](https://github.com/prefix-dev/pixi/releases/tag/v0.82.1)

## Part 2 — Explanations and supporting evidence

This section explains the guidance in Part 1. Claims, linked sources and short quotes show what the recommended actions cover and the evidence for carrying them out safely. It also includes alternative instructions and identifies pending fixes or gaps in the evidence. **A-numbers match the sections in Part 1**: A03 explains section 03, for example.

### A01. Operating-system certificate protection

**Checked: 26 October 2026.**

| Claim | Sources | Quotes |
| --- | --- | --- |
| **macOS:** Apple delivers certificate-blocking updates automatically, separately from macOS releases. Apple confirms that the eMudhra block has been active since 19 October for Safari, Mail and apps using its networking and security frameworks. | [Apple’s incident guidance](https://support.apple.com/en-us/149555) | “Updates to certificate configuration data are sent to devices automatically and as needed” |
| **Windows:** trusted and blocked certificate lists update automatically by default. Tanium inspected Microsoft’s block list dated 19 October and found all eight compromised keys included. | [Microsoft’s certificate-update documentation](https://learn.microsoft.com/en-us/windows-server/identity/ad-cs/certificate-trust)<br><br>[Tanium’s inspection, reported 24 October 2026](https://www.tanium.com/blog/emudhra-ca-compromise-find-and-fix-your-exposure-with-tanium) | **Microsoft:** “By default, Windows downloads the CTLs from the Internet via an automatic mechanism called the CTL Updater.”<br><br>**Tanium:** “dated October 19, lists all eight compromised keys” |

**Scope.** Automatic delivery requires internet access and enabled certificate updates. Apple’s separate command-line certificate fix is still pending; see [03](#03-macos-command-line-certificates).

[Back to action 01](#01-operating-system-certificate-protection)

### A02. Browsers

**Checked: 26 October 2026.**

| Claim | Sources | Quotes |
| --- | --- | --- |
| **Chrome:** the eMudhra certificate block was delivered automatically, without a browser update or restart. | [Google’s incident notice](https://blog.google/security/protecting-chrome-and-android-users-from-compromised-emudhra-intermediate-ca-certificates/) | “Chrome users were immediately protected against all certificates sharing those keys, without requiring a browser update or restart.” |
| **Firefox:** users with normal settings received the eMudhra certificate block automatically through OneCRL, without updating or restarting Firefox. | [Mozilla’s incident notice](https://blog.mozilla.org/security/2026/10/20/protecting-firefox-users-following-the-compromise-of-emudhra-intermediate-certificates/) | “OneCRL delivers this protection without requiring users to update or restart Firefox.” |
| **Safari:** Apple automatically delivers the certificate blocks that protect Safari against this incident. | [Apple’s incident guidance](https://support.apple.com/en-us/149555) | “Your Apple devices now automatically block these certificates in Safari” |
| **Microsoft Edge:** certificate-revocation lists receive automatic security updates, including when component updates are disabled. Independent tests found the eMudhra block working. | [Microsoft’s component-update policy](https://learn.microsoft.com/en-us/deployedge/microsoft-edge-policies/componentupdatesenabled)<br><br>[FairSSL’s Windows 11 tests, 21 October 2026](https://www.fairssl.dk/en/articles/emudhra-ca-noegler-knaekket/)<br><br>[Tanium’s lab report, 24 October 2026](https://www.tanium.com/blog/emudhra-ca-compromise-find-and-fix-your-exposure-with-tanium) | **Microsoft:** “updates that are deemed "critical for security" are still applied even if you disable this policy.” The examples include “certificate revocation lists”.<br><br>**FairSSL:** “on the evening of 21-10-2026 Edge blocked them in our test.”<br><br>**Tanium:** “In an internal Tanium lab, these versions showed as protected” … “Edge with CRLSet 6498.2026.10.20” |

**Edge evidence.** FairSSL and Tanium describe protection on tested installations; a separate Microsoft incident announcement was not found in this check. Tanium’s version number identifies the certificate-revocation component, not the browser.

**Scope.** Automatic delivery requires internet access. Mozilla notes that unusual settings can prevent its background update and provides fixed browser releases for those users.

[Back to action 02](#02-browsers)

### A03. macOS command-line certificates

**Checked: 26 October 2026.**

| Claim | Sources | Quotes |
| --- | --- | --- |
| **Command provenance:** all four commands in 03 match Apple’s published eMudhra response exactly: backup, two root removals and verification, in that order. | [Apple’s incident instructions, steps 1–4](https://support.apple.com/en-us/149555) | “Copy each command exactly as written.” |
| **Download safety — command inspection:** the commands use installed tools to back up, edit and check local files. They do not download software or certificates, so executing this repair does not depend on an affected HTTPS connection. | [Apple’s complete command sequence, inspected for downloads](https://support.apple.com/en-us/149555) | Local-file excerpt: `cp -p /etc/ssl/cert.pem`. The no-download conclusion comes from inspecting the complete sequence. |
| **Protection provided:** removing emSign G3 and AAA Certificate Services blocks the affected direct and cross-signed TLS chains through this file. Step 2 alone leaves the cross-signed route. | [Apple’s explanation and removal steps 2–3](https://support.apple.com/en-us/149555) | “also remove AAA Certificate Services” |
| **Affected versions:** Apple-provided tools that check certificates against `/etc/ssl/cert.pem`, including curl, Git and Python. | [Apple’s affected-tool guidance](https://support.apple.com/en-us/149555) | “macOS or the Xcode Command Line Tools” |
| **Fix pending:** Apple still describes the permanent software fix as forthcoming; these commands are its interim mitigation. | [Apple’s incident guidance](https://support.apple.com/en-us/149555) | “future software update” |

[Back to action 03](#03-macos-command-line-certificates)

### A04. Homebrew

**Checked: 27 October 2026.**

| Claim | Sources | Quotes |
| --- | --- | --- |
| **Commands:** 04 applies Homebrew’s documented update sequence to `ca-certificates`. | [Homebrew’s update instructions](https://docs.brew.sh/FAQ#how-do-i-update-my-local-packages) | `brew update`; `brew upgrade <formula>`. |
| **Coverage:** The fixed package (`2026-10-20`) removes AAA Certificate Services and the emSign G3 roots from Homebrew’s certificate bundle, including certificates imported from macOS keychains. | [Maintainer’s incident confirmation](https://github.com/Homebrew/homebrew-core/pull/319756#issuecomment-6342608209)<br><br>[Released certificate filtering](https://github.com/Homebrew/homebrew-core/blob/ddecfafe263bc2b7c34528327a4f30b0bfbcc13b/Formula/c/ca-certificates.rb) | **Maintainer:** “yes this one fixes it with official upstream change”.<br><br>**Code:** “Exclude AAA Certificate Services and the emSign G3 roots by their public keys.” |
| **Download safety — our source assessment:** Complete both root removals in 03 first. This local repair blocks the affected chains for Apple’s curl/Git when they use the repaired certificate file. | [Apple’s affected-tool guidance and repair instructions](https://support.apple.com/en-us/149555) | “like curl and git”; “check certificates against /etc/ssl/cert.pem”; “also remove AAA Certificate Services”. |

**Other setups:** Separately installed downloaders and custom certificate lists need their own protection before either update command.

[Back to action 04](#04-homebrew)

### A05. Git for Windows

**Checked: 27 October 2026.**

| Claim | Sources | Quotes |
| --- | --- | --- |
| **Official command:** `git update-git-for-windows` checks for an update and offers to install it. | [Git for Windows’ update instructions](https://gitforwindows.org/faq.html#how-do-i-update-git-for-windows-upon-new-releases) | “you can manually run `git update-git-for-windows`”. |
| **Coverage:** 2.56.0(3) addresses this incident. Use the latest release, currently 2.56.0(4), which adds another security fix. | [Incident release, 2.56.0(3)](https://github.com/git-for-windows/git/releases/tag/v2.56.0.windows.3)<br><br>[Subsequent security release, 2.56.0(4)](https://github.com/git-for-windows/git/releases/tag/v2.56.0.windows.4) | **2.56.0(3):** “This is a security fix release, addressing the eMudhra Intermediate CA Compromise.”<br><br>**2.56.0(4):** “another urgent security bug fix release”. |
| **First-download protection — source assessment:** standard installations from 2.41 use Windows’ certificate checks for the updater’s curl downloads. That route benefits from Microsoft’s incident block once delivered to the device. | [Updater download code](https://github.com/git-for-windows/build-extra/blob/64870a8db99915da332835404d5f2e7bfbf0672a/git-extra/git-update-git-for-windows#L350-L351)<br><br>[Git for Windows’ switch to Windows TLS](https://github.com/git-for-windows/MINGW-packages/commit/1a51ba3fecbf09d36afba5e892614eabdb63f861)<br><br>[curl’s Windows certificate verification](https://curl.se/docs/sslcerts.html)<br><br>[Microsoft’s incident notice](https://www.microsoft.com/en-us/msrc/blog/2026/10/microsoft-response-to-compromised-emudhra-certificate-authority-keys) | **Updater:** `curl -# -L -o $installer $download`.<br><br>**Build change:** “Secure Channel-only variant by default”; “Release Git for Windows v2.41.0 with this setup.”<br><br>**curl:** “curl uses the certificates that are built into the OS.”<br><br>**Microsoft:** “On October 20, Microsoft published an update to the Windows Disallowed Certificate Trust List to block the affected certificate authorities.” |

**Limits:** The 2.41 boundary is our source assessment, not a cutoff stated in the vendor’s incident notice. Earlier or customised downloaders were not verified; use the [official installer](https://gitforwindows.org/) through the [protected browser from 02](#02-browsers) if their protection is uncertain. The updater script has no explicit installer checksum or signature check. Portable copies need the [official PortableGit download](https://github.com/git-for-windows/git/releases/latest); Git bundled inside another app needs that app’s update. This was a source review, not an execution test on Windows.

[Back to action 05](#05-git-for-windows)

### A06. Conda, Miniconda, Anaconda and Miniforge

**Checked: 27 October 2026.**

| Claim | Sources | Quotes |
| --- | --- | --- |
| **Command provenance:** 06 uses Conda’s incident command, changing `=26.9.4` to `>=26.9.4` so newer releases are allowed while the fixed minimum is required. | [Conda’s incident instructions](https://conda.org/blog/2026-10-21-emudhra-security-updates/#conda-2694)<br><br>[Conda’s version-constraint syntax](https://docs.conda.io/projects/conda/en/stable/user-guide/concepts/pkg-specs.html) | **Incident command:** `conda install --name base conda=26.9.4`.<br><br>**Syntax:** “Match versions greater than or equal to”. |
| **Coverage:** Conda 26.9.4 requires corrected `certifi` and `ca-certificates` packages in its own environment, normally `base`. Other environments still need 10. | [Conda’s fixed release](https://github.com/conda/conda/releases/tag/26.9.4)<br><br>[Incident guidance on other environments](https://conda.org/blog/2026-10-21-emudhra-security-updates/#update-ca-packages-separately) | **Release:** “update the packaged CA bundles following the eMudhra intermediate CA compromise”.<br><br>**Scope:** “Updating `base` does not update those bundles.” |
| **Compatibility:** the corrected packages require Python ≥3.10 on Anaconda main or ≥3.11 on conda-forge. Retired Anaconda main platforms, including Intel Macs (`osx-64`), need a supported distribution or platform. | [Conda’s release requirements](https://github.com/conda/conda/issues/16880)<br><br>[Incident guidance on unsupported platforms](https://conda.org/blog/2026-10-21-emudhra-security-updates/#update-ca-packages-separately) | “fixed certifi builds require Python >=3.10 in defaults and >=3.11 in conda-forge.”<br><br>“Retired defaults platforms have no updated CA bundles.” |

**Evidence limit:** Conda recommends the online update. We have not independently verified protection of its first download when the existing installation still trusts the affected certificates, and the incident notice does not explain that protection. This is a limit of our verification, not evidence that an update has been intercepted. The optional offline procedure below is our additional precaution, not a prerequisite required by Conda’s incident guidance.

Custom certificate files need separate repair; pip has its own update in [09](#09-pip-in-each-python-environment).

<details>
<summary>Optional maintainer procedure: offline certificate repair</summary>

Using the protected browser from 02, obtain `certifi` and `ca-certificates` from the installation’s existing official channel: [Anaconda main](https://repo.anaconda.com/pkgs/main/) or [conda-forge](https://conda.anaconda.org/conda-forge/). Match the platform and Python version. Use `certifi` ≥2026.10.19; use `ca-certificates` ≥2026.10.20 for Anaconda main or ≥2026.10.19 for conda-forge. Verify the files against checksums obtained through that protected route.

**Check dependencies before installing:** explicit local files bypass dependency solving. Conda’s [install documentation](https://docs.conda.io/projects/conda/en/stable/commands/install.html) says this mode “implies the --no-deps option”. Do not force incompatible packages; arrange a supported installation if no compatible corrected packages exist.

```bash
# Replace with compatible files obtained and verified by the maintainer.
conda install --offline --name base ./CA_PACKAGE.conda ./CERTIFI_PACKAGE.conda
```

Once the actual download route uses the corrected certificates, run the update in 06. If Conda is installed outside `base`, use its installation environment instead.

</details>

[Back to action 06](#06-conda-miniconda-anaconda-and-miniforge)

### A07. uv

**Checked: 27 October 2026.**

| Claim | Sources | Quotes |
| --- | --- | --- |
| **Self-update command and protection:** Astral maintainer William Woodruff recommends self-updating using the protected OS verifier. 07 substitutes the equivalent `--system-certs` flag so the same command works on macOS and Windows, once the protection in 01 has arrived. | [Maintainer’s incident-specific update advice](https://discuss.python.org/t/information-for-python-users-regarding-emudhra-emsign-ca-certificate-security-incident/109576/10)<br><br>[Astral’s equivalent certificate options](https://docs.astral.sh/uv/concepts/authentication/certificates/#system-certificates) | **Maintainer:** `UV_SYSTEM_CERTS=true uv self update`; “assumes that your system certificates/verifier are already updated”.<br><br>**Options:** “pass the `--system-certs` flag”; “set the `UV_SYSTEM_CERTS` environment variable to `true`”. |
| **Package-manager commands:** Homebrew and pip update their own uv installations after 04 or 09. The pip command adds a fixed minimum; the Homebrew command applies its documented upgrade syntax to uv. | [Astral’s package-manager update instructions](https://docs.astral.sh/uv/getting-started/installation/#upgrading-uv)<br><br>[Homebrew’s update instructions](https://docs.brew.sh/FAQ#how-do-i-update-my-local-packages)<br><br>[Homebrew download protection](#a04-homebrew) and [pip download protection](#a09-pip-in-each-python-environment) | **Astral:** `pip install --upgrade uv`.<br><br>**Homebrew:** `brew upgrade <formula>`. |
| **macOS installer downloads — source assessment:** self-update reruns an installer that makes further downloads with curl. When that is Apple’s bundled curl, both root removals in 03 protect those downloads. | [Astral’s update behavior](https://docs.astral.sh/uv/getting-started/installation/#upgrading-uv)<br><br>[Apple’s incident repair](https://support.apple.com/en-us/149555)<br><br>[Astral’s shell installer, inspected at 0.13.6](https://releases.astral.sh/github/uv/releases/download/0.13.6/uv-installer.sh) | **Astral:** “Updating uv will re-run the installer”.<br><br>**Apple:** “also remove AAA Certificate Services”.<br><br>**Installer:** `curl -sSfL "$1" -o "$2"`. |
| **Windows installer downloads — source assessment:** subsequent downloads by the PowerShell installer also use Windows’ certificate checks, benefiting from the incident block once delivered through 01. | [Astral’s Windows installer, inspected at 0.13.6](https://releases.astral.sh/github/uv/releases/download/0.13.6/uv-installer.ps1)<br><br>[Microsoft’s .NET TLS documentation](https://learn.microsoft.com/en-us/dotnet/framework/network-programming/tls)<br><br>[Microsoft’s incident response](https://www.microsoft.com/en-us/msrc/blog/2026/10/microsoft-response-to-compromised-emudhra-certificate-authority-keys) | **Installer:** `New-Object Net.Webclient`.<br><br>**.NET:** “.NET Framework is dependent on `Schannel` on Windows”.<br><br>**Incident response:** “Windows Disallowed Certificate Trust List to block the affected certificate authorities”. |
| **uv coverage:** versions before 0.13.4 use an affected default bundle. 0.13.4 removes the root; 0.13.5 adds blocking of the compromised intermediate keys. 0.13.6 includes both fixes. | [Astral’s incident advisory](https://github.com/astral-sh/uv/security/advisories/GHSA-hq85-5cpj-mr62)<br><br>[0.13.5 security release](https://github.com/astral-sh/uv/releases/tag/0.13.5) | **Advisory:** “uv 0.13.4 removes this root CA from its vendored CA bundle”.<br><br>**Release:** “Reject certificate chains containing compromised eMudhra intermediate keys”. |

**Older standalone versions:** Our minimum of 0.11.2 for this update route comes from [Astral’s release notes](https://github.com/astral-sh/uv/blob/main/changelogs/0.11.x.md#0112): “Use uv reqwest client for self update”. This is our compatibility boundary, not a cutoff stated in the incident advisory. For earlier versions, obtain the [official installer](https://docs.astral.sh/uv/getting-started/installation/#standalone-installer) through the protected browser in [02](#02-browsers); on macOS, complete 03 before running it.

[Back to action 07](#07-uv)

### A08. Python installations

**Checked: 28 October 2026.**

| Claim | Sources | Quotes |
| --- | --- | --- |
| **Python fix:** the five minimum versions in 08 block the compromised keys in Python’s `ssl` module, including normal pip connections, even with an older certificate bundle. | [Python’s incident advisory](https://discuss.python.org/t/information-for-python-users-regarding-emudhra-emsign-ca-certificate-security-incident/109576)<br><br>[Fixed-release announcement](https://discuss.python.org/t/python-3-15-2-3-14-10-3-13-18-3-12-16-and-3-11-18-are-out/109678) | **Advisory:** “regardless of whether the emSign root CA certificate is in use by ‘certifi’”.<br><br>**Release:** “These keys are now blocked by the `ssl` module”. |
| python-build checks downloaded source against bundled SHA-256 checksums when a supported checksum tool is available. Obtain pyenv and its checksums through the protected update route first. | [python-build checksum verification](https://github.com/pyenv/pyenv/blob/master/plugins/python-build/README.md#checksum-verification)<br><br>[Python 3.12.16 download definition](https://github.com/pyenv/pyenv/blob/master/plugins/python-build/share/python-build/3.12.16) | **Verification:** “verify the SHA2 checksum of each downloaded package before installing it”.<br><br>**Definitions:** “All bundled definitions include checksums.” |
| Installing a fixed Python with pyenv leaves existing Python installations and environments in place. They do not automatically switch to the new version. | [pyenv’s separate installations](https://github.com/pyenv/pyenv#locating-pyenv-provided-python-installations)<br><br>[pyenv-virtualenv’s environment creation](https://github.com/pyenv/pyenv-virtualenv#using-pyenv-virtualenv-with-pyenv) | **pyenv:** “Each Python version is installed into its own directory”.<br><br>**Environment example:** “will create a virtualenv based on Python 2.7.10”. |
| **Existing uv environments:** the shared Python upgrade alone does not move environments tied to an exact old patch or installation path. This applies to security patches too. | [Astral’s upgrade limitations](https://docs.astral.sh/uv/concepts/python-versions/#upgrading-python-versions) | “an explicitly requested patch version”; “it will not be transparently upgraded to a new version”. |
| **Older versions:** python.org’s 3.11–3.13 fixes are source-only; use a supplier’s fixed build. Python 3.10 and older no longer receive upstream security fixes. | [Python’s release announcement and maintainer’s explanation](https://discuss.python.org/t/python-3-15-2-3-14-10-3-13-18-3-12-16-and-3-11-18-are-out/109678)<br><br>[Python’s incident advisory](https://discuss.python.org/t/information-for-python-users-regarding-emudhra-emsign-ca-certificate-security-incident/109576) | **3.11–3.13:** “source-only security releases”.<br><br>**Older:** “Python versions 3.10 and older are end-of-life”. |

**Existing uv environments:** Environments that follow a Python release line normally pick up the updated patch automatically. Those tied to an exact older patch or installation path can keep using the old copy; they also need to be moved to a fixed Python. Project version settings and lockfiles are covered separately in [14](#14-project-version-files-and-saved-environments). Changing those files alone does not update an existing environment.

These examples cover pyenv; the separate [pyenv-win project](https://github.com/pyenv/pyenv#windows) uses different update commands.

[Back to action 08](#08-python-installations)

### A09. pip in each Python environment

**Checked: 27 October 2026.**

| Claim | Sources | Quotes |
| --- | --- | --- |
| **Fix:** pip 26.2.2 updates pip’s own certificate bundle. Update each environment’s copy. | [Python’s incident advisory](https://discuss.python.org/t/information-for-python-users-regarding-emudhra-emsign-ca-certificate-security-incident/109576)<br><br>[pip 26.2.2 release notes](https://pip.pypa.io/en/stable/news/) | **Advisory:** “For all existing virtual environments on your system”.<br><br>**pip release:** “Upgrade certifi to 2026.10.19”. |
| **Normal update:** the command in 09 is pip’s documented updater. Python’s fixed releases protect this download against the compromised keys, even before pip is updated. | [pip’s upgrade instructions](https://pip.pypa.io/en/stable/installation/#upgrading-pip)<br><br>[Python’s fixed-release announcement](https://discuss.python.org/t/python-3-15-2-3-14-10-3-13-18-3-12-16-and-3-11-18-are-out/109678) | **Command:** `python -m pip install --upgrade pip`.<br><br>**Python release:** “Normal uses of `pip`, including to upgrade `pip` itself, are now safe from attacks based on those compromised keys”. |
| **Alternative update:** the command below is copied from Python’s incident advisory. Its supplied SHA-256 checksum verifies the download even when old certificate trust is still affected. | [Python security guidance and exact command](https://discuss.python.org/t/information-for-python-users-regarding-emudhra-emsign-ca-certificate-security-incident/109576)<br><br>[pip’s checksum documentation](https://pip.pypa.io/en/stable/topics/secure-installs/) | **Security guidance:** “install securely, regardless of a MITM attack”.<br><br>**Checksum documentation:** “protect against remote tampering and network issues”. |
| **Older Apple Python:** Python 3.9 cannot install the fixed pip 26.2.2 release. | [pip 26.2.2’s Python requirement](https://pypi.org/project/pip/26.2.2/) | “Python >=3.10”. |
| **Apple update pending:** the fix for the certificate file used by Apple’s command-line tools is still awaiting a macOS update. | [Apple’s incident notice](https://support.apple.com/en-us/149555) | “This file will be updated in a future macOS update.” |

#### Apple’s bundled pip

The Apple Command Line Tools installation checked locally on **27 October 2026** contained **Python 3.9.6 and pip 21.2.4**. The fixed pip release requires Python 3.10 or later, so that Apple-provided copy cannot take the update. Updating pip does not upgrade the Python it runs on; other Macs may have different bundled versions.

Apple’s pending update concerns `/etc/ssl/cert.pem`. Its notice does not specifically confirm a fix for pip’s own certificate bundle, so we cannot assume that update—or the workaround in 03—will fix bundled pip. Use pip in a separately maintained Python environment instead of modifying Apple’s copy.

#### pip update before the Python fix

Use this in the activated environment if its Python fix is not confirmed. **If pip is already 26.2.2 or later, skip this command.**

**Compatibility:** pip 26.2.2 [requires Python ≥3.10](https://pypi.org/project/pip/26.2.2/). For older Python, follow 08. Use a separately maintained Python environment for older Apple/Xcode installations; do not use `sudo pip` or edit Apple’s Python.

Obtain the command and checksum through the protected browser described in [02](#02-browsers), and copy the entire command:

```bash
python -m pip install "pip @ https://files.pythonhosted.org/packages/3f/06/3e4420302363fbb71be3c7f35ce042f54bfca330e3e66b3a2e3c4cca579f/pip-26.2.2-py3-none-any.whl#sha256=fd7f4dfa8190e014013b9f08f3e973f4e4c26ffd179a204895533a7254813a70"
```

We checked the full command against the advisory and its checksum against [PyPI’s wheel details](https://pypi.org/project/pip/26.2.2/#files). pip checks the supplied checksum before installing the downloaded file.

[Back to action 09](#09-pip-in-each-python-environment)

### A10. Python certificate packages in each environment

**Checked: 27 October 2026.**

| Claim | Sources | Quotes |
| --- | --- | --- |
| **certifi update:** Python’s incident guidance recommends 2026.10.19 or later, using repaired pip first. Our command targets the active environment and adds that minimum. | [Python’s incident advisory and certifi update command](https://discuss.python.org/t/information-for-python-users-regarding-emudhra-emsign-ca-certificate-security-incident/109576) | **Minimum:** “certifi 2026.10.19”.<br><br>**Order:** “use this secure pip installation to upgrade certifi and uv”.<br><br>**Command excerpt:** `-m pip install --upgrade certifi`. |
| **Conda commands:** copied from the incident instructions, with `base` replaced by the environment’s name as directed. Both certificate bundles need updating in each environment that uses them. | [Conda’s incident instructions: update CA packages separately](https://conda.org/blog/2026-10-21-emudhra-security-updates/#update-ca-packages-separately) | **Both channels:** `"certifi>=2026.10.19"`.<br><br>**defaults:** `"ca-certificates>=2026.10.20"`.<br><br>**conda-forge:** `"ca-certificates>=2026.10.19"`.<br><br>**Other environments:** “Updating `base` does not update those bundles.” |
| **Conda download order:** completing 06 installs the corrected CA dependencies for subsequent downloads. This follows from Conda’s release requirements; A06 retains the first-update uncertainty. | [Conda 26.9.4’s requirements](https://conda.org/blog/2026-10-21-emudhra-security-updates/#conda-2694)<br><br>[First Conda update → A06](#a06-conda-miniconda-anaconda-and-miniforge) | **Conda:** “It requires the minimum CA package versions listed below.” |

**Compatibility:** Conda’s corrected certifi needs Python **3.10+ on defaults** or **3.11+ on conda-forge**. Defaults has no corrected CA packages for Intel macOS (`osx-64`) or the retired `linux-ppc64le` and `linux-s390x` platforms. [Conda’s guidance](https://conda.org/blog/2026-10-21-emudhra-security-updates/#update-ca-packages-separately) calls for a supported Python/platform or a distribution that supplies the fix. If this affects you, get help choosing that route; keep certificate verification enabled.

Saved requirements are covered in [14](#14-project-version-files-and-saved-environments).

[Back to action 10](#10-python-certificate-packages-in-each-environment)

### A11. Node.js, npm/npx, Yarn, pnpm and Corepack

**Checked: 27 October 2026.**

| Claim | Sources | Quotes |
| --- | --- | --- |
| **Node fix released:** 22.23.4, 24.22.1 and 26.12.1 address this incident. Tools using these runtimes’ normal TLS connections inherit the block; this is our inference from the Node patch. | [Node’s incident release announcement and downloads](https://nodejs.org/en/blog/vulnerability/october-2026-security-releases)<br><br>[Node’s certificate-verification patch](https://github.com/nodejs/node/pull/66871) | **Announcement:** “ca-cert (eMudhra Intermediate CA Compromise) on 26.x, 24.x, 22.x”.<br><br>**Patch:** “every SSL_CTX Node.js creates”. |
| **Official installers:** the linked releases provide Windows and macOS installers. Obtain them through the protected browser in 02, so the old Node runtime does not perform the download. | [Node 24’s official installer links](https://nodejs.org/en/blog/release/v24.22.1/)<br><br>[Browser protection evidence → A02](#a02-browsers) | **Release page:** “Windows 64-bit Installer”; “Windows ARM 64-bit Installer”; “macOS 64-bit Installer”. |
| **Homebrew and Conda commands:** adapted from their documented updaters to select the existing Node package and fixed release line. Complete 04 or 06 first to protect their downloads. | [Homebrew upgrade command](https://docs.brew.sh/Manpage#upgrade-options-installed_formulainstalled_cask-) · [node@24 package](https://formulae.brew.sh/formula/node@24)<br><br>[Conda install command](https://docs.conda.io/projects/conda/en/stable/commands/install.html)<br><br>[Homebrew protection → A04](#a04-homebrew) · [Conda protection and first-update limit → A06](#a06-conda-miniconda-anaconda-and-miniforge) | **Homebrew:** “Upgrade outdated, unpinned packages”.<br><br>**Conda:** “installs a set of packages consistent with those specifications”. |
| **nvm download order:** the commands follow nvm’s documented install/use syntax, selecting line 24. Our source inspection shows it downloads through curl, or wget as fallback. Apple’s curl therefore needs 03 first. | [nvm’s usage instructions](https://github.com/nvm-sh/nvm#usage)<br><br>[nvm’s download function](https://github.com/nvm-sh/nvm/blob/master/nvm.sh#L143-L217)<br><br>[Apple workaround evidence → A03](#a03-macos-command-line-certificates) | **Documented syntax:** `nvm install 14.7.0`; `nvm use`.<br><br>**Downloader code:** `NVM_DOWNLOADER='curl'`; `NVM_DOWNLOADER='wget'`. |
| **nvm-windows — source assessment:** the same install/use commands are supported. Its standard HTTPS client uses Go’s normal certificate verification, which delegates to Windows. With certificate verification enabled, this route benefits from the incident block once delivered through 01. This is a source assessment, not an execution test on Windows. | [Install command](https://docs.nvm-windows.com/command/install/) · [Use command](https://docs.nvm-windows.com/command/use/)<br><br>[1.2.2 download code](https://github.com/nvm-windows/nvm/blob/1.2.2/src/web/web.go) · [v2 HTTP client](https://github.com/nvm-windows/common/blob/main/http/client.go)<br><br>[Go’s incident explanation](https://go.dev/blog/emudhra-incident)<br><br>[Windows protection → A01](#a01-operating-system-certificate-protection) | **Commands:** `nvm install 24`; `nvm use <version>`.<br><br>**v2 client:** `transport := gohttp.DefaultTransport.(*gohttp.Transport).Clone()`.<br><br>**Go:** “On Windows and macOS we delegate these checks to the platform APIs”. |
| **Standalone pnpm 10/11:** these releases update their bundled Node. Updating a separately installed Node does not replace that copy. | [pnpm 10.34.8 release notes](https://github.com/pnpm/pnpm/releases/tag/v10.34.8)<br><br>[pnpm 11.28.8 release notes](https://github.com/pnpm/pnpm/releases/tag/v11.28.8) | **10.34.8:** “The standalone pnpm executables now embed Node.js 24.22.1.”<br><br>**11.28.8:** “The standalone `@pnpm/exe` builds now embed Node.js 26.12.1.” |
| **pnpm 12 target:** 12.15.0’s released source selects the fixed certificate verifier. This is our dependency inspection, not a pnpm incident notice or a test of its binaries. | [pnpm 12.15.0’s dependency lockfile](https://github.com/pnpm/pnpm/blob/v12.15.0/Cargo.lock#L7432-L7442)<br><br>[RustSec’s verifier advisory](https://rustsec.org/advisories/RUSTSEC-2026-0382.html) | **Lockfile:** `name = "rustls-webpki"`; `version = "0.103.16"`.<br><br>**Advisory:** “Versions 0.103.16 and 0.104.0-alpha.8 reject paths containing these public keys.” |
| **pnpm update routes:** use repaired npm/Homebrew, or the official installer through protected curl/PowerShell. We add the fixed version to pnpm’s documented commands; the installer’s download dependencies were checked in its source. | [pnpm’s installation instructions](https://pnpm.io/installation) · [Homebrew’s pnpm package](https://formulae.brew.sh/formula/pnpm)<br><br>[Official installer and version selection](https://github.com/pnpm/get.pnpm.io#configuring)<br><br>[Shell installer source](https://github.com/pnpm/get.pnpm.io/blob/main/install.sh) · [PowerShell installer source](https://github.com/pnpm/get.pnpm.io/blob/main/install.ps1) | **npm:** `npm install -g pnpm@latest`.<br><br>**Installer setting:** `PNPM_VERSION`.<br><br>**Shell source:** `curl -fsSL "$1"`.<br><br>**PowerShell source:** `Invoke-WebRequest`. |

**Other Node version managers:** download protection remains unverified here for [fnm](https://github.com/Schniz/fnm) and [Volta](https://github.com/volta-cli/volta). Have the maintainer confirm the installed downloader’s protection before updating. For nvm using another curl or wget, repair that tool’s certificate protection first.

[Back to action 11](#11-nodejs-npmnpx-yarn-pnpm-and-corepack)

### A12. virtualenv: the tool that creates Python environments

**Checked: 27 October 2026.**

| Claim | Sources | Quotes |
| --- | --- | --- |
| **Fix:** Python’s incident guidance requires virtualenv 21.14.12 or later. That release bundles fixed pip 26.2.2 for creating new environments. | [Python’s incident guidance](https://discuss.python.org/t/information-for-python-users-regarding-emudhra-emsign-ca-certificate-security-incident/109576)<br><br>[virtualenv 21.14.12 release notes](https://virtualenv.pypa.io/en/latest/changelog.html#v21-14-12-2026-10-21) | **Incident minimum:** “virtualenv: 21.14.12”.<br><br>**Release:** “the embedded pip wheel from `26.2.1` to `26.2.2`”. |
| **Command:** the pip example adapts virtualenv’s documented installation command: we require the incident-fixed version, omit `--user` to update the active environment and add `--upgrade`. | [virtualenv’s installation instructions](https://virtualenv.pypa.io/en/latest/how-to/install.html)<br><br>[pip’s upgrade option](https://pip.pypa.io/en/stable/cli/pip_install/#cmdoption-U) | **virtualenv:** `python -m pip install --user virtualenv`.<br><br>**pip:** “Upgrade all specified packages to the newest available version.” |
| **Download protection:** repair the installer before it downloads virtualenv. Python’s security guidance supports using corrected pip to obtain further fixes. For Conda, complete 06 first; its initial-download verification limit remains in A06. | [Python security guidance](https://discuss.python.org/t/information-for-python-users-regarding-emudhra-emsign-ca-certificate-security-incident/109576)<br><br>[pip repair evidence → A09](#a09-pip-in-each-python-environment)<br><br>[Conda’s incident instructions](https://conda.org/blog/2026-10-21-emudhra-security-updates/#conda-2694) · [Conda evidence and limit → A06](#a06-conda-miniconda-anaconda-and-miniforge) | **Python security guidance:** “From this secure installation you can bootstrap into a secure installation of certifi, uv, or other dependencies.”<br><br>**Conda’s prerequisite command:** `conda install --name base conda=26.9.4`. |
| **Update the tool’s own copy:** pre-commit, tox and Nox normally use virtualenv installed alongside the tool. This is supported by our source inspection and Nox’s documentation; it is why 12 repeats the update in each tool’s environment. | [pre-commit’s Python environment creation](https://github.com/pre-commit/pre-commit/blob/v4.7.0/pre_commit/languages/python.py)<br><br>[tox’s dependency declaration](https://github.com/tox-dev/tox/blob/main/pyproject.toml)<br><br>[Nox’s environment backends](https://nox.thea.codes/en/stable/config.html#configuring-a-sessions-virtualenv) | **pre-commit:** `[sys.executable, '-mvirtualenv', envdir]`.<br><br>**tox dependency:** `"virtualenv>=21.1"`.<br><br>**Nox:** “The `virtualenv` backend runs from Nox’s interpreter”. |

**Existing environments:** this updates the tool that creates environments; it does not replace Python or pip in environments already created. Those still need [08](#08-python-installations)–[10](#10-python-certificate-packages-in-each-environment). For pre-commit’s saved environments, follow [13](#13-pre-commit-recreate-saved-hook-environments).

**Coverage:** new environments also need a fixed Python from 08. Settings that explicitly select an older pip must be corrected. If tox or Nox is configured to use another environment creator, update that creator instead. If your existing Conda channel cannot supply the required virtualenv version, resolve that with your maintainer rather than replacing its package with pip.

[Back to action 12](#12-virtualenv-the-tool-that-creates-python-environments)

### A13. pre-commit: recreate saved hook environments

**Checked: 27 October 2026.**

| Claim | Sources | Quotes |
| --- | --- | --- |
| `pre-commit clean` followed by `pre-commit install-hooks` replaces cached Python hook environments with ones using corrected Python and pip, provided the updates and version selections in 08, 12 and 14 are complete. | [Documented clean command](https://pre-commit.com/#pre-commit-clean) · [Documented install-hooks command](https://pre-commit.com/#pre-commit-install-hooks)<br><br>[Python hook creation code](https://github.com/pre-commit/pre-commit/blob/v4.7.0/pre_commit/languages/python.py#L214-L228)<br><br>[virtualenv 21.14.12 release notes](https://virtualenv.pypa.io/en/latest/changelog.html#v21-14-12-2026-10-21)<br><br>[Python fix → A08](#a08-python-installations) · [Creator fix → A12](#a12-virtualenv-the-tool-that-creates-python-environments) | **Clean:** “Clean out cached pre-commit files.”<br><br>**Install:** “Install all missing environments for the available hooks.”<br><br>**Python creation:** `[sys.executable, '-mvirtualenv', envdir]`.<br><br>**virtualenv:** “the embedded pip wheel from `26.2.1` to `26.2.2`”. |
| The earlier Git and Python repairs are needed to protect downloads during rebuilding. Our source inspection shows Git fetches hook code and the new Python environment runs pip to install it; certificate verification must remain enabled. | [Git download code](https://github.com/pre-commit/pre-commit/blob/v4.7.0/pre_commit/store.py#L177-L193)<br><br>[Python hook installation code](https://github.com/pre-commit/pre-commit/blob/v4.7.0/pre_commit/languages/python.py#L214-L228)<br><br>[Git protection → A03](#a03-macos-command-line-certificates) / [A05](#a05-git-for-windows) · [Python protection → A08](#a08-python-installations) | **Git:** `git_cmd('fetch', 'origin', '--tags')`.<br><br>**Python installer:** `('python', '-mpip', 'install', '.', *additional_dependencies)`. |

**Version checks:** Check Python inside the recreated hook environments against [08](#08-python-installations), and pip against **26.2.2 or later**. Correct old `language_version` / `default_language_version` settings before rebuilding. These checks concern the hook’s environment, which can differ from your terminal’s default.

**Scope:** The evidence above covers the standard Python hook installation path. Hooks that use an existing system environment need that environment’s earlier updates. If you configure separate pre-commit cache locations, repeat the clean/rebuild for each one; CI caches are handled in [19](#19-github-actions).

[Back to action 13](#13-pre-commit-recreate-saved-hook-environments)

### A14. Project version files and saved environments

**Download safety and ordering.** Regeneration downloads run through the corrected tool. New artifact checksums must come through that protected route, not be copied from an untrusted response.

**What this fixes.** Corrected definitions and saved environments prevent a later rebuild restoring the old program.

**Limits and follow-up.** Check requirements*.txt, .python-version, pyproject.toml, locks, Conda specs, Dockerfiles, .nvmrc/.node-version, Volta/packageManager settings, hook runtime versions and workflows. Review the dependency diff. Historical locks need not be rewritten; label unsuitable online rebuilds and provide a corrected active definition. Old model/source files are not automatically malicious.

**Why recreation can work.** A replacement removes the old environment’s installed copies, provided its creator and saved requirements select fixed versions. An old creator can still supply old starter pip, and unchanged requirements can reinstall old certificate packages. Complete 09 before other package downloads and check 10 after setup. Python treats project virtual environments as disposable; the incident advisory also supports updating existing environments. Keep project files outside the environment and test the replacement before removing the old one.

Sources: [uv Python version settings](https://docs.astral.sh/uv/concepts/python-versions/#project-python-versions), [uv targeted lock upgrades](https://docs.astral.sh/uv/concepts/projects/sync/), [pip-tools targeted upgrades](https://pip-tools.readthedocs.io/en/stable/), [GitHub cache management](https://docs.github.com/en/actions/how-tos/manage-workflow-runs/manage-caches), [Python virtual-environment lifecycle](https://docs.python.org/3/library/venv.html), [Python incident guidance for existing environments](https://discuss.python.org/t/information-for-python-users-regarding-emudhra-emsign-ca-certificate-security-incident/109576).

[Back to action 14](#14-project-version-files-and-saved-environments)

### A15. Code editors and AI coding tools

**Checked: 27 October 2026.**

| Claim | Sources | Quotes |
| --- | --- | --- |
| These tools can bundle Node.js, Electron and other components. Affected bundled copies need updating through the tool's own releases; a separate Node update does not replace them. | [Electron's bundled runtimes](https://www.electronjs.org/docs/latest/)<br><br>[Electron's incident advisory](https://github.com/electron/electron/security/advisories/GHSA-jr5w-w6x2-84p9) | **Bundled components:** “embedding Chromium and Node.js into its binary”.<br><br>**Required fix:** “you must update to a patched version of Electron”. |
| After the applicable earlier steps, official installer downloads through your browser or updates through your usual package manager have certificate protection for this incident. This assessment follows from the linked fixes. | [Browser protection → A02](#a02-browsers) · [Homebrew protection → A04](#a04-homebrew) · [npm/Node protection → A11](#a11-nodejs-npmnpx-yarn-pnpm-and-corepack)<br><br>[Chrome notice](https://blog.google/security/protecting-chrome-and-android-users-from-compromised-emudhra-intermediate-ca-certificates/) · [Homebrew confirmation](https://github.com/Homebrew/homebrew-core/pull/319756#issuecomment-6342608209) · [Node release notice](https://nodejs.org/en/blog/vulnerability/october-2026-security-releases) | **Chrome:** “Chrome users were immediately protected against all certificates sharing those keys”.<br><br>**Homebrew:** “yes this one fixes it with official upstream change”.<br><br>**Node:** “ca-cert (eMudhra Intermediate CA Compromise)”. |

[Back to action 15](#15-code-editors-and-ai-coding-tools)

### A16. GitHub CLI

**Checked: 27 October 2026.**

| Claim | Sources | Quotes |
| --- | --- | --- |
| GitHub CLI **2.102.1** includes Go's block against the compromised certificates, adding protection independent of the OS block. This follows from its recorded compiler update and Go's advisory. | [GitHub CLI release](https://github.com/cli/cli/releases/tag/v2.102.1)<br><br>[Go's incident advisory](https://go.dev/blog/emudhra-incident) | **GitHub CLI:** “bump Go toolchain from 1.27.1 to 1.27.3”.<br><br>**Go:** “automatically reject certificates issued by the compromised eMudhra intermediates”. |
| GitHub documents both update commands. Homebrew after 04 and WinGet after 01 provide protected download routes; the WinGet assessment follows from its use of Windows download services in the inspected source. | [GitHub's macOS instructions](https://github.com/cli/cli/blob/trunk/docs/install_macos.md#homebrew) · [GitHub's Windows instructions](https://github.com/cli/cli/blob/trunk/docs/install_windows.md#winget)<br><br>[WinGet download code](https://github.com/microsoft/winget-cli/blob/master/src/AppInstallerCommonCore/Downloader.cpp)<br><br>[Windows protection → A01](#a01-operating-system-certificate-protection) · [Homebrew protection → A04](#a04-homebrew) | **Homebrew:** `brew upgrade gh`.<br><br>**WinGet:** `winget upgrade --id GitHub.cli --source winget`.<br><br>**Windows download services:** `WinINetDownloadToStream` / `DODownload`. |

[Back to action 16](#16-github-cli)

### A17. Azure CLI

**Checked: 27 October 2026.**

| Claim | Sources | Quotes |
| --- | --- | --- |
| Azure CLI's certificate fix is **merged but not yet released**. Its maintainer expects it in the **3 November 2026** release. | [Azure CLI fix and release date](https://github.com/Azure/azure-cli/pull/34250) | **Maintainer, 27 October:** “The upcoming release is next Tuesday”, linking to “November 2026 (2026-11-03)”. |

[Back to action 17](#17-azure-cli)

### A18. Docker Desktop

**Checked: 27 October 2026.**

| Claim | Sources | Quotes |
| --- | --- | --- |
| Docker Desktop's bundled tools were affected by the compromised certificates. **4.96.3** includes the incident fixes; 4.96.1 and 4.96.2 covered only some components. | [4.96.3 release notes](https://docs.docker.com/desktop/release-notes/#4963)<br><br>[4.96.2's partial-fix notice](https://docs.docker.com/desktop/release-notes/#4962) | **4.96.3:** “Removed trust for certificates issued by the compromised eMudhra intermediate CA compromise.”<br><br>**4.96.2:** “Upcoming patch releases will cover the remaining components.” |
| Downloading Docker's full installer through the protected browser in 02 avoids relying on the old Docker client's certificate checks to obtain the fix. | [Official macOS installer](https://docs.docker.com/desktop/setup/install/mac-install/) · [Official Windows installer](https://docs.docker.com/desktop/setup/install/windows-install/)<br><br>[Browser protection → A02](#a02-browsers) | **macOS:** “Download the installer”.<br><br>**Windows:** “Docker Desktop Installer.exe”. |

[Back to action 18](#18-docker-desktop)

### A19. GitHub Actions

**Checked: 27 October 2026.**

| Claim | Sources | Quotes |
| --- | --- | --- |
| GitHub-hosted jobs use their own environments, so the earlier fixes must be applied through the workflow's version settings and update steps. This applies the earlier incident guidance to GitHub's documented job setup. | [GitHub-hosted job environments](https://docs.github.com/en/actions/concepts/workflows-and-actions/dependency-caching)<br><br>[Saved version settings → A14](#a14-project-version-files-and-saved-environments) | **Hosted jobs:** “Jobs on GitHub-hosted runners start in a clean runner image”. |
| Delete affected caches after correcting the workflow’s versions and update steps, so later runs do not restore old copies. This ordering follows from GitHub’s documented cache reuse; deletion uses its documented web interface. | [Cache reuse](https://docs.github.com/en/actions/concepts/workflows-and-actions/dependency-caching)<br><br>[Deleting cache entries](https://docs.github.com/en/actions/how-tos/manage-workflow-runs/manage-caches#deleting-cache-entries) | **Reuse:** “restores its contents as-is”.<br><br>**Deletion:** “Users with `write` access to a repository can use the GitHub web interface to delete cache entries.” |

[Back to action 19](#19-github-actions)

### A20. Notebook kernels

**Checked: 27 October 2026.**

| Claim | Sources | Quotes |
| --- | --- | --- |
| Update the Python environment used by the notebook, then restart any kernels that were already running during the updates. This applies the incident fixes and download precautions in 08–10 to Jupyter’s documented kernel setup. | [IPython kernels and environments](https://ipython.readthedocs.io/en/stable/install/kernel_install.html#kernels-for-different-environments)<br><br>[Jupyter’s kernel restart guidance](https://jupyter-notebook.readthedocs.io/en/stable/notebook.html#basic-workflow)<br><br>[Python fix → A08](#a08-python-installations) · [pip fix → A09](#a09-pip-in-each-python-environment) · [Certificate packages → A10](#a10-python-certificate-packages-in-each-environment) | **Environments:** “multiple IPython kernels for different virtualenvs or conda environments”.<br><br>**Restart:** “restart the whole computational process, use the Kernel, Restart menu option”. |

[Back to action 20](#20-notebook-kernels)

### A22. Electron apps you develop

**Checked: 27 October 2026.**

| Claim | Sources | Quotes |
| --- | --- | --- |
| Electron 42.11.16, 43.8.1 and 44.9.1 block the compromised certificates in its bundled Chromium and Node.js networking. Rebuilding and distributing the app delivers that fix to users; updating a separate browser or Node installation does not replace the app's bundled copies. | [Electron's incident advisory](https://github.com/electron/electron/security/advisories/GHSA-jr5w-w6x2-84p9)<br><br>[Electron's bundled runtimes](https://www.electronjs.org/docs/latest/) | **Coverage:** “both Chromium networking and Node.js `tls`, `https` and `fetch`”.<br><br>**Fixed versions:** `42.11.16`; `43.8.1`; `44.9.1`.<br><br>**Bundled copies:** “embedding Chromium and Node.js into its binary”. |
| The example adapts Electron's documented npm command to the fixed version. Completing 11 first protects the normal Node-based downloads, with certificate verification enabled; this follows from the documented downloader and Node fix. | [Electron installation instructions](https://www.electronjs.org/docs/latest/tutorial/installation)<br><br>[Electron's downloader](https://github.com/electron/get#how-it-works)<br><br>[Node download protection → A11](#a11-nodejs-npmnpx-yarn-pnpm-and-corepack) | **Install command:** `npm install electron --save-dev`.<br><br>**Downloader:** “uses the built-in Fetch API as the downloader”. |

[Back to action 22](#22-electron-apps-you-develop)

### A23. Python publishing and signing tools

**Checked: 27 October 2026.**

| Claim | Sources | Quotes |
| --- | --- | --- |
| Python's incident guidance recommends **Sigstore 4.5.1 or later**. The command adds an upgrade and that minimum to Sigstore's documented pip installation command. Completing 08–10 first protects the download in that environment. | [Python's incident guidance](https://discuss.python.org/t/information-for-python-users-regarding-emudhra-emsign-ca-certificate-security-incident/109576)<br><br>[Sigstore's installation command](https://pypi.org/project/sigstore/4.5.1/#installation)<br><br>[Python protection → A08](#a08-python-installations) · [pip protection → A09](#a09-pip-in-each-python-environment) | **Recommended version:** “sigstore 4.5.1”.<br><br>**Install command:** `python -m pip install sigstore`.<br><br>**Safe update route:** “From this secure installation you can bootstrap into a secure installation of certifi, uv, or other dependencies.” |
| PyPA's publishing Action **1.14.2 and earlier are affected; 1.14.3 is fixed**. It replaces the publishing container's certifi and Sigstore. Our inspection shows GitHub Actions fetches that container, so its download route needs the earlier repairs described in 19. | [PyPA's incident advisory](https://github.com/pypa/gh-action-pypi-publish/security/advisories/GHSA-5xjx-3w67-w9x9)<br><br>[Fixed dependency versions](https://github.com/pypa/gh-action-pypi-publish/blob/v1.14.3/requirements/runtime.txt)<br><br>[Container download code](https://github.com/pypa/gh-action-pypi-publish/blob/v1.14.3/create-docker-action.py#L14-L18) · [Workflow protection → A19](#a19-github-actions) | **Advisory:** “Affected versions” `<= 1.14.2`; “Patched versions” `>= 1.14.3`.<br><br>**Dependencies:** `certifi==2026.10.19`; `sigstore==4.5.1`.<br><br>**Container source:** `docker://ghcr.io/{repo}:{docker_ref}`. |

[Back to action 23](#23-python-publishing-and-signing-tools)

### A24. Hugging Face

**Checked: 27 October 2026.**

`hf_xet` is Hugging Face’s helper for transferring model and dataset files. Recent `huggingface_hub` installations include it automatically, so you may use it through Transformers or Datasets without installing it separately. It contains Rust networking code, which the earlier Python and certifi updates do not replace. [Hugging Face’s documentation](https://huggingface.co/docs/hub/xet/using-xet-storage)

| Claim | Sources | Quotes |
| --- | --- | --- |
| Hugging Face has merged dependency updates for `hf_xet` that address the compromised certificates. | [Hugging Face’s fix](https://github.com/huggingface/xet-core/pull/1038)<br><br>[Certificate-checking advisory](https://rustsec.org/advisories/RUSTSEC-2026-0382.html) | **Fix:** “rustls-webpki 0.103.15 -> 0.103.16 (RUSTSEC-2026-0382)”; “webpki-root-certs 1.0.6 -> 1.0.10 (RUSTSEC-2026-0383)”.<br><br>**Advisory:** “Versions 0.103.16 and 0.104.0-alpha.8 reject paths containing these public keys.” |
| A fixed package release is still pending. Our release check found `hf_xet` 1.7.0, published on 6 October, before the fix merged on 23 October. | [Current PyPI release](https://pypi.org/project/hf-xet/)<br><br>[Merged fix](https://github.com/huggingface/xet-core/pull/1038) | **PyPI:** “Release files for hf-xet 1.7.0”; “Released: Oct 6, 2026”.<br><br>**Fix status:** “Merged”; “Oct 23, 2026”. |

The earlier Python and certificate-package updates still apply. A pending release does not establish that the installed `hf_xet` is protected.

[Back to action 24](#24-hugging-face)

### A25. uv tools

**Checked: 27 October 2026.** These steps apply Astral’s incident advice using its documented tool and cache commands. Astral’s incident notice does not itself prescribe this sequence or the alternative below.

| Claim | Sources | Quotes |
| --- | --- | --- |
| Updating uv does not update the packages in its separate tool environments. | [Astral’s incident advisory](https://github.com/astral-sh/uv/security/advisories/GHSA-hq85-5cpj-mr62)<br><br>[Tool environments](https://docs.astral.sh/uv/concepts/tools/#the-uv-tool-interface) | **Incident:** “You MUST evaluate your other packages.”<br><br>**Tools:** “isolated virtual environment”. |
| The uv update in 07 fixes its certificate checks before it downloads tool updates. | [Astral’s incident advisory](https://github.com/astral-sh/uv/security/advisories/GHSA-hq85-5cpj-mr62) · [Current target and update route → A07](#a07-uv) | “uv 0.13.4 removes this root CA from its vendored CA bundle”. |
| `uv tool upgrade --all` updates all installed uv tools and their dependencies, within saved version requirements. | [Upgrade all tools](https://docs.astral.sh/uv/guides/tools/#upgrading-tools)<br><br>[Dependency upgrades and version constraints](https://docs.astral.sh/uv/concepts/tools/#upgrading-tools) | **Command:** `uv tool upgrade --all`.<br><br>**Packages:** “all packages in a tool environment”.<br><br>**Limits:** “respect the version constraints”. |
| `uv cache clean` deletes cached tool environments; uv recreates them when next needed. | [Cache cleaning](https://docs.astral.sh/uv/concepts/cache/#clearing-the-cache)<br><br>[Temporary and installed tool environments](https://docs.astral.sh/uv/concepts/tools/#tool-environments) | **Cleanup:** “removes all cache entries from the cache directory”.<br><br>**Replacement environments:** “created automatically”. |
| The alternative’s `--with` requirements select fixed dependency versions for each run, without clearing the cache. | [Extra dependency requirements](https://docs.astral.sh/uv/concepts/tools/#including-additional-dependencies)<br><br>[Installed-tool selection](https://docs.astral.sh/uv/concepts/tools/#tool-versions) · [A10](#a10-python-certificate-packages-in-each-environment), [A12](#a12-virtualenv-the-tool-that-creates-python-environments) | **Syntax:** `uvx --with <extra-package> <tool>`.<br><br>**Isolation:** “ignore the installed version”. |

**Cache effects:** This clears the whole configured uv cache, including downloaded packages, build results and temporary tool environments. Later runs may need internet access and extra time to download or build packages again. [Cache documentation](https://docs.astral.sh/uv/concepts/cache/#clearing-the-cache).

**Version requirements still apply:** Neither approach overrides requirements that force affected versions. Correct those before rebuilding; project settings are covered in [14](#14-project-version-files-and-saved-environments), and installed-tool checks are below.

#### Alternative without clearing the uv cache

Complete 07 and 08 first, as for the main instructions.

1. **Installed tools:** run `uv tool upgrade --all` once, if you have not already done so.
2. **Tools run with `uvx` or `uv tool run`:** if the command reuses an installed tool you have already updated, no further action is needed for that copy. For a cached tool that uses the packages below, add the matching options immediately after `uvx` or `uv tool run`:

| Packages the tool uses | Options to add |
| --- | --- |
| certifi | `--isolated --with "certifi>=2026.10.19"` |
| virtualenv | `--isolated --with "virtualenv>=21.14.12"` |
| Both | `--isolated --with "certifi>=2026.10.19" --with "virtualenv>=21.14.12"` |

Keep the tool name and its arguments after these options. **Use the options each time you run that tool.** They select suitable dependencies for that invocation without clearing the rest of your cache. Do not add packages the tool does not use. If an old version requirement causes a conflict, resolve it before running the tool.

#### Check installed uv tools

`uv tool upgrade --all` respects saved version requirements. Where installed, certifi must reach **2026.10.19 or later** and virtualenv **21.14.12 or later**.

To inspect a tool’s packages, use `uv tool list --show-paths` to locate its environment, then run:

```text
uv pip show --python "TOOL_PYTHON" certifi virtualenv
```

Replace `TOOL_PYTHON` with the Python executable inside that environment. This is a [read-only package check](https://docs.astral.sh/uv/reference/cli/#uv-pip-show). If a version requirement prevents the fix, follow [Astral’s reinstall guidance](https://docs.astral.sh/uv/concepts/tools/#upgrading-tools) to change it. Do not modify uv tool environments with pip.

[Back to action 25](#25-uv-tools)

### Connection tests

**Optional browser check:** first open a normal HTTPS site. Then open both test pages; certificate warnings are expected. **Do not continue past them.**

- [emSign route](https://dv-g3b-emudhra.we-have-a-problem.com/)
- [AAA cross-signed route](https://dv-g3b-aaa-emudhra.we-have-a-problem.com/)

If a test loads normally, that browser accepted the tested certificate. A timeout or blocked network connection is inconclusive. Two tests do not cover every affected chain; maintainers should use the full relevant set from the [incident researcher’s test material](https://emudhra.we-have-a-problem.com/). This is an independent researcher’s site, not eMudhra’s official website.

**Application checks, for maintainers:** run the following only with the library already used by the application, in its actual environment. Do not install a different library just to get a passing result. Custom SSL/client settings must be reproduced in the test.

Requests:

```bash
python -c "import requests; print(requests.get('https://pypi.org/simple/pip/', timeout=15).status_code)"
python -c "import requests; requests.get('https://dv-g3b-emudhra.we-have-a-problem.com/', timeout=15)"
python -c "import requests; requests.get('https://dv-g3b-aaa-emudhra.we-have-a-problem.com/', timeout=15)"
```

HTTPX:

```bash
python -c "import httpx; print(httpx.get('https://pypi.org/simple/pip/', timeout=15).status_code)"
python -c "import httpx; httpx.get('https://dv-g3b-emudhra.we-have-a-problem.com/', timeout=15)"
python -c "import httpx; httpx.get('https://dv-g3b-aaa-emudhra.we-have-a-problem.com/', timeout=15)"
```

Apple’s built-in curl specifically:

```bash
/usr/bin/curl --head --max-time 15 https://pypi.org/simple/pip/
/usr/bin/curl --head --max-time 15 https://dv-g3b-emudhra.we-have-a-problem.com/
/usr/bin/curl --head --max-time 15 https://dv-g3b-aaa-emudhra.we-have-a-problem.com/
```

**Read the result:** normal HTTPS should connect; affected-certificate requests should fail specifically during certificate verification. Python may print a traceback containing that error. Any HTTP response from a test site, including 403/404, means the tested connection accepted the certificate. DNS errors, firewalls and timeouts do not prove rejection. A corporate proxy replacing certificates changes what the test measures. Never send real credentials, API keys, prompts or customer data to test sites.

For complete offline bundle checks, follow [Conda’s published OpenSSL instructions](https://conda.org/blog/2026-10-21-emudhra-security-updates/#check-the-package-contents) for every relevant test chain and explicit used bundle. Use an existing suitable OpenSSL rather than adding an extra audit environment merely for this guide. Vendor evidence that all affected keys are blocked matters where another trusted root could authorise the same key.

### Terms and incident background

| Term | Meaning |
| --- | --- |
| CA / certificate authority | An organisation trusted to issue certificates used to identify websites. |
| Bundle / trust store | A trusted-certificate list. The computer and an app can use different copies. |
| Runtime / interpreter | Software that runs code, such as Python or Node. Apps can include another copy. |
| Environment | A separate Python/package installation for a project or tool. |
| Version pin / lockfile | A setting/file selecting versions; it can restore an old version at the next rebuild. |
| Cache | Saved downloads or environments that a tool reuses. |
| Backport | The same fix applied to an older-numbered release, confirmed by its supplier. |
| CLI | A program used by typing commands in a terminal. |

The compromised signing keys belonged to **intermediate CAs**, not the root keys themselves. An affected client can accept a forged website certificate if an attacker also gets the connection to reach their server, for example through network/routing/DNS interference. Merely having an affected program installed does not execute an attack. A fix can block the affected keys/certificates or remove roots authorising them. An old root in a file alone does not establish vulnerability when the live verifier separately blocks the keys; revocation alone does not protect software that never enforces it.

The website certificate and the update file’s vendor signature are **different checks**. A checksum also helps only when its expected value came from a trusted source; an attacker who can replace both file and checksum on the same intercepted connection defeats that comparison. Keep normal publisher, signature and artifact-integrity checks enabled.

**Unconfirmed fixes stay unconfirmed.** For self-contained pnpm 12, the listed unconfirmed editor/AI/CLI bundles, hf_xet, Ollama and LM Studio, obtain the component’s own vendor/build evidence or sufficiently complete client checks. The checked Microsoft sources do not name a minimum incident-specific Edge version either. Being listed here does not prove vulnerability; a vendor’s silence or “latest” status does not prove protection. If an affected program is confirmed and has no protected update route, stop its affected network operations until repaired.

This guide covers Python/JavaScript/LLM development tools across macOS, Windows and the separate environments they use. It is not a live inventory of every team machine. Additional downloaders and included runtimes should be assessed in the same way.
