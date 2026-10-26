# eMudhra certificate incident: developer update guide

**Guidance checked: 26 October 2026.** For macOS and Windows, including Python/JavaScript projects, LLM tools, remote systems and containers.

## Part 1 — What to do

> [!IMPORTANT]
> You will not have all these tools. Only follow items for software you already use. Skip the rest. Do not install a program just because it appears in this guide.

Start with your operating system and browser. Then find your tools in the list below and work through those items. Project/CI/server items are for whoever maintains those systems. If you do not recognise a tool or know how it was installed, ask the person supporting that setup rather than trying every command.

### Order of work

Each row is a separate item. Its matching **A-number in Part 2** explains the safety of the update method, its place in the order, and what it fixes.

| Step | Tool or task | Only applies if… |
| --- | --- | --- |
| [01](#01-operating-system-updates) | Operating-system updates | Everyone: follow the row for your computer. |
| [02](#02-browsers) | Browsers | Everyone: check the browser you use. Do not install all of these browsers. |
| [03](#03-macos-command-line-certificates) | macOS command-line certificates | macOS users who run curl, Git, Python or similar tools from Terminal. |
| [04](#04-homebrew) | Homebrew | You already use Homebrew on macOS. |
| [05](#05-git-for-windows) | Git for Windows | Windows users with Git installed, including portable copies. |
| [06](#06-conda-miniconda-anaconda-and-miniforge) | Conda, Miniconda, Anaconda and Miniforge | You use any of these Conda distributions. |
| [07](#07-mamba-and-condas-libmamba-solver) | Mamba and Conda’s libmamba solver | You use Mamba or Conda’s libmamba solver. Otherwise skip. |
| [08](#08-micromamba) | Micromamba | You use the separate Micromamba program. |
| [09](#09-uv-and-the-python-copies-it-manages) | uv and the Python copies it manages | You use uv, or still use a Python downloaded by uv. |
| [10](#10-python-installations) | Python installations | You run Python, including through Conda, uv, Homebrew or python.org. |
| [11](#11-pip-in-each-python-environment) | pip in each Python environment | You install packages with pip. Repeat in each environment you use. |
| [12](#12-python-certificate-packages-in-each-environment) | Python certificate packages in each environment | An environment uses certifi or Conda ca-certificates for downloads/API calls. |
| [13](#13-nodejs-npmnpx-yarn-pnpm-and-corepack) | Node.js, npm/npx, Yarn, pnpm and Corepack | You use Node, npm/npx, Yarn, pnpm or Corepack, including project-selected copies. |
| [14](#14-bun) | Bun | You use Bun, or distribute apps built with it. |
| [15](#15-deno) | Deno | You use Deno. |
| [16](#16-pipenv) | Pipenv | You use Pipenv. |
| [17](#17-pixi) | Pixi | You use Pixi. |
| [18](#18-python-tools-installed-in-separate-environments) | Python tools installed in separate environments | You use pipx, uv tools/uvx, Poetry, PDM, Hatch, pip-tools, tox or nox. |
| [19](#19-project-version-files-and-saved-environments) | Project version files and saved environments | You maintain project requirements, lockfiles, runtime settings or CI caches. |
| [20](#20-virtualenv-the-tool-that-creates-python-environments) | virtualenv: the tool that creates Python environments | You use virtualenv directly or through pre-commit/tox/nox. |
| [21](#21-pre-commit-recreate-saved-hook-environments) | pre-commit: recreate saved hook environments | You use pre-commit in a project or CI. |
| [22](#22-code-editors-and-ai-coding-tools) | Code editors and AI coding tools | You use one of the listed apps. Update only yours. |
| [23](#23-development-and-cloud-command-line-tools) | Development and cloud command-line tools | You use gh, Git LFS, Azure CLI, AWS CLI, gcloud, or separately installed curl/OpenSSL tools. |
| [24](#24-docker-desktop-and-container-images) | Docker Desktop and container images | You use Docker or development containers. |
| [25](#25-wsl-remote-machines-azure-ml-and-ci) | WSL, remote machines, Azure ML and CI | Your code runs in these places, or you maintain their environments. |
| [26](#26-electron-apps-your-team-develops) | Electron apps your team develops | You build and distribute an Electron app; otherwise skip. |
| [27](#27-python-publishing-and-signing-tools) | Python publishing and signing tools | You publish packages with Twine, Sigstore or PyPA’s publishing Action. |
| [28](#28-python-apps-that-make-https-connections) | Python apps that make HTTPS connections | Your programs use Requests, HTTPX, SDKs or other HTTP libraries. |
| [29](#29-hugging-face-and-extra-model-downloaders) | Hugging Face and extra model downloaders | You use huggingface_hub, hf_xet, Transformers or Datasets downloads. |
| [30](#30-llm-api-libraries-agent-frameworks-and-mcp-servers) | LLM API libraries, agent frameworks and MCP servers | You use provider SDKs, LangChain, LlamaIndex, LiteLLM or MCP services. |
| [31](#31-ollama-lm-studio-and-other-local-llm-apps) | Ollama, LM Studio and other local LLM apps | You use one of these local model apps. |
| [32](#32-inference-training-evaluation-and-notebooks) | Inference, training, evaluation and notebooks | You run vLLM, SGLang, Transformers, llama.cpp, notebooks or cloud/GPU jobs. |
| [33](#33-running-services-workers-and-scheduled-jobs) | Running services, workers and scheduled jobs | You maintain deployed apps, background workers, agents or scheduled jobs. |

### Before running commands

- **Choose your existing installation method.** For example, update a Conda-installed Node with Conda. Installing another Node with Homebrew can leave the old copy in use.
- **Read the “Before” line.** Some methods need another item first. If a download route is still unconfirmed, pause that item for the maintainer; do not bypass certificate or signature errors.
- **Replace example names.** `myenv`, `APP`, `NAME`, filenames and image tags mean your actual environment, tool or file. Activate the intended Python environment before its Python/pip commands.
- **Use the right terminal.** Python/Conda/Node commands work in macOS Terminal or your normal Windows PowerShell/Conda prompt. macOS-only and Linux-only steps are labelled. Choose one matching installation method, not every alternative. Run separate command lines one at a time.
- **Keep the same supported release line.** For example, use the fixed Node 24.x version for Node 24. **≥ means “at least”.** A supplier-confirmed backport is also acceptable. Do not downgrade an already fixed newer version.

The OS updater in 01 can be used before the developer-tool certificate repairs: it verifies vendor signatures on update files. For later official downloads, use the browser protection checked in 02. A normal update command is not automatically proof that an old downloader can safely obtain its own fix; any remaining condition is stated in its item/A-entry.

### 01. Operating-system updates

**Applies to:** Everyone: follow the row for your computer.

**Before:** You can start here. Use the updater built into Settings.

| Your computer | Do this |
| --- | --- |
| macOS | **System Settings → General → Software Update**. Install available security updates and restart if asked. Keep automatic system-data/security updates enabled. |
| Windows | **Settings → Windows Update → Check for Windows updates**. Install available security updates and restart if asked. Windows 10 puts this under **Update & Security**. |

On a work-managed computer, use your organisation’s normal update process. IT should also confirm that automatic certificate-list updates are reaching it.

[Why this update method is safe, and what it covers → A01](#a01-operating-system-updates)

### 02. Browsers

**Applies to:** Everyone: check the browser you use. Do not install all of these browsers.

**Before:** Complete 01. Keep normal browser/security updates enabled.

| Browser | Do this |
| --- | --- |
| Chrome | No special version upgrade is needed if its automatic certificate-blocking update has arrived. Keep automatic updates on. |
| Firefox desktop | No special version upgrade is needed if its automatic certificate-blocking updates are working. If disabled/unavailable, use a fixed supported line: **158.0.1**, **ESR 153.5.1** or **ESR 115.43.1**, or a later fixed release on that line. |
| Safari | No separate incident-specific update is needed when Apple’s automatic certificate blocks are current. |
| Microsoft Edge — Windows/macOS | Open **⋯ → Help and feedback → About Microsoft Edge**, or enter `edge://settings/help`. Allow updates, select **Restart** if asked, and keep automatic updates on. Also check the affected-certificate test pages below. |
| Other browser | Follow its vendor’s update/security guidance. Do not assume Chrome’s update also updated another browser. |

**Check:** use the [browser tests](#connection-tests). Do not continue past certificate warnings. If protection or automatic updates cannot be confirmed, ask the maintainer/IT before using that browser to download the first fixes.

[Why this update method is safe, and what it covers → A02](#a02-browsers)

### 03. macOS command-line certificates

**Applies to:** macOS users who run curl, Git, Python or similar tools from Terminal.

**Before:** Complete 01. These commands edit a system file and may ask for your Mac login password.

If both roots were already removed, skip to the check. Otherwise follow Apple’s commands in order.

**1. Keep a backup:**

```bash
[ -e /etc/ssl/cert.pem.orig ] || sudo cp -p /etc/ssl/cert.pem /etc/ssl/cert.pem.orig
```

**2. Remove the emSign G3 root:**

```bash
t=$(mktemp) && awk -v s='=== /C=IN/OU=emSign PKI/O=eMudhra Technologies Limited/CN=emSign ECC Root CA - G3' '/^(===|###) /{skip=0} $0==s{skip=1} !skip' /etc/ssl/cert.pem > "$t" && [ -s "$t" ] && sudo install -m 0644 -o root -g wheel "$t" /etc/ssl/cert.pem; rm -f "$t"
```

**3. Remove AAA Certificate Services to cover the second trust route:**

```bash
t=$(mktemp) && awk -v s='=== /C=GB/ST=Greater Manchester/L=Salford/O=Comodo CA Limited/CN=AAA Certificate Services' '/^(===|###) /{skip=0} $0==s{skip=1} !skip' /etc/ssl/cert.pem > "$t" && [ -s "$t" ] && sudo install -m 0644 -o root -g wheel "$t" /etc/ssl/cert.pem; rm -f "$t"
```

**4. Check:**

```bash
openssl crl2pkcs7 -nocrl -certfile /etc/ssl/cert.pem | openssl pkcs7 -print_certs -noout | grep -E 'emSign ECC Root CA - G3|AAA Certificate Services'
```

**Expected:** no output. A certificate-file parsing error is a problem: follow Apple’s backup-restoration instructions. **Recheck after macOS updates until Apple’s permanent fix is installed.** Removing AAA can also stop some unrelated legitimate sites working in these tools.

[Why this update method is safe, and what it covers → A03](#a03-macos-command-line-certificates)

### 04. Homebrew

**Applies to:** You already use Homebrew on macOS.

**Before:** Complete 03 if Homebrew uses Apple’s command-line downloader. A custom downloader needs its own check.

```bash
brew update
brew list --versions ca-certificates
```

If the second command prints an installed version older than `ca-certificates 2026-10-20`, run:

```bash
brew upgrade ca-certificates
```

If no version is printed, skip that command. Update other installed Homebrew packages only where their sections below call for it.

[Why this update method is safe, and what it covers → A04](#a04-homebrew)

### 05. Git for Windows

**Applies to:** Windows users with Git installed, including portable copies.

**Before:** Use the protected browser from 02 for the official installer.

1. Run the [official Git for Windows installer](https://git-scm.com/install/windows) for **2.56.0(3) or a later fixed release**.
2. Update any portable copy separately from the official portable download.
3. In the terminal/editor where you use Git, check:

```bash
git --version
```

**Minimum output:** `2.56.0.windows.3`.

[Why this update method is safe, and what it covers → A05](#a05-git-for-windows)

### 06. Conda, Miniconda, Anaconda and Miniforge

**Applies to:** You use any of these Conda distributions.

**Before:** If the old Conda downloader’s protection is unconfirmed, have the maintainer establish the verified update route in A06 first. OS updates alone do not repair its private bundles.

```bash
conda install --name base conda=26.9.4
conda --version
```

**Target:** Conda **26.9.4 or a later corrected release**. Do not downgrade an already fixed newer version. Repeat for separate Conda installations. For certificate packages in other environments, use 12.

[Why this update method is safe, and what it covers → A06](#a06-conda-miniconda-anaconda-and-miniforge)

### 07. Mamba and Conda’s libmamba solver

**Applies to:** You use Mamba or Conda’s libmamba solver. Otherwise skip.

**Before:** As with 06, first confirm the downloader uses corrected certificates or use the verified offline repair.

**Mamba installed from conda-forge:**

```bash
mamba install --name base --override-channels --channel conda-forge mamba libmamba "ca-certificates>=2026.10.19"
```

Use the environment containing Mamba. Include `libmambapy` if already installed; for Mamba 1.x also include `"certifi>=2026.10.19"`.

**Conda’s libmamba solver:** choose the existing channel.

```bash
# conda-forge
conda install --name base --override-channels --channel conda-forge conda-libmamba-solver libmambapy libmamba "certifi>=2026.10.19" "ca-certificates>=2026.10.19"
```

```bash
# Anaconda main / defaults
conda install --name base --override-channels --channel defaults conda-libmamba-solver libmambapy libmamba "certifi>=2026.10.19" "ca-certificates>=2026.10.20"
```

**Check:** package versions, build numbers and channels with `conda list`. Do not switch channels just to match an example.

[Why this update method is safe, and what it covers → A07](#a07-mamba-and-condas-libmamba-solver)

### 08. Micromamba

**Applies to:** You use the separate Micromamba program.

**Before:** Use a confirmed protected download route; do not assume updating Conda repaired Micromamba.

| How installed | Update |
| --- | --- |
| Direct installation | `micromamba self-update` |
| Homebrew | `brew upgrade micromamba` after 04 |

**Check:** `micromamba --version`. Also update environment certificates using 12. **Fixed-version confirmation remains required**; “latest” alone is not a verified incident fix.

[Why this update method is safe, and what it covers → A08](#a08-micromamba)

### 09. uv and the Python copies it manages

**Applies to:** You use uv, or still use a Python downloaded by uv.

**Before:** For pip-installed uv, complete 11 first. For Homebrew, complete 04. An old standalone self-updater needs a verified download route.

| How uv was installed | Update — choose one |
| --- | --- |
| Direct installation | `uv self update` |
| Homebrew | `brew upgrade uv` |
| pip | `python -m pip install --upgrade "uv>=0.13.5"` |

Then refresh each managed Python release line you use; for example:
```bash
uv python upgrade 3.12
uv --version
uv python list --only-installed
```

**Target:** uv **≥0.13.5**. Check pip in existing environments using 11.

[Why this update method is safe, and what it covers → A09](#a09-uv-and-the-python-copies-it-manages)

### 10. Python installations

**Applies to:** You run Python, including through Conda, uv, Homebrew or python.org.

**Before:** Update its supplier first: Conda 06, Homebrew 04, or uv 09. Use 02 for official installers.

| Python line | First fixed upstream version |
| --- | --- |
| 3.11 | 3.11.18 |
| 3.12 | 3.12.16 |
| 3.13 | 3.13.18 |
| 3.14 | 3.14.10 |
| 3.15 | 3.15.2 |

| How installed | Update |
| --- | --- |
| Conda, existing Python 3.12 example | `conda install --name myenv "python>=3.12.16,<3.13"` |
| uv | Follow 09 for each managed release line. |
| python.org, 3.14/3.15 | Run the current [python.org installer](https://www.python.org/downloads/) for your existing release line. |
| Homebrew/other supplier | Update the same versioned package; require the fix or a documented backport. |

**Check which Python you are using:**
```bash
python -c "import sys; print(sys.executable); print(sys.version)"
```

The Python project’s **3.11–3.13 releases are source-only**; use your supplier’s corrected package rather than looking for a new python.org installer. Do not replace Apple’s Python or change release lines just to copy an example.

[Why this update method is safe, and what it covers → A10](#a10-python-installations)

### 11. pip in each Python environment

**Applies to:** You install packages with pip. Repeat in each environment you use.

**Before:** Activate the intended environment. Do not update only base and assume other environments changed.

**If pip is already ≥26.2.2, check its configuration; do not reinstall an older version.** Otherwise choose one route:

**A. Python already has the confirmed key-block fix from 10:**

```bash
python -m pip install --upgrade pip
```

**B. That Python fix is not confirmed: use the advisory’s checksum-checked first update:**

```bash
python -m pip install "pip @ https://files.pythonhosted.org/packages/3f/06/3e4420302363fbb71be3c7f35ce042f54bfca330e3e66b3a2e3c4cca579f/pip-26.2.2-py3-none-any.whl#sha256=fd7f4dfa8190e014013b9f08f3e973f4e4c26ffd179a204895533a7254813a70"
```

**Check:**

```bash
python -m pip --version
```

**Target:** pip **≥26.2.2**. This requires Python **≥3.10**. Use a maintained development Python for older Apple/Xcode pip; do not use `sudo pip` or edit Apple’s Python.

[Why this update method is safe, and what it covers → A11](#a11-pip-in-each-python-environment)

### 12. Python certificate packages in each environment

**Applies to:** An environment uses certifi or Conda ca-certificates for downloads/API calls.

**Before:** For pip, complete 11. For Conda, complete 06. Choose the installer that owns the existing package.

**pip-installed certifi:**

```bash
python -m pip install --upgrade "certifi>=2026.10.19"
python -m pip show certifi
```

**Conda environment: choose its existing channel and replace `myenv`:**

```bash
# conda-forge
conda install --name myenv --override-channels --channel conda-forge "certifi>=2026.10.19" "ca-certificates>=2026.10.19"
```

```bash
# Anaconda main / defaults
conda install --name myenv --override-channels --channel defaults "certifi>=2026.10.19" "ca-certificates>=2026.10.20"
```

Repeat for each used environment. Do not install unused certifi, replace a Conda-owned copy with pip, or switch channels for this checklist.

[Why this update method is safe, and what it covers → A12](#a12-python-certificate-packages-in-each-environment)

### 13. Node.js, npm/npx, Yarn, pnpm and Corepack

**Applies to:** You use Node, npm/npx, Yarn, pnpm or Corepack, including project-selected copies.

**Before:** Use 04 for Homebrew, 06 for Conda, or a verified official download via 02. Other version-manager downloaders must be confirmed protected.

| Existing Node line | Minimum fixed |
| --- | --- |
| 22 | 22.23.4 |
| 24 | 24.22.1 |
| 26 | 26.12.1 |

**Existing Node 24 examples — choose only the method you use:**

| Installed with | Update |
| --- | --- |
| Official installer | Run the current [official Node 24 installer](https://nodejs.org/en/download). |
| Conda | `conda install --name myenv "nodejs>=24.22.1,<25"` |
| Homebrew node@24 | `brew upgrade node@24` |
| nvm, macOS/Linux/WSL | `nvm install 24` then `nvm use 24` |
| nvm-windows | `nvm install 24.22.1` then `nvm use 24.22.1`, or the later fixed 24.x equivalent |
| fnm | `fnm install 24` then `fnm use 24` |
| Volta | `volta install node@24`; check project settings too |

**Check in the environment/editor you use:**
```bash
node -p "process.version + ' ' + process.execPath"
npm --version
```

Normal JavaScript npm/npx/Yarn/pnpm/Corepack using that Node inherit its fix. **Self-contained pnpm 12 needs its own confirmation; see A13.** Do not change Node major versions just to copy this table.

[Why this update method is safe, and what it covers → A13](#a13-nodejs-npmnpx-yarn-pnpm-and-corepack)

### 14. Bun

**Applies to:** You use Bun, or distribute apps built with it.

**Before:** Protect the existing updater/installer route first; a Node update does not update Bun.

| Installed with | Update |
| --- | --- |
| Direct installation | `bun upgrade` |
| Homebrew | `brew upgrade bun` after 04 |
| Scoop | `scoop update bun` through its verified update route |

**Check:** `bun --version` is **≥1.4.4**. Rebuild and redistribute apps made with `bun build --compile`.

[Why this update method is safe, and what it covers → A14](#a14-bun)

### 15. Deno

**Applies to:** You use Deno.

**Before:** Protect its existing updater/installer route first; a Node update does not update Deno.

| Installed with | Update |
| --- | --- |
| Direct installation | `deno upgrade` |
| Homebrew | `brew upgrade deno` after 04 |
| WinGet | `winget upgrade DenoLand.Deno` through its verified update route |
| Scoop | `scoop update deno` through its verified update route |

**Check:** `deno --version` is **≥2.9.8**.

[Why this update method is safe, and what it covers → A15](#a15-deno)

### 16. Pipenv

**Applies to:** You use Pipenv.

**Before:** Protect the pip/pipx environment running the update: 11 and, for pipx, 18.

| Installed with | Update |
| --- | --- |
| pipx | `pipx upgrade pipenv` |
| pip | `python -m pip install --upgrade "pipenv>=2026.10.2"` |

**Check:** `pipenv --version` is **≥2026.10.2**. Check the project environment separately using 11–12.

[Why this update method is safe, and what it covers → A16](#a16-pipenv)

### 17. Pixi

**Applies to:** You use Pixi.

**Before:** For Homebrew/Conda, complete 04/06. A direct self-update needs a confirmed safe first-download route.

| Installed with | Update |
| --- | --- |
| Direct installer | `pixi self-update` |
| Homebrew | `brew upgrade pixi` |
| Conda | `conda update --name my-pixi-env --channel conda-forge pixi`, replacing the environment/channel with yours |

**Check:** `pixi --version` is **≥0.82.1**. Update managed environment certificates and active lockfiles where needed.

[Why this update method is safe, and what it covers → A17](#a17-pixi)

### 18. Python tools installed in separate environments

**Applies to:** You use pipx, uv tools/uvx, Poetry, PDM, Hatch, pip-tools, tox or nox.

**Before:** Protect the actual installation backend first: pip 11, uv 09, or the existing package manager.

| Installation | Do this |
| --- | --- |
| pipx | Inspect `pipx list --skip-maintenance`. Repair its shared pip if needed (A18), then `pipx upgrade-shared`. Update the relevant app with `pipx upgrade APP`. |
| Packages inside a pipx app | Inspect `pipx runpip --skip-maintenance APP show pip certifi virtualenv`. Update only used affected packages via that environment’s Python or `pipx runpip`. |
| uv tools | `uv tool list`; update an existing certifi dependency with `uv tool upgrade APP --upgrade-package certifi`. Refresh affected saved uvx environments. |
| Poetry installed directly | `poetry self update`; on Windows follow the original installer’s method because Poetry warns about self commands there. |
| Other pip/pipx-installed tools | Update the relevant tool with the same installer, e.g. `pipx upgrade tox` or `python -m pip install --upgrade tox`. |
| App including its own Python | Update the whole vendor distribution; a project’s pip cannot edit that separate copy safely. |

Replace `APP` with the installed app’s name. **Check its own Python, pip, certifi and virtualenv where used**, not just the managed project.

[Why this update method is safe, and what it covers → A18](#a18-python-tools-installed-in-separate-environments)

### 19. Project version files and saved environments

**Applies to:** You maintain project requirements, lockfiles, runtime settings or CI caches.

**Before:** Fix the installer/runtime that will regenerate the files first.

1. Correct active requirements that would reinstall an affected version.
2. Use the existing project tool to update affected packages, not every dependency:

| Project tool | Existing certifi example |
| --- | --- |
| uv | `uv lock --upgrade-package certifi`, then `uv sync --locked` |
| Poetry | `poetry update certifi` |
| pip-tools | `pip-compile --upgrade-package certifi`, with the project’s normal file arguments; then its normal sync |
| Conda/Pixi | Update the existing environment definition and regenerate it through the project’s normal workflow. |

3. Refresh affected tool/image caches after fixing their source settings. Keep unrelated data and historical experiment records.

[Why this update method is safe, and what it covers → A19](#a19-project-version-files-and-saved-environments)

### 20. virtualenv: the tool that creates Python environments

**Applies to:** You use virtualenv directly or through pre-commit/tox/nox.

**Before:** Protect pip/the package manager in the environment running virtualenv first.

**If installed with pip:**

```bash
python -m pip install --upgrade "virtualenv>=21.14.12"
python -m virtualenv --version
```

For Conda/pipx, use that existing installer. **Target: ≥21.14.12.** Update it where it actually runs, then recreate affected environments using 21.

[Why this update method is safe, and what it covers → A20](#a20-virtualenv-the-tool-that-creates-python-environments)

### 21. pre-commit: recreate saved hook environments

**Applies to:** You use pre-commit in a project or CI.

**Before:** Complete 05/03 as applicable, 20, and 19 for any old runtime settings. Protect Node/Go downloaders too.

**Once, clear the affected saved environments:**

```bash
pre-commit clean
```

**Then in each project with `.pre-commit-config.yaml`:**

```bash
pre-commit install-hooks
```

Check pip, Node and Go versions **inside the recreated hook environments**. Use pip ≥26.2.2, the relevant Node minimum in 13, and Go **1.26.10 / 1.27.3** on those lines or a confirmed backport. **Leave hook versions unchanged.**

[Why this update method is safe, and what it covers → A21](#a21-pre-commit-recreate-saved-hook-environments)

### 22. Code editors and AI coding tools

**Applies to:** You use one of the listed apps. Update only yours.

**Before:** Use its normal updater or original installer. npm/pip/Homebrew routes require the corresponding earlier fix.

| Tool / installation | Update |
| --- | --- |
| VS Code | **Check for Updates** or the [official download](https://code.visualstudio.com/updates/v1_143). Target **1.143.1 or a later fixed release**. |
| Cursor editor / CLI | Normal editor updater/[official installer](https://www.cursor.com/downloads); CLI: `cursor-agent update`. |
| Windsurf | Normal supported updater/official download. Do not run a user-installed Windows copy as Administrator when updating. |
| Codex desktop | Normal app updater/current official desktop installer; fully restart. |
| Codex CLI, npm | `npm install -g @openai/codex@latest`; check `codex --version`. |
| Codex CLI, Homebrew | `brew update` then `brew upgrade --cask codex`. This is the CLI package. |
| Claude Code, direct install | `claude update`, then restart and check `claude --version`. |
| Claude Code, package manager | `brew upgrade claude-code` (keep your existing channel) or `winget upgrade Anthropic.ClaudeCode`. |

**After updating:** check separately downloaded runtimes, extensions/MCP servers and remote copies. **For tools without a confirmed fixed version, leave the certificate check pending; updating the app alone is not proof.** See the per-tool table in A22.

[Why this update method is safe, and what it covers → A22](#a22-code-editors-and-ai-coding-tools)

### 23. Development and cloud command-line tools

**Applies to:** You use gh, Git LFS, Azure CLI, AWS CLI, gcloud, or separately installed curl/OpenSSL tools.

**Before:** Use the original installer/manager after its earlier fix, or a verified official installer via 02.

| Tool | Update | Check / target |
| --- | --- | --- |
| GitHub CLI | Homebrew: `brew upgrade gh`; otherwise current official installation method | `gh --version`: **≥2.102.1** |
| Git LFS | Homebrew: `brew upgrade git-lfs`; also update Git for Windows where used | `git lfs version`; separate fix confirmation required |
| Azure CLI / ML extension | `az upgrade`; ML separately: `az extension update --name ml` | `az version`; `az extension list`; private bundles need checking |
| AWS CLI v2 | Current official signed macOS PKG/Windows MSI; supported installations can use `aws update` | `aws --version`; private bundles need checking |
| Google Cloud CLI | Component-manager install: `gcloud components update`; otherwise use its original OS package manager | `gcloud version`; selected Python/CA list need checking |
| Separately installed curl/OpenSSL | Follow the supplier’s instructions for that existing installation and the certificate list it actually uses. Ask its maintainer/IT if unclear. | Check the actual program, TLS backend and selected CA files; replacing the executable alone may leave an old CA list. |

Do not edit a vendor’s private Python using your project’s pip. **For the unconfirmed rows, normal “latest” status is not a completed certificate check.**

[Why this update method is safe, and what it covers → A23](#a23-development-and-cloud-command-line-tools)

### 24. Docker Desktop and container images

**Applies to:** You use Docker or development containers.

**Before:** Use the normal Desktop updater/verified official installer. Correct image dependency definitions before rebuilding.

1. Update Docker Desktop to **4.96.3 or a later fixed release** with its updater or official [macOS](https://docs.docker.com/desktop/setup/install/mac-install/) / [Windows](https://docs.docker.com/desktop/setup/install/windows-install/) installer.
2. Update affected certificates/runtimes/packages in the Dockerfile/base image and project locks. Change an old pinned image digest to a corrected image.
3. Rebuild through the project workflow; example:

```bash
docker build --pull --no-cache -t TEAM_IMAGE:PATCHED .
```

4. Recreate containers and deploy the new image. Updating Desktop does not repair an old image or running container.

[Why this update method is safe, and what it covers → A24](#a24-docker-desktop-and-container-images)

### 25. WSL, remote machines, Azure ML and CI

**Applies to:** Your code runs in these places, or you maintain their environments.

**Before:** Repeat the earlier applicable download/update checks on that machine or inside that environment.

| Location | Do this |
| --- | --- |
| WSL | List with `wsl --list --verbose`. `wsl --update` updates WSL itself. Enter each used Linux with `wsl --distribution NAME` and update its packages too. |
| Ubuntu | Vendor incident command: `sudo apt update && sudo apt install --only-upgrade ca-certificates 'libnss3*'`. Use a supported release/required extended-security support. |
| Amazon Linux 2/2023 | Vendor command: `sudo yum update ca-certificates`. Apply other runtime/application fixes too. |
| Other Linux / remote SSH | Use that system’s vendor instructions; check its own Python/Node, certificates, downloaders and editor server/extensions. |
| Azure ML | Build/register a corrected environment version, select it in jobs/deployments and confirm a fresh job actually uses it. |
| GitHub Actions | Update affected setup/action releases; check both project runtimes and runtimes included in the actions. |
| Self-hosted runners | Update the runner app **and** its OS/development tools; run fresh jobs on the corrected setup. |
| pre-commit CI cache | Correct its inputs, then remove only the affected cache under **Actions → Caches**, or change its key. Keep hook versions unchanged. |

[Why this update method is safe, and what it covers → A25](#a25-wsl-remote-machines-azure-ml-and-ci)

### 26. Electron apps your team develops

**Applies to:** You build and distribute an Electron app; otherwise skip.

**Before:** Protect the Node/package-manager download route first, and update the project’s existing release line.

| Electron line | First fixed stable release |
| --- | --- |
| 42 | 42.11.16 |
| 43 | 43.8.1 |
| 44 | 44.9.1 |

Existing Electron 43 / npm example:
```bash
npm install --save-dev electron@43.8.1
```

Use a later fixed supported release on your line if appropriate. Update the lockfile, rebuild/sign/distribute the app, and test the delivered build.

[Why this update method is safe, and what it covers → A26](#a26-electron-apps-your-team-develops)

### 27. Python publishing and signing tools

**Applies to:** You publish packages with Twine, Sigstore or PyPA’s publishing Action.

**Before:** Protect the environment doing the publishing using 10–12; CI downloads follow 25.

| Tool | Do this |
| --- | --- |
| Python Sigstore | In its pip-installed environment: `python -m pip install --upgrade "sigstore>=4.5.1"`; otherwise use its original installer. |
| Twine / other publisher | Update its used Python/pip/certifi; no universal fixed Twine version was established. |
| PyPA publishing Action | Use **v1.14.3 or a later fixed release**, or the verified full commit SHA required by team policy. |

```yaml
- uses: pypa/gh-action-pypi-publish@v1.14.3
```

[Why this update method is safe, and what it covers → A27](#a27-python-publishing-and-signing-tools)

### 28. Python apps that make HTTPS connections

**Applies to:** Your programs use Requests, HTTPX, SDKs or other HTTP libraries.

**Before:** Complete 12 for the application’s environment, and check any custom certificate settings.

1. Ensure the **certifi actually used** is **≥2026.10.19**, where that is the selected list.
2. Have the maintainer check custom certificate files/settings.
3. Restart/redeploy, then test the application’s own connection library—not just curl.

[Why this update method is safe, and what it covers → A28](#a28-python-apps-that-make-https-connections)

### 29. Hugging Face and extra model downloaders

**Applies to:** You use huggingface_hub, hf_xet, Transformers or Datasets downloads.

**Before:** Protect Python downloads using 11–12. A separate native downloader needs its own evidence.

```bash
python -m pip show huggingface-hub hf-xet transformers datasets
```

Apply 28 to Python connections. Check the vendor’s fix for extra downloaders such as **hf_xet**, then update through the project’s normal protected workflow if needed. **No fixed/vulnerable hf_xet version was confirmed here: leave that check pending rather than assuming certifi repaired it.**

[Why this update method is safe, and what it covers → A29](#a29-hugging-face-and-extra-model-downloaders)

### 30. LLM API libraries, agent frameworks and MCP servers

**Applies to:** You use provider SDKs, LangChain, LlamaIndex, LiteLLM or MCP services.

**Before:** Protect the actual environment/library making each connection using the earlier relevant steps.

| Used component | Do this |
| --- | --- |
| Python library using certifi | Follow 28 in that environment. |
| JavaScript library/MCP server using normal Node HTTPS | Follow 13 for the Node actually running it. |
| Separate downloader / included runtime | Require its own vendor fix or connection verification. |
| boto3 / Bedrock | Use **botocore ≥1.43.117** with compatible boto3, through the existing project workflow. |
| LiteLLM | Fixed corresponding lines: **1.103.8, 1.104.6, 1.105.4, 1.106.3**, or later vendor-confirmed fixed releases. Update library/proxy/container copies separately. |

Existing pip-installed LiteLLM 1.106 example: `python -m pip install "litellm==1.106.3"`. Use the project lock workflow where applicable; do not downgrade a later fixed release or move every project to this line.

[Why this update method is safe, and what it covers → A30](#a30-llm-api-libraries-agent-frameworks-and-mcp-servers)

### 31. Ollama, LM Studio and other local LLM apps

**Applies to:** You use one of these local model apps.

**Before:** Use the normal vendor updater/verified official distribution, with its download checks intact.

| App | Do this |
| --- | --- |
| Ollama on macOS/Windows | Accept its automatic desktop update and choose **Restart to update**, or use the current official installer. |
| LM Studio | Update the app normally; also check separately downloaded model-running runtimes in its runtime tools. |
| Similar app | Check its own model/runtime downloader and API connections. |

**No incident-fixed minimum for these apps was confirmed here.** Obtain vendor confirmation/test results before marking the relevant downloader fixed.

[Why this update method is safe, and what it covers → A31](#a31-ollama-lm-studio-and-other-local-llm-apps)

### 32. Inference, training, evaluation and notebooks

**Applies to:** You run vLLM, SGLang, Transformers, llama.cpp, notebooks or cloud/GPU jobs.

**Before:** Apply earlier fixes in the environment doing downloads/API calls, not only the laptop submitting the job.

1. Update its used certificates, runtime, API libraries and extra downloaders.
2. Rebuild images/environments and use them in fresh jobs (24–25).
3. Check the notebook kernel’s Python/packages and restart it.
4. Verify its actual network client before new downloads/API traffic.

**Do not automatically upgrade CUDA, GPU drivers, model weights or every ML framework for this incident.**

[Why this update method is safe, and what it covers → A32](#a32-inference-training-evaluation-and-notebooks)

### 33. Running services, workers and scheduled jobs

**Applies to:** You maintain deployed apps, background workers, agents or scheduled jobs.

**Before:** Correct their definitions, packages and images with the relevant earlier steps.

1. Build the updated environment/image using the normal release process.
2. Deploy it and restart affected web services, workers, scheduled jobs, notebook servers and agent/model processes.
3. Confirm the **running** program uses the new version/image.
4. Test its own HTTPS library against normal and affected-certificate sites. Do not send real credentials or data to test sites.

[Why this update method is safe, and what it covers → A33](#a33-running-services-workers-and-scheduled-jobs)

### Final Checklist

- [ ] Relevant tools updated; unused tools marked **not applicable**.
- [ ] The actual program/environment was checked, not a different installed copy.
- [ ] Current project files/caches will not bring the old version back.
- [ ] Running apps/jobs were restarted or redeployed where needed.
- [ ] Relevant connection tests pass; unconfirmed vendor/runtime checks remain **pending confirmation**.

## Part 2 — Why each update method is safe, and what it covers

**A01 corresponds to 01, A02 to 02, and so on.** Each entry separates two questions: whether the first update can be obtained safely, and whether the resulting update fixes the relevant component. “Documented update command” and “confirmed incident fix” are not the same thing.

### A01. Operating-system updates

**Download safety and ordering.** The built-in updater checks the update file’s vendor signature, separately from the HTTPS certificate used to download it. Apple says Software Update verifies Apple’s signature automatically before installation. Microsoft requires updates from its public update service to be Microsoft-signed. Forging a website certificate does not supply the valid vendor signature needed for a substituted OS update. This is why this step can come before repairing developer-tool certificate files.

**What this fixes.** Available OS updates and automatic certificate-list updates protect the parts of the system that use them. Windows normally retrieves trusted and disallowed certificate lists automatically through its CTL Updater, separately from ordinary OS releases. Microsoft’s built-in curl uses Schannel, which normally uses the Windows certificate store and checks revocation. This differs from the separate Apple PEM file in 03. There is no requirement to download a certificate file from an unfamiliar site before running the built-in updater.

**Limits and follow-up.** This is not a guarantee against unrelated problems or a reason to bypass an update/signature error. IT-managed update servers follow organisation policy; confirm automatic certificate-list delivery is current rather than inferring it from the OS version. Schannel describes the built-in curl’s default architecture, not proof that every Windows download client or custom CA setting is protected. Git for Windows, private application bundles and WSL are separate: see 05, the relevant tool items and 25. OS updates do not replace every application’s own certificate list. Apple still describes the separate command-line file fix as forthcoming; see 03.

Sources: [Apple incident guidance](https://support.apple.com/en-us/149555), [Apple background-update settings](https://support.apple.com/en-us/101591), [Microsoft’s Windows Update instructions](https://support.microsoft.com/en-us/windows/deployment/updates-lifecycle/install-windows-updates), [Microsoft’s CTL documentation](https://learn.microsoft.com/en-us/windows-server/identity/ad-cs/certificate-trust), [Apple: automatic update-signature verification](https://support.apple.com/en-us/102130), [Microsoft: required signatures for public Windows Update](https://learn.microsoft.com/en-us/windows/deployment/update/waas-wu-settings#allow-signed-updates-from-an-intranet-microsoft-update-service-location), [curl supplied with Windows uses Schannel](https://curl.se/windows/microsoft.html).

[Back to action 01](#01-operating-system-updates)

### A02. Browsers

**Download safety and ordering.** Chrome and Firefox have authenticated update systems, including checks on signed update files. Safari is maintained through Apple’s signed OS update process. Edge has its own updater and background certificate-blocking components; use its supported updater, not an arbitrary downloaded script. Where a browser’s incident protection has already arrived, it provides the protected HTTPS route for subsequent official downloads.

**What this fixes.** Google and Mozilla document automatic certificate blocks that do not require a new browser version. Apple documents automatic framework protection. Microsoft documents Edge’s separate checker and background revocation-list updates, but the checked sources do not name an incident-specific minimum Edge version.

**Limits and follow-up.** An “up to date” browser version alone is not proof that every background security list arrived. Restricted internet access and managed settings need checking. Edge’s certificate system is separate from the OS; do not infer its status from Chrome or Windows alone. Two test pages are useful checks, not proof for every chain.

Sources: [Apple incident guidance](https://support.apple.com/en-us/149555), [Google’s Chrome protection notice](https://blog.google/security/protecting-chrome-and-android-users-from-compromised-emudhra-intermediate-ca-certificates/), [Mozilla’s Firefox protection notice](https://blog.mozilla.org/security/2026/10/20/protecting-firefox-users-following-the-compromise-of-emudhra-intermediate-certificates/), [Microsoft’s update steps](https://support.microsoft.com/en-us/edge/microsoft-edge-update-settings), [Edge’s own certificate checker](https://learn.microsoft.com/en-us/deployedge/microsoft-edge-security-cert-verification), [background components and certificate-blocking updates](https://learn.microsoft.com/en-us/deployedge/microsoft-edge-policies/ComponentUpdatesEnabled), [Chrome updater authentication](https://chromium.googlesource.com/chromium/src/+/main/docs/updater/functional_spec.md#update-formats), [Firefox update signatures](https://firefox-source-docs.mozilla.org/toolkit/mozapps/update/docs/MarFiles.html).

[Back to action 02](#02-browsers)

### A03. macOS command-line certificates

**Download safety and ordering.** The commands operate on files already on the Mac; they do not download an installer through the affected connection. They are the commands Apple publishes for this situation.

**What this fixes.** Some Apple/Xcode command-line tools use /etc/ssl/cert.pem without applying the framework’s certificate block. Removing both emSign G3 and AAA blocks the relevant direct and cross-signed routes in that file.

**Limits and follow-up.** Apple calls this an immediate optional mitigation while its permanent software update is pending. Removing AAA affects unrelated chains too. A macOS update can replace the file; follow Apple’s recheck/backup instructions until the permanent fix has shipped. Separate pip and app bundles are not repaired by this edit. The final grep returning status 1 means no match and is expected; a parser error is different.

Sources: [Apple’s commands and limitations](https://support.apple.com/en-us/149555).

[Back to action 03](#03-macos-command-line-certificates)

### A04. Homebrew

**Download safety and ordering.** This route relies on the downloader actually used by Homebrew being protected. With the usual Apple curl route, 03 repairs its certificate file before these network commands. Do not infer that an unknown custom curl/private bundle was repaired too.

**What this fixes.** brew update refreshes Homebrew and package information. brew upgrade ca-certificates updates that installed package’s CA list. These are different operations; brew update alone does not update Apple’s PEM file.

**Limits and follow-up.** Do not install an unused CA package or add global certificate overrides to complete this item. Homebrew’s included Ruby and any other private CA files used for network operations still need checking. This section does not certify every Homebrew configuration.

Sources: [Apple’s commands and limitations](https://support.apple.com/en-us/149555), [Homebrew command documentation](https://docs.brew.sh/Manpage), [Homebrew’s CA-bundle incident change](https://github.com/Homebrew/homebrew-core/blob/d02b8762f4a29859c47a635a0d2683223fea9812/Formula/c/ca-certificates.rb).

[Back to action 04](#04-homebrew)

### A05. Git for Windows

**Download safety and ordering.** Obtain the existing distribution’s installer through a browser with the incident block, keeping normal publisher/signature checks enabled. Do not bootstrap through an unconfirmed old Git downloader.

**What this fixes.** The Git for Windows incident release updates the bundled components. Updating the full supported distribution replaces those copies together.

**Limits and follow-up.** Portable copies, Git included in editors and custom CA files can remain separate. Verify the executable actually used, not just one Git found in a different terminal.

Sources: [official Git for Windows installer](https://git-scm.com/install/windows), [Git for Windows incident release](https://github.com/git-for-windows/git/releases/tag/v2.56.0.windows.3).

[Back to action 05](#05-git-for-windows)

### A06. Conda, Miniconda, Anaconda and Miniforge

**Download safety and ordering.** The vendor publishes this update command, but does not establish an independent first-download verification for every old Conda/libmamba setup. The command is a fix, not proof that an affected downloader can securely fetch itself. Before using it, confirm the selected downloader/store is corrected, or use verified offline CA packages/current corrected distribution from the protected browser or IT. A maintainer-only offline procedure is below.

**What this fixes.** Conda 26.9.4 requires corrected base CA packages. A successful standard update covers that dependency requirement in the installation environment, normally base; it does not require an extra identical base repair.

**Limits and follow-up.** Other environments require 12. Conda-forge certifi requires Python ≥3.11; Anaconda main requires ≥3.10. Retired Anaconda main platforms, including osx-64 Intel Macs, have no corrected packages. If requirements cannot be met, plan a supported environment/distribution change. Solver/plugins need 07 where used.

#### Maintainer procedure: offline first-update repair

Conda supports installing verified local package files without going online:

```bash
# Replace with compatible files obtained and verified by the maintainer.
conda install --offline --name base ./CA_PACKAGE.conda ./CERTIFI_PACKAGE.conda
```

This is an application of Conda’s documented offline-install feature, not an extra universal incident one-liner. **Explicit local files bypass dependency solving.** Select packages and dependencies compatible with this installation; do not guess builds or force them with `--no-deps`. Once its actual download path is corrected, use the normal official update above.

Sources: [Conda’s incident instructions](https://conda.org/blog/2026-10-21-emudhra-security-updates/), [release requirements and platform limits](https://github.com/conda/conda/issues/16880), [documented offline packages and dependency limitation](https://docs.conda.io/projects/conda/en/latest/user-guide/concepts/installing-with-conda.html), [install options](https://docs.conda.io/projects/conda/en/stable/commands/install.html).

[Back to action 06](#06-conda-miniconda-anaconda-and-miniforge)

### A07. Mamba and Conda’s libmamba solver

**Download safety and ordering.** The update route has the same first-download condition as Conda. A fixed Python/pip alone does not prove the native libmamba download path uses a corrected CA file.

**What this fixes.** Updating Mamba and supporting libraries together lets the solver select compatible packages. The corrected CA minimum protects its native HTTPS route. Some corrected packages change only the build number.

**Limits and follow-up.** This is not an instruction to install Mamba on every computer. If conda-pypi is installed, include it in the update: the plugin’s corrected requirements are in its Conda packages at 0.13.0 build 1 / PyPI release 0.13.1. Follow vendor compatibility notes for rattler; conda-rattler-solver 0.2.0 is incompatible with py-rattler 0.28.0. Repodata hotfixes do not update already installed packages.

Sources: [Mamba and solver incident guidance](https://conda.org/blog/2026-10-21-emudhra-security-updates/#conda-libmamba-solver-and-conda-pypi), [Mamba’s corrected package rebuild](https://github.com/conda-forge/mamba-feedstock/pull/403).

[Back to action 07](#07-mamba-and-condas-libmamba-solver)

### A08. Micromamba

**Download safety and ordering.** Micromamba is self-contained. Its old self-updater may use a different verifier from the OS or Conda. Use a verified vendor download through 02/IT, or a downloader whose incident fix is confirmed, before relying on online self-update.

**What this fixes.** The vendor documents the normal update mechanism. Updating the separate program and its environments addresses the right installations.

**Limits and follow-up.** The sources checked did not establish a minimum incident-fixed Micromamba binary. Obtain vendor build evidence or a sufficiently complete test of its actual download path; do not mark it fixed solely because the version is current.

Sources: [Mamba and solver incident guidance](https://conda.org/blog/2026-10-21-emudhra-security-updates/#conda-libmamba-solver-and-conda-pypi), [Micromamba installation/self-update documentation](https://mamba.readthedocs.io/en/latest/installation/micromamba-installation.html).

[Back to action 08](#08-micromamba)

### A09. uv and the Python copies it manages

**Download safety and ordering.** A protected pip from 11 can fetch pip-installed uv. Homebrew follows 04. A still-affected standalone uv cannot be declared safe merely because self-update is a documented command: use independently verified vendor artifacts through 02/IT if necessary.

**What this fixes.** uv’s TLS fix was in 0.13.4; 0.13.5 also makes refreshed Python 3.11+ builds available with fixed pip/ensurepip starters. Update uv first so it knows the newer builds, then upgrade the managed Python.

**Limits and follow-up.** The refreshed starter pip is different from the later Python-level key block in 10. Existing environments and exact old interpreter paths may remain unchanged. Do not delete old Python folders while environments reference them. A uv-managed Python can remain after uv was removed.

Sources: [uv update instructions](https://docs.astral.sh/uv/getting-started/installation/), [uv 0.13.5 incident fixes](https://github.com/astral-sh/uv/releases/tag/0.13.5), [managed Python upgrade behavior](https://docs.astral.sh/uv/concepts/python-versions/).

[Back to action 09](#09-uv-and-the-python-copies-it-manages)

### A10. Python installations

**Download safety and ordering.** Conda/Homebrew/uv must already satisfy their own download conditions. For a python.org installer, obtain it through the protected browser and retain normal installer/publisher checks. An interpreter update is not a safe way to bypass an affected package manager that downloads it.

**What this fixes.** The fixed Python ssl verifier blocks the compromised keys. Ordinary users of that module, including normal pip connections, gain that protection even if an old root remains in a separate CA file.

**Limits and follow-up.** Native extensions or other executables can use another verifier. Older unsupported Python needs a maintained build/migration. Exact venv paths and Jupyter kernels can keep another interpreter: check inside the notebook and restart it. Upstream versions and distributor backports are alternative evidence, not interchangeable version-number guesses.

Sources: [official release/download pages](https://www.python.org/downloads/), [Python’s fixed-runtime announcement](https://discuss.python.org/t/python-3-15-2-3-14-10-3-13-18-3-12-16-and-3-11-18-are-out/109678), [Conda install/version constraints](https://docs.conda.io/projects/conda/en/stable/commands/install.html), [uv Python upgrades](https://docs.astral.sh/uv/concepts/python-versions/).

[Back to action 10](#10-python-installations)

### A11. pip in each Python environment

**Download safety and ordering.** Route A relies on the actual interpreter’s confirmed compromised-key block. Route B names an exact wheel and SHA-256 checksum from the official Python advisory; pip rejects substituted contents that do not match it. Obtain that command through the protected browser/trusted advisory. This provides a specific first-download check instead of assuming old pip is safe.

**What this fixes.** pip 26.2.2 replaces its included certifi. OS certificate edits alone do not remove the old copy inside pip; updating importable application certifi also does not update pip’s copy.

**Limits and follow-up.** The system/custom CA configuration still needs checking. Include Conda, .venv, tox/nox, user installs, isolated tools and notebook kernels. ensurepip/virtualenv can create another old pip later; use 09/10/20 or repeat the verified first update in that new environment. Do not add persistent sitecustomize/CA overrides as an assumed fix.

Sources: [Python’s incident advisory and first-update command](https://discuss.python.org/t/information-for-python-users-regarding-emudhra-emsign-ca-certificate-security-incident/109576), [pip’s upgrade instructions](https://pip.pypa.io/en/stable/installation/#upgrading-pip), [ensurepip documentation](https://docs.python.org/3/library/ensurepip.html), [pip release notes](https://pip.pypa.io/en/stable/news/), [pip HTTPS trust behavior](https://pip.pypa.io/en/stable/topics/https-certificates/), [pip checksum checks](https://pip.pypa.io/en/stable/topics/secure-installs/).

[Back to action 11](#11-pip-in-each-python-environment)

### A12. Python certificate packages in each environment

**Download safety and ordering.** The corrected pip/Conda download route is established first. This ordering avoids fetching the certificate fix through a still-unconfirmed installer. If the actual client instead uses a custom CA list, update/check that selected list separately.

**What this fixes.** The minimum certifi is 2026.10.19. Conda’s ca-certificates minimum is 2026.10.19 on conda-forge and 2026.10.20 on Anaconda main. These repair environment/application bundles separately from pip’s included bundle.

**Limits and follow-up.** Check active pins (19) and used CA overrides (28). Conda Python/platform availability limits from A06 still apply. Updating base does not update every environment, and changing a package not used by the connection cannot establish protection.

Sources: [Conda’s incident instructions](https://conda.org/blog/2026-10-21-emudhra-security-updates/), [release requirements and platform limits](https://github.com/conda/conda/issues/16880), [documented offline packages and dependency limitation](https://docs.conda.io/projects/conda/en/latest/user-guide/concepts/installing-with-conda.html), [install options](https://docs.conda.io/projects/conda/en/stable/commands/install.html), [Python’s incident advisory and first-update command](https://discuss.python.org/t/information-for-python-users-regarding-emudhra-emsign-ca-certificate-security-incident/109576), [pip’s upgrade instructions](https://pip.pypa.io/en/stable/installation/#upgrading-pip), [ensurepip documentation](https://docs.python.org/3/library/ensurepip.html), [Requests certificate configuration](https://requests.readthedocs.io/en/stable/user/advanced/), [HTTPX SSL defaults and overrides](https://www.python-httpx.org/advanced/ssl/), [Render’s application remediation/verification guidance](https://render.com/docs/advisory-emudhra).

[Back to action 12](#12-python-certificate-packages-in-each-environment)

### A13. Node.js, npm/npx, Yarn, pnpm and Corepack

**Download safety and ordering.** Package-manager routes depend on their earlier fixes. The official installer route uses the protected browser and normal publisher checks. An old nvm/fnm/other downloader is not automatically safe because Node’s new release is fixed: verify its connection/artifact checking or use the verified distribution supplied by IT.

**What this fixes.** The Node security releases fix its TLS checking. Package managers using its normal JavaScript networking do not need a separate npm version upgrade just to replace certificates.

**Limits and follow-up.** Unsupported/EOL Node lines need a maintained fix/migration. Check .nvmrc/.node-version, Volta, editor, CI and container copies. Plain Homebrew node can change major; do not assume it is node@24. Additional CA settings/custom HTTPS code need review. Keep Corepack/integrity/signature checks enabled.

#### pnpm 12 and independently packaged managers

pnpm 12 is a self-contained executable; updating system Node does not prove its own networking fixed. The documented command is `pnpm self-update`, but inside a project with a `packageManager` pin it can change the project pin rather than the global installation. No incident-specific fixed pnpm 12 minimum was established here. Confirm its own vendor build or actual client; do not force a pnpm major upgrade as an assumed certificate fix.

`command -v node` on macOS/Linux or `Get-Command node -All` in PowerShell helps identify installed copies. `volta pin` changes a project setting and should be reviewed as a project change.

Sources: [Node’s download page](https://nodejs.org/en/download), [Node security release](https://nodejs.org/en/blog/vulnerability/october-2026-security-releases), [nvm](https://github.com/nvm-sh/nvm), [nvm-windows](https://github.com/nvm-windows/nvm), [fnm](https://github.com/Schniz/fnm), [Volta install command](https://docs.volta.sh/reference/install), [npm and Node relationship](https://docs.npmjs.com/about-npm-versions/), [Corepack documentation](https://github.com/nodejs/node/blob/main/deps/corepack/README.md), [pnpm’s current installation architecture](https://pnpm.io/installation/).

[Back to action 13](#13-nodejs-npmnpx-yarn-pnpm-and-corepack)

### A14. Bun

**Download safety and ordering.** Use a protected existing package manager, a confirmed protected self-updater, or verified official artifacts obtained through 02/IT. The fixed version itself does not prove an affected old updater securely downloaded it.

**What this fixes.** Bun 1.4.4 removes the relevant root and blocks the compromised keys, including with alternative CA settings. The runtime is separate from Node.

**Limits and follow-up.** Compiled Bun apps contain the old runtime until rebuilt. Do not infer their status from a newer Bun on the development computer.

Sources: [Bun’s incident security advisory](https://github.com/oven-sh/bun/security/advisories/GHSA-26q8-h657-pf84), [Bun update instructions](https://bun.sh/docs/installation).

[Back to action 14](#14-bun)

### A15. Deno

**Download safety and ordering.** As for Bun, confirm the first-download route or use verified vendor artifacts through 02/IT. A documented self-update command alone is not evidence about an old binary’s verifier.

**What this fixes.** Deno’s 2.9.8 release explicitly includes the incident TLS fix.

**Limits and follow-up.** Other copies and distributed self-contained applications need their own runtime/build evidence. Updating Node does not replace them.

Sources: [Deno 2.9.8 release](https://github.com/denoland/deno/releases/tag/v2.9.8), [Deno update methods](https://docs.deno.com/runtime/getting_started/installation/).

[Back to action 15](#15-deno)

### A16. Pipenv

**Download safety and ordering.** The update runs through an already protected pip or pipx backend. This dependency can be completed first even when pipx’s detailed section appears later.

**What this fixes.** Pipenv’s fixed release updates its own included pip and CA packages; those are separate from the managed project’s pip.

**Limits and follow-up.** A tool update does not update all project environments. Keep the original installation method.

Sources: [Pipenv installation methods](https://pipenv.pypa.io/en/latest/installation.html), [Pipenv vendored-pip/CA fix](https://pipenv.pypa.io/en/latest/changelog.html).

[Back to action 16](#16-pipenv)

### A17. Pixi

**Download safety and ordering.** Use the corrected package manager, confirmed self-updater, or independently verified official download through 02/IT. Standalone Pixi has its own roots, so an OS update alone cannot justify its old self-updater.

**What this fixes.** Pixi 0.82.1 updates webpki-root-certs and rustls-webpki to the incident-fixed dependencies.

**Limits and follow-up.** Standalone and package-manager builds may select different trust stores. Managed environments remain separate.

Sources: [Pixi vendor update documentation](https://pixi.prefix.dev/latest/installation/), [Pixi incident release](https://github.com/prefix-dev/pixi/releases/tag/v0.82.1).

[Back to action 17](#17-pixi)

### A18. Python tools installed in separate environments

**Download safety and ordering.** The backend doing the download must be corrected before network maintenance. --skip-maintenance lets the maintainer inspect without triggering pipx’s automatic shared-library update. Use the shared environment’s own Python with the hash-verified pip route when necessary.

**What this fixes.** These tools run in separate environments. Updating their used installer/runtime/CA packages repairs those copies; updating only the project misses them.

**Limits and follow-up.** A top-level upgrade can leave older dependencies that still satisfy requirements. pipx may delegate to uv; update that backend too. Standalone binaries require vendor update/build evidence. Newly created environments need fixed starters, while old project environments need their own updates.

#### Maintainer check: pipx’s shared environment

`pipx environment --skip-maintenance --value PIPX_SHARED_LIBS` prints its shared environment’s location. Use that environment’s Python with 11 if its pip needs the first-update fix. Older pipx may not support the same flags; check `pipx --help` and identify the environment directly. Do not assume upgrading an app upgrades all its already-installed dependencies.

Sources: [pipx CLI and shared-pip operations](https://pipx.pypa.io/stable/reference/cli.html), [pipx shared environment/backend configuration](https://pipx.pypa.io/stable/reference/environment-variables.html), [uv isolated-tool upgrades](https://docs.astral.sh/uv/concepts/tools/), [Poetry self commands and Windows caveat](https://python-poetry.org/docs/cli/), [PDM CLI](https://pdm-project.org/en/latest/reference/cli/), [Hatch installation variants](https://hatch.pypa.io/latest/install/), [tox installation](https://tox.wiki/en/latest/installation.html), [Nox documentation](https://nox.thea.codes/en/stable/), [Poetry’s own environment and self commands](https://python-poetry.org/docs/cli/#self).

[Back to action 18](#18-python-tools-installed-in-separate-environments)

### A19. Project version files and saved environments

**Download safety and ordering.** Regeneration downloads run through the corrected tool. New artifact checksums must come through that protected route, not be copied from an untrusted response.

**What this fixes.** Corrected definitions and saved environments prevent a later rebuild restoring the old program.

**Limits and follow-up.** Check requirements*.txt, pyproject.toml, locks, Conda/Pixi specs, Dockerfiles, .nvmrc/.node-version, Volta/packageManager settings, hook runtime versions and workflows. Review the dependency diff. Historical locks need not be rewritten; label unsuitable online rebuilds and provide a corrected active definition. Old model/source files are not automatically malicious.

**Why recreation can work.** A replacement removes the old environment’s installed copies, provided its creator and saved requirements select fixed versions. An old creator can still supply old starter pip, and unchanged requirements can reinstall old certificate packages. Complete 11 before other package downloads and check 12 after setup. Python treats project virtual environments as disposable; the incident advisory also supports updating existing environments. Keep project files outside the environment and test the replacement before removing the old one.

Sources: [uv targeted lock upgrades](https://docs.astral.sh/uv/concepts/projects/sync/), [Poetry update command](https://python-poetry.org/docs/cli/#update), [pip-tools targeted upgrades](https://pip-tools.readthedocs.io/en/stable/), [GitHub cache management](https://docs.github.com/en/actions/how-tos/manage-workflow-runs/manage-caches), [Python virtual-environment lifecycle](https://docs.python.org/3/library/venv.html), [Python incident guidance for existing environments](https://discuss.python.org/t/information-for-python-users-regarding-emudhra-emsign-ca-certificate-security-incident/109576).

[Back to action 19](#19-project-version-files-and-saved-environments)

### A20. virtualenv: the tool that creates Python environments

**Download safety and ordering.** The package update uses corrected pip/the verified existing manager. It is not safe to repair the creator by ignoring a warning from the installer fetching it.

**What this fixes.** virtualenv 21.14.12 includes pip 26.2.2 as its starter. New environments can then begin with fixed pip.

**Limits and follow-up.** Updating pre-commit alone may leave older virtualenv installed. Older ensurepip wheels and other creators can still recreate old pip; check 09–11. Existing environments do not automatically change when the creator changes.

Sources: [virtualenv seed fixes](https://virtualenv.pypa.io/en/latest/changelog.html).

[Back to action 20](#20-virtualenv-the-tool-that-creates-python-environments)

### A21. pre-commit: recreate saved hook environments

**Download safety and ordering.** Cache deletion itself does not download code. Recreating hooks does: Git, the environment creator and runtime downloaders must be protected first. An old fixed-version setting can still force a vulnerable download, so correct it before rebuilding.

**What this fixes.** A fixed creator plus recreation replaces old hook installers/runtimes without changing the source versions of the checks. install-hooks installs environments without running the hooks.

**Limits and follow-up.** Do not use pre-commit autoupdate as cache repair; it changes hook revisions. A shell’s default Node/Go is not the private hook copy. Rebuild Go-based executables with the corrected compiler. In CI, remove only affected pre-commit caches after correcting their inputs.

Sources: [virtualenv seed fixes](https://virtualenv.pypa.io/en/latest/changelog.html), [pre-commit cache and install-hooks commands](https://pre-commit.com/), [Go’s security release history](https://go.dev/doc/devel/release), [GitHub CLI’s patched Go build](https://github.com/cli/cli/releases/tag/v2.102.1).

[Back to action 21](#21-pre-commit-recreate-saved-hook-environments)

### A22. Code editors and AI coding tools

**Download safety and ordering.** For each row, use its supported updater with publisher/integrity checks, or a verified official distribution obtained through the protected browser. Package-manager routes require the actual runtime/store used to download to be corrected. This guide does not establish that every old private app updater is safe; if its route is affected/unconfirmed, use verified vendor/IT distribution before online self-update.

**What this fixes.** VS Code’s maintainer confirms a backport in 1.143, despite the older-looking Electron version. Other tools need their own included runtime/build confirmation. The matrix below distinguishes normal update instructions from confirmed incident fixes.

**Limits and follow-up.** Do not manually replace private runtime files, delete chats/settings, or disable security checks. Updating terminal Node does not replace an app’s own Node. Updating a parent app does not patch independent MCP servers, extensions or remote hosts. Native Claude can include its own runtime; Codex CLI and desktop/bundled tools are separate.

| Same tool as Part 1 | Download/fix evidence and remaining condition |
| --- | --- |
| VS Code | Microsoft confirms the backport. Keep normal updater checks and verify the remote/server copies separately. |
| Cursor | The official method is documented; no minimum incident-fixed editor/CLI version was confirmed here. A VS Code fix does not automatically cover a fork. |
| Windsurf | The supported updater is the normal path. A minimum incident-fixed app/runtime was not confirmed here. |
| Codex desktop | User reports in the linked issue do not establish every bundle/rollout. Check each downloaded package-installation runtime after app update. |
| Codex CLI, npm | The download is made by the already corrected npm/Node. CLI update does not prove desktop private bundles changed. |
| Codex CLI, Homebrew | Download conditions from 04 apply. It updates that CLI cask, not independently downloaded desktop dependencies. |
| Claude Code, direct | Vendor documents automatic/manual update and binary-integrity verification. No minimum incident-fixed Claude release was established in the checked changelog. |
| Claude Code, package manager | Correct the package manager first; update the existing channel. System Node/Bun is not evidence about the runtime included in Claude. |

Sources: [official installer](https://www.cursor.com/downloads), [VS Code update page](https://code.visualstudio.com/updates/v1_143), [Microsoft maintainer confirmation of the backport](https://github.com/microsoft/vscode/pull/343490#issuecomment-6344976686), [VS Code backport PR](https://github.com/microsoft/vscode/pull/342838), [Cursor CLI updates](https://docs.cursor.com/en/cli/installation), [Windsurf vendor troubleshooting/update notes](https://docs.windsurf.com/troubleshooting/windsurf-common-issues), [OpenAI’s documented CLI update routes](https://developers.openai.com/cookbook/examples/codex/using_goals_in_codex), [Codex runtime issue](https://github.com/openai/codex/issues/56076), [Claude Code updates and signature verification](https://code.claude.com/docs/en/setup), [Claude Code changelog](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md).

[Back to action 22](#22-code-editors-and-ai-coding-tools)

### A23. Development and cloud command-line tools

**Download safety and ordering.** Homebrew/package-manager routes inherit the stated earlier prerequisites. For vendor installers, use a verified official distribution through the protected browser with expected publisher/signature checks. The checked documentation supplies the commands but not an independent bootstrap proof for every affected old CLI self-updater; do not assume that proof exists.

**What this fixes.** GitHub CLI 2.102.1 explicitly uses the corrected Go runtime. Other tools can include independent Python, native verifiers and extension CA packages; update the whole supported distribution and assess those used components.

**Limits and follow-up.** No universal incident-fixed minimum was verified here for standalone Git LFS, Azure CLI/ML, AWS CLI or gcloud. Separate curl builds can use Schannel or file-based certificate lists; OpenSSL tools can select CA files/directories/stores explicitly. There is no universal update command for every supplier’s distribution. Correct the selected trust list or use the supplier-confirmed incident fix, and test that client. Updating an unrelated Windows certificate store or merely replacing an executable is not sufficient evidence. Git LFS 3.8.1 being included in Git for Windows does not certify every standalone build. az upgrade checks extensions too. Updating boto3/certifi in a project does not update CLI v2. gcloud’s component update route differs from OS-owned installations.

| Same tool as Part 1 | What establishes completion |
| --- | --- |
| GitHub CLI | Corrected vendor build at/above 2.102.1; check the copy actually invoked. |
| Git LFS | Vendor build/trust-store evidence or a complete test of its actual network client. |
| Azure CLI / ML | Check the CLI’s used certifi and separately active extension bundles; a version number alone was not established as proof. |
| AWS CLI v2 | Check the private CLI distribution; a project SDK update is separate. |
| gcloud | Check selected Python, private CA settings and the actual network client. |
| Separate curl/OpenSSL tools | Supplier-confirmed correction of the used verifier/CA list, or sufficiently complete verification of that actual client and configuration. Windows’ built-in curl is covered by 01/A01; Git-bundled copies by 05/A05. |

Sources: [GitHub CLI release](https://github.com/cli/cli/releases/tag/v2.102.1), [GitHub CLI installation methods](https://github.com/cli/cli#installation), [Git LFS releases](https://github.com/git-lfs/git-lfs/releases/tag/v3.8.1), [Azure CLI update documentation](https://learn.microsoft.com/en-us/cli/azure/update-azure-cli?view=azure-cli-latest), [Azure extension update](https://learn.microsoft.com/en-us/cli/azure/extension?view=azure-cli-latest#az-extension-update), [AWS CLI update documentation](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html), [gcloud component updates](https://docs.cloud.google.com/sdk/docs/components), [curl distributions and TLS backends](https://curl.se/windows/microsoft.html), [curl certificate-list selection](https://curl.se/docs/sslcerts.html), [OpenSSL verification and trust-store options](https://docs.openssl.org/3.5/man1/openssl-verification-options/).

[Back to action 23](#23-development-and-cloud-command-line-tools)

### A24. Docker Desktop and container images

**Download safety and ordering.** Desktop comes through its supported updater or verified vendor installer obtained with 02. Image build steps then download through the corrected OS/package/runtime clients inside the image; host protection alone does not establish their safety. Repair/verify the base and installers before online dependency downloads.

**What this fixes.** Desktop 4.96.3 covers the identified bundled fixes; earlier October hotfixes were partial. New images must contain corrected CA/runtime/application packages. --pull fetches the selected base; --no-cache reruns build steps.

**Limits and follow-up.** An old exact digest still selects an old image even with --pull. Existing containers and remote Linux Docker Engine/CLI/plugins are separate. Use their vendor updates as applicable. Do not delete all Docker data or unrelated caches.

Sources: [Docker Desktop incident releases](https://docs.docker.com/desktop/release-notes/), [macOS installation/update distribution](https://docs.docker.com/desktop/setup/install/mac-install/), [Docker build cache and pull guidance](https://docs.docker.com/build/building/best-practices/), [host/container trust-store separation](https://docs.docker.com/engine/network/ca-certs/), [Docker Desktop Windows installation](https://docs.docker.com/desktop/setup/install/windows-install/).

[Back to action 24](#24-docker-desktop-and-container-images)

### A25. WSL, remote machines, Azure ML and CI

**Download safety and ordering.** The host OS update route does not protect every guest or remote environment. Each repeats the prerequisite checks. Ubuntu APT verifies signed repository information and package checksums using its archive keys, independently of website CAs. For Amazon Linux retain the configured RPM signature checks; AL2023 enables package GPG checks by default. Do not bypass authentication to force a CA update. For CI/cloud images, establish trusted base artifacts and corrected installers before downloads.

**What this fixes.** These systems have independent installed tools and trust stores. Updating where the code executes puts the relevant fix into the actual environment.

**Limits and follow-up.** wsl --update does not update Linux packages. Runner auto-update does not maintain the machine’s tools. setup-node selects project Node, not every action’s included Node. For Node 24, node-version: 24 must actually resolve to ≥24.22.1; caches/mirrors can be stale. Preserve reviewed action tags/SHAs. Updating local Azure CLI does not mutate an old Azure ML environment. AL2023 repository metadata signature checks are separate from package checks and disabled by default: do not claim a successful command proves updates were not withheld. Release pins can retain old packages; verify the installed CA/runtime fix.

Sources: [WSL commands](https://learn.microsoft.com/en-us/windows/wsl/basic-commands), [Ubuntu’s incident package fix](https://ubuntu.com/blog/emudhra-ca-compromise-fixes-available), [AWS’s Amazon Linux CA fix](https://aws.amazon.com/security/security-bulletins/2026-150-aws/), [Azure ML environment management](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-manage-environments-v2?view=azureml-api-2), [GitHub action runtime metadata](https://docs.github.com/en/actions/reference/workflows-and-actions/metadata-syntax#runs-for-javascript-actions), [self-hosted runner responsibilities](https://docs.github.com/en/actions/reference/runners/self-hosted-runners), [targeted GitHub cache deletion](https://docs.github.com/en/actions/how-tos/manage-workflow-runs/manage-caches), [setup-node’s documented version inputs](https://github.com/actions/setup-node), [Ubuntu APT signature/checksum verification](https://manpages.ubuntu.com/manpages/noble/man8/apt-secure.8.html), [Amazon Linux package vs repository verification](https://docs.aws.amazon.com/linux/al2023/ug/repo-metadata-signing.html).

[Back to action 25](#25-wsl-remote-machines-azure-ml-and-ci)

### A26. Electron apps your team develops

**Download safety and ordering.** The dependency download runs through the already protected package manager/Node, with normal artifact-integrity checks. The distributed app then follows the team’s normal signing/release process.

**What this fixes.** Electron’s advisory names these fixed branches. Rebuilding includes the corrected verifier in the app that users run.

**Limits and follow-up.** Updating only the build computer does not change old distributed apps. Extra included downloaders/runtimes need their own assessment. Documented vendor backports are also acceptable.

Sources: [Electron security advisory](https://github.com/electron/electron/security/advisories/GHSA-jr5w-w6x2-84p9), [npm install syntax](https://docs.npmjs.com/cli/v11/commands/npm-install/).

[Back to action 26](#26-electron-apps-your-team-develops)

### A27. Python publishing and signing tools

**Download safety and ordering.** Use corrected pip/the original package manager for the Python tool. Retrieve/review the action reference through the protected source/CI route. Do not remove the team’s immutable-SHA or signing requirements just to copy the example.

**What this fixes.** Publishing has separate network/signing clients. The specific Sigstore/action versions update those distributions; the build environment’s pip alone does not update them.

**Limits and follow-up.** Twine’s actual environment still needs checking. Do not replace your signing keys or release credentials solely because a tool is listed; investigate real compromise evidence separately.

Sources: [Python advisory’s publishing-tool requirements](https://discuss.python.org/t/information-for-python-users-regarding-emudhra-emsign-ca-certificate-security-incident/109576), [Sigstore 4.5.1](https://pypi.org/project/sigstore/4.5.1/), [PyPA publishing action v1.14.3](https://github.com/pypa/gh-action-pypi-publish/releases/tag/v1.14.3).

[Back to action 27](#27-python-publishing-and-signing-tools)

### A28. Python apps that make HTTPS connections

**Download safety and ordering.** The certifi update is downloaded through the already corrected installer. Restart/redeploy does not establish network safety by itself; the running client must select the corrected verifier/list.

**What this fixes.** Application certifi is separate from pip’s included list. Requests/HTTPX document their selected CA sources, so correcting the list actually used addresses application traffic.

**Limits and follow-up.** Check REQUESTS_CA_BUNDLE, CURL_CA_BUNDLE, SSL_CERT_FILE, SSL_CERT_DIR, verify=, SSL contexts and app-supplied files. IT should manage corporate extra roots. aiohttp, gRPC and native helpers can differ; updating an unused certifi does not certify them.

Sources: [Requests certificate configuration](https://requests.readthedocs.io/en/stable/user/advanced/), [HTTPX SSL defaults and overrides](https://www.python-httpx.org/advanced/ssl/), [Render’s application remediation/verification guidance](https://render.com/docs/advisory-emudhra).

[Back to action 28](#28-python-apps-that-make-https-connections)

### A29. Hugging Face and extra model downloaders

**Download safety and ordering.** Python package updates use the corrected installer. Native model downloaders may use another TLS implementation; establish their protection before asking them to download models or fixes.

**What this fixes.** Hugging Face documents hf_xet as a separate Rust downloader. That establishes an additional component to assess, not that it is vulnerable.

**Limits and follow-up.** Do not blindly upgrade the whole ML stack or redownload every model. Cached model weights do not mean every later metadata/tokenizer/runtime request is offline.

Sources: [Hugging Face’s Xet client documentation](https://github.com/huggingface/hub-docs/blob/main/docs/hub/xet/using-xet-storage.md), [Hub installation instructions](https://huggingface.co/docs/huggingface_hub/installation).

[Back to action 29](#29-hugging-face-and-extra-model-downloaders)

### A30. LLM API libraries, agent frameworks and MCP servers

**Download safety and ordering.** Updates run through the corrected installer/backend. Protecting a parent editor does not authenticate downloads made by an independent MCP subprocess, SDK helper or proxy; check those actual routes too.

**What this fixes.** These HTTPS connections carry API keys, prompts and responses. The selected Python/Node trust fix protects default clients; AWS and LiteLLM have additional specific vendor fixes.

**Limits and follow-up.** Framework names do not identify every transport. Check custom proxies, explicit CA lists and standalone MCP environments. A compatible trust fix does not automatically require updating every framework. Updating botocore does not update AWS CLI v2.

Sources: [HTTPX transport/trust configuration](https://www.python-httpx.org/advanced/ssl/), [AWS SDK bulletin](https://aws.amazon.com/security/security-bulletins/2026-150-aws/), [LiteLLM’s vendor fix and branch backports](https://github.com/BerriAI/litellm/releases/tag/v1.106.3).

[Back to action 30](#30-llm-api-libraries-agent-frameworks-and-mcp-servers)

### A31. Ollama, LM Studio and other local LLM apps

**Download safety and ordering.** An old app updater is not automatically safe because the app is listed here. Use a confirmed protected updater or verified official distribution through 02/IT. Model/runtime downloads can use separate components.

**What this fixes.** The vendor’s normal app update replaces its included components. Local inference with existing files and no network connection does not expose that operation to this HTTPS attack.

**Limits and follow-up.** The next model search/download, runtime update or API request is a separate network operation. Being listed establishes scope to check, not confirmed vulnerability or a confirmed fix.

Sources: [Ollama’s update FAQ](https://docs.ollama.com/faq), [LM Studio’s offline/network and runtime behavior](https://lmstudio.ai/docs/app/offline).

[Back to action 31](#31-ollama-lm-studio-and-other-local-llm-apps)

### A32. Inference, training, evaluation and notebooks

**Download safety and ordering.** The same verified installer/client prerequisites apply on the GPU/cloud/container system. A local successful update or model benchmark does not authenticate a remote environment’s downloads.

**What this fixes.** Correcting the component making HTTPS connections addresses this incident without unrelated GPU/framework changes.

**Limits and follow-up.** A kernel may use another Python from the notebook server. llama.cpp/native downloaders may not use Python certifi. Confirm the actual linked client/vendor build; model accuracy/performance tests are not certificate tests.

Sources: [application/container remediation guidance](https://render.com/docs/advisory-emudhra), [Hugging Face native-download architecture](https://github.com/huggingface/hub-docs/blob/main/docs/hub/xet/using-xet-storage.md), [Docker host/container trust separation](https://docs.docker.com/engine/network/ca-certs/).

[Back to action 32](#32-inference-training-evaluation-and-notebooks)

### A33. Running services, workers and scheduled jobs

**Download safety and ordering.** The build must obtain dependencies through corrected, verified routes. Deployment then selects that reviewed artifact through the organisation’s normal authenticated release process; a fresh deployment of an old image is not remediation.

**What this fixes.** Restart/redeployment puts the corrected packages into the process making real connections. Testing there checks the actual runtime/list in use.

**Limits and follow-up.** A new build in a registry does not prove rollout. curl in a service shell can use another certificate list from HTTPX/Requests/native clients. There is no universal safe redeploy command for every platform.

Sources: [Render’s deployment and verification advisory](https://render.com/docs/advisory-emudhra).

[Back to action 33](#33-running-services-workers-and-scheduled-jobs)

### Connection tests

**Browser check:** first open a normal HTTPS site. Then open both test pages; certificate warnings are expected. **Do not continue past them.**

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

The website certificate and the update file’s vendor signature are **different checks**. The OS explanation in A01 relies on the latter, not on pretending that every certificate list was fixed before the first update. A checksum also helps only when its expected value came from a trusted source; an attacker who can replace both file and checksum on the same intercepted connection defeats that comparison. Keep normal publisher, signature and artifact-integrity checks enabled.

**Unconfirmed fixes stay unconfirmed.** For Micromamba, self-contained pnpm 12, the listed unconfirmed editor/AI/CLI bundles, hf_xet, Ollama and LM Studio, obtain the component’s own vendor/build evidence or sufficiently complete client checks. The checked Microsoft sources do not name a minimum incident-specific Edge version either. Being listed here does not prove vulnerability; a vendor’s silence or “latest” status does not prove protection. If an affected program is confirmed and has no protected update route, stop its affected network operations until repaired.

This guide covers Python/JavaScript/LLM development tools across macOS, Windows and the separate environments they use. It is not a live inventory of every team machine. Additional downloaders and included runtimes should be assessed in the same way.
