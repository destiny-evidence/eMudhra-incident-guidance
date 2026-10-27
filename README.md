# eMudhra certificate incident: developer update guide

**Guidance checked: 26 October 2026.** For macOS and Windows, including Python/JavaScript projects, LLM tools, remote systems and containers.

## Part 1 — What to do

> [!IMPORTANT]
> You will not have all these tools. Only follow items for software you already use. Skip the rest. Do not install a program just because it appears in this guide.

Read the operating-system and browser guidance, then find your tools in the list below and work through those items. Project/CI/server items are for whoever maintains those systems. If you do not recognise a tool or know how it was installed, ask the person supporting that setup rather than trying every command.

### Order of work

Each row is a separate item. Sections 01–28 have matching **A-numbers in Part 2**, explaining the safety of the update method, its place in the order, and what it fixes. Section 29 lists other tools without update instructions.

| Step | Tool or task | Only applies if… |
| --- | --- | --- |
| [01](#01-operating-system-certificate-protection) | Operating-system certificate protection | Everyone. |
| [02](#02-browsers) | Browsers | Everyone: check the browser you use. Do not install all of these browsers. |
| [03](#03-macos-command-line-certificates) | macOS command-line certificates | You use Apple’s bundled curl, Git, Python or similar tools, supplied with macOS or Xcode Command Line Tools. |
| [04](#04-homebrew) | Homebrew | You already use Homebrew on macOS. |
| [05](#05-git-for-windows) | Git for Windows | Windows users with Git installed, including portable copies. |
| [06](#06-conda-miniconda-anaconda-and-miniforge) | Conda, Miniconda, Anaconda and Miniforge | You use any of these Conda distributions. |
| [07](#07-uv) | uv | You use uv. |
| [08](#08-python-installations) | Python installations | You run Python, including through Conda, uv, Homebrew or python.org. |
| [09](#09-pip-in-each-python-environment) | pip in each Python environment | You install packages with pip. Repeat in each environment you use. |
| [10](#10-python-certificate-packages-in-each-environment) | Python certificate packages in each environment | An environment uses certifi or Conda ca-certificates for downloads/API calls. |
| [11](#11-nodejs-npmnpx-yarn-pnpm-and-corepack) | Node.js, npm/npx, Yarn, pnpm and Corepack | You use Node, npm/npx, Yarn, pnpm or Corepack, including project-selected copies. |
| [12](#12-pipenv) | Pipenv | You use Pipenv. |
| [13](#13-python-tools-installed-in-separate-environments) | Python tools installed in separate environments | You use pipx, uv tools/uvx, Poetry, PDM, Hatch, pip-tools, tox or nox. |
| [14](#14-project-version-files-and-saved-environments) | Project version files and saved environments | You maintain project requirements, lockfiles, runtime settings or CI caches. |
| [15](#15-virtualenv-the-tool-that-creates-python-environments) | virtualenv: the tool that creates Python environments | You use virtualenv directly or through pre-commit/tox/nox. |
| [16](#16-pre-commit-recreate-saved-hook-environments) | pre-commit: recreate saved hook environments | You use pre-commit in a project or CI. |
| [17](#17-code-editors-and-ai-coding-tools) | Code editors and AI coding tools | You use one of the listed apps. Update only yours. |
| [18](#18-development-and-cloud-command-line-tools) | Development and cloud command-line tools | You use gh, Git LFS, Azure CLI, AWS CLI, gcloud, or separately installed curl/OpenSSL tools. |
| [19](#19-docker-desktop-and-container-images) | Docker Desktop and container images | You use Docker or development containers. |
| [20](#20-wsl-remote-machines-azure-ml-and-ci) | WSL, remote machines, Azure ML and CI | Your code runs in these places, or you maintain their environments. |
| [21](#21-electron-apps-your-team-develops) | Electron apps your team develops | You build and distribute an Electron app; otherwise skip. |
| [22](#22-python-publishing-and-signing-tools) | Python publishing and signing tools | You publish packages with Twine, Sigstore or PyPA’s publishing Action. |
| [23](#23-python-apps-that-make-https-connections) | Python apps that make HTTPS connections | Your programs use Requests, HTTPX, SDKs or other HTTP libraries. |
| [24](#24-hugging-face-and-extra-model-downloaders) | Hugging Face and extra model downloaders | You use huggingface_hub, hf_xet, Transformers or Datasets downloads. |
| [25](#25-llm-api-libraries-agent-frameworks-and-mcp-servers) | LLM API libraries, agent frameworks and MCP servers | You use provider SDKs, LangChain, LlamaIndex, LiteLLM or MCP services. |
| [26](#26-ollama-lm-studio-and-other-local-llm-apps) | Ollama, LM Studio and other local LLM apps | You use one of these local model apps. |
| [27](#27-inference-training-evaluation-and-notebooks) | Inference, training, evaluation and notebooks | You run vLLM, SGLang, Transformers, llama.cpp, notebooks or cloud/GPU jobs. |
| [28](#28-running-services-workers-and-scheduled-jobs) | Running services, workers and scheduled jobs | You maintain deployed apps, background workers, agents or scheduled jobs. |
| [29](#29-other-tools) | Other tools | You use one of the additional tools listed. |

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

Run:

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
| pip | `python -m pip install --upgrade "uv>=0.13.6"` |

**Check:** `uv --version` should now report **0.13.6 or later**.

[Update evidence and download protection → A07](#a07-uv)

### 08. Python installations

**Applies to:** You run Python, including through Conda, uv, Homebrew or python.org.

**Before:** Update its supplier first: Conda 06, Homebrew 04, or uv 07. Use 02 for official installers.

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
| Downloaded by uv | `uv python upgrade` |
| python.org, 3.14/3.15 | Run the current [python.org installer](https://www.python.org/downloads/) for your existing release line. |
| Homebrew/other supplier | Update the same versioned package; require the fix or a documented backport. |

**Check which Python you are using:**
```bash
python -c "import sys; print(sys.executable); print(sys.version)"
```

The Python project’s **3.11–3.13 releases are source-only**; use your supplier’s corrected package rather than looking for a new python.org installer. Do not replace Apple’s Python or change release lines just to copy an example.

[Why this update method is safe, and what it covers → A08](#a08-python-installations)

### 09. pip in each Python environment

**Applies to:** You install packages with pip. Repeat in each environment you use.

**Before:** Activate the intended environment. Do not update only base and assume other environments changed.

**If pip is already ≥26.2.2, check its configuration; do not reinstall an older version.** Otherwise choose one route:

**A. Python already has the confirmed key-block fix from 08:**

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

[Why this update method is safe, and what it covers → A09](#a09-pip-in-each-python-environment)

### 10. Python certificate packages in each environment

**Applies to:** An environment uses certifi or Conda ca-certificates for downloads/API calls.

**Before:** For pip, complete 09. For Conda, complete 06. Choose the installer that owns the existing package.

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

[Why this update method is safe, and what it covers → A10](#a10-python-certificate-packages-in-each-environment)

### 11. Node.js, npm/npx, Yarn, pnpm and Corepack

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

Normal JavaScript npm/npx/Yarn/pnpm/Corepack using that Node inherit its fix. **Self-contained pnpm 12 needs its own confirmation; see A11.** Do not change Node major versions just to copy this table.

[Why this update method is safe, and what it covers → A11](#a11-nodejs-npmnpx-yarn-pnpm-and-corepack)

### 12. Pipenv

**Applies to:** You use Pipenv.

**Before:** Protect the pip/pipx environment running the update: 09 and, for pipx, 13.

| Installed with | Update |
| --- | --- |
| pipx | `pipx upgrade pipenv` |
| pip | `python -m pip install --upgrade "pipenv>=2026.10.2"` |

**Check:** `pipenv --version` is **≥2026.10.2**. Check the project environment separately using 09–10.

[Why this update method is safe, and what it covers → A12](#a12-pipenv)

### 13. Python tools installed in separate environments

**Applies to:** You use pipx, uv tools/uvx, Poetry, PDM, Hatch, pip-tools, tox or nox.

**Before:** Protect the actual installation backend first: pip 09, uv 07, or the existing package manager.

| Installation | Do this |
| --- | --- |
| pipx | Inspect `pipx list --skip-maintenance`. Repair its shared pip if needed (A13), then `pipx upgrade-shared`. Update the relevant app with `pipx upgrade APP`. |
| Packages inside a pipx app | Inspect `pipx runpip --skip-maintenance APP show pip certifi virtualenv`. Update only used affected packages via that environment’s Python or `pipx runpip`. |
| uv tools | `uv tool list`; update an existing certifi dependency with `uv tool upgrade APP --upgrade-package certifi`. Refresh affected saved uvx environments. |
| Poetry installed directly | `poetry self update`; on Windows follow the original installer’s method because Poetry warns about self commands there. |
| Other pip/pipx-installed tools | Update the relevant tool with the same installer, e.g. `pipx upgrade tox` or `python -m pip install --upgrade tox`. |
| App including its own Python | Update the whole vendor distribution; a project’s pip cannot edit that separate copy safely. |

Replace `APP` with the installed app’s name. **Check its own Python, pip, certifi and virtualenv where used**, not just the managed project.

[Why this update method is safe, and what it covers → A13](#a13-python-tools-installed-in-separate-environments)

### 14. Project version files and saved environments

**Applies to:** You maintain project requirements, lockfiles, runtime settings or CI caches.

**Before:** Fix the installer/runtime that will regenerate the files first.

1. Correct active requirements that would reinstall an affected version.
2. Use the existing project tool to update affected packages, not every dependency:

| Project tool | Existing certifi example |
| --- | --- |
| uv | `uv lock --upgrade-package certifi`, then `uv sync --locked` |
| Poetry | `poetry update certifi` |
| pip-tools | `pip-compile --upgrade-package certifi`, with the project’s normal file arguments; then its normal sync |
| Conda | Update the existing environment definition and regenerate it through the project’s normal workflow. |

3. Refresh affected tool/image caches after fixing their source settings. Keep unrelated data and historical experiment records.

[Why this update method is safe, and what it covers → A14](#a14-project-version-files-and-saved-environments)

### 15. virtualenv: the tool that creates Python environments

**Applies to:** You use virtualenv directly or through pre-commit/tox/nox.

**Before:** Protect pip/the package manager in the environment running virtualenv first.

**If installed with pip:**

```bash
python -m pip install --upgrade "virtualenv>=21.14.12"
python -m virtualenv --version
```

For Conda/pipx, use that existing installer. **Target: ≥21.14.12.** Update it where it actually runs, then recreate affected environments using 16.

[Why this update method is safe, and what it covers → A15](#a15-virtualenv-the-tool-that-creates-python-environments)

### 16. pre-commit: recreate saved hook environments

**Applies to:** You use pre-commit in a project or CI.

**Before:** Complete 05/03 as applicable, 15, and 14 for any old runtime settings. Protect Node/Go downloaders too.

**Once, clear the affected saved environments:**

```bash
pre-commit clean
```

**Then in each project with `.pre-commit-config.yaml`:**

```bash
pre-commit install-hooks
```

Check pip, Node and Go versions **inside the recreated hook environments**. Use pip ≥26.2.2, the relevant Node minimum in 11, and Go **1.26.10 / 1.27.3** on those lines or a confirmed backport. **Leave hook versions unchanged.**

[Why this update method is safe, and what it covers → A16](#a16-pre-commit-recreate-saved-hook-environments)

### 17. Code editors and AI coding tools

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

**After updating:** check separately downloaded runtimes, extensions/MCP servers and remote copies. **For tools without a confirmed fixed version, leave the certificate check pending; updating the app alone is not proof.** See the per-tool table in A17.

[Why this update method is safe, and what it covers → A17](#a17-code-editors-and-ai-coding-tools)

### 18. Development and cloud command-line tools

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

[Why this update method is safe, and what it covers → A18](#a18-development-and-cloud-command-line-tools)

### 19. Docker Desktop and container images

**Applies to:** You use Docker or development containers.

**Before:** Use the normal Desktop updater/verified official installer. Correct image dependency definitions before rebuilding.

1. Update Docker Desktop to **4.96.3 or a later fixed release** with its updater or official [macOS](https://docs.docker.com/desktop/setup/install/mac-install/) / [Windows](https://docs.docker.com/desktop/setup/install/windows-install/) installer.
2. Update affected certificates/runtimes/packages in the Dockerfile/base image and project locks. Change an old pinned image digest to a corrected image.
3. Rebuild through the project workflow; example:

```bash
docker build --pull --no-cache -t TEAM_IMAGE:PATCHED .
```

4. Recreate containers and deploy the new image. Updating Desktop does not repair an old image or running container.

[Why this update method is safe, and what it covers → A19](#a19-docker-desktop-and-container-images)

### 20. WSL, remote machines, Azure ML and CI

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

[Why this update method is safe, and what it covers → A20](#a20-wsl-remote-machines-azure-ml-and-ci)

### 21. Electron apps your team develops

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

[Why this update method is safe, and what it covers → A21](#a21-electron-apps-your-team-develops)

### 22. Python publishing and signing tools

**Applies to:** You publish packages with Twine, Sigstore or PyPA’s publishing Action.

**Before:** Protect the environment doing the publishing using 08–10; CI downloads follow 20.

| Tool | Do this |
| --- | --- |
| Python Sigstore | In its pip-installed environment: `python -m pip install --upgrade "sigstore>=4.5.1"`; otherwise use its original installer. |
| Twine / other publisher | Update its used Python/pip/certifi; no universal fixed Twine version was established. |
| PyPA publishing Action | Use **v1.14.3 or a later fixed release**, or the verified full commit SHA required by team policy. |

```yaml
- uses: pypa/gh-action-pypi-publish@v1.14.3
```

[Why this update method is safe, and what it covers → A22](#a22-python-publishing-and-signing-tools)

### 23. Python apps that make HTTPS connections

**Applies to:** Your programs use Requests, HTTPX, SDKs or other HTTP libraries.

**Before:** Complete 10 for the application’s environment, and check any custom certificate settings.

1. Ensure the **certifi actually used** is **≥2026.10.19**, where that is the selected list.
2. Have the maintainer check custom certificate files/settings.
3. Restart/redeploy, then test the application’s own connection library—not just curl.

[Why this update method is safe, and what it covers → A23](#a23-python-apps-that-make-https-connections)

### 24. Hugging Face and extra model downloaders

**Applies to:** You use huggingface_hub, hf_xet, Transformers or Datasets downloads.

**Before:** Protect Python downloads using 09–10. A separate native downloader needs its own evidence.

```bash
python -m pip show huggingface-hub hf-xet transformers datasets
```

Apply 23 to Python connections. Check the vendor’s fix for extra downloaders such as **hf_xet**, then update through the project’s normal protected workflow if needed. **No fixed/vulnerable hf_xet version was confirmed here: leave that check pending rather than assuming certifi repaired it.**

[Why this update method is safe, and what it covers → A24](#a24-hugging-face-and-extra-model-downloaders)

### 25. LLM API libraries, agent frameworks and MCP servers

**Applies to:** You use provider SDKs, LangChain, LlamaIndex, LiteLLM or MCP services.

**Before:** Protect the actual environment/library making each connection using the earlier relevant steps.

| Used component | Do this |
| --- | --- |
| Python library using certifi | Follow 23 in that environment. |
| JavaScript library/MCP server using normal Node HTTPS | Follow 11 for the Node actually running it. |
| Separate downloader / included runtime | Require its own vendor fix or connection verification. |
| boto3 / Bedrock | Use **botocore ≥1.43.117** with compatible boto3, through the existing project workflow. |
| LiteLLM | Fixed corresponding lines: **1.103.8, 1.104.6, 1.105.4, 1.106.3**, or later vendor-confirmed fixed releases. Update library/proxy/container copies separately. |

Existing pip-installed LiteLLM 1.106 example: `python -m pip install "litellm==1.106.3"`. Use the project lock workflow where applicable; do not downgrade a later fixed release or move every project to this line.

[Why this update method is safe, and what it covers → A25](#a25-llm-api-libraries-agent-frameworks-and-mcp-servers)

### 26. Ollama, LM Studio and other local LLM apps

**Applies to:** You use one of these local model apps.

**Before:** Use the normal vendor updater/verified official distribution, with its download checks intact.

| App | Do this |
| --- | --- |
| Ollama on macOS/Windows | Accept its automatic desktop update and choose **Restart to update**, or use the current official installer. |
| LM Studio | Update the app normally; also check separately downloaded model-running runtimes in its runtime tools. |
| Similar app | Check its own model/runtime downloader and API connections. |

**No incident-fixed minimum for these apps was confirmed here.** Obtain vendor confirmation/test results before marking the relevant downloader fixed.

[Why this update method is safe, and what it covers → A26](#a26-ollama-lm-studio-and-other-local-llm-apps)

### 27. Inference, training, evaluation and notebooks

**Applies to:** You run vLLM, SGLang, Transformers, llama.cpp, notebooks or cloud/GPU jobs.

**Before:** Apply earlier fixes in the environment doing downloads/API calls, not only the laptop submitting the job.

1. Update its used certificates, runtime, API libraries and extra downloaders.
2. Rebuild images/environments and use them in fresh jobs (19–20).
3. Check the notebook kernel’s Python/packages and restart it.
4. Verify its actual network client before new downloads/API traffic.

**Do not automatically upgrade CUDA, GPU drivers, model weights or every ML framework for this incident.**

[Why this update method is safe, and what it covers → A27](#a27-inference-training-evaluation-and-notebooks)

### 28. Running services, workers and scheduled jobs

**Applies to:** You maintain deployed apps, background workers, agents or scheduled jobs.

**Before:** Correct their definitions, packages and images with the relevant earlier steps.

1. Build the updated environment/image using the normal release process.
2. Deploy it and restart affected web services, workers, scheduled jobs, notebook servers and agent/model processes.
3. Confirm the **running** program uses the new version/image.
4. Test its own HTTPS library against normal and affected-certificate sites. Do not send real credentials or data to test sites.

[Why this update method is safe, and what it covers → A28](#a28-running-services-workers-and-scheduled-jobs)

### Final Checklist

- [ ] Relevant tools updated; unused tools marked **not applicable**.
- [ ] The actual program/environment was checked, not a different installed copy.
- [ ] Current project files/caches will not bring the old version back.
- [ ] Running apps/jobs were restarted or redeployed where needed.
- [ ] Relevant application/service connection tests pass; unconfirmed vendor/runtime checks remain **pending confirmation**.

### 29. Other tools

I wasn’t sure whether anyone on the team uses these tools, so I haven’t included update instructions. They also need updating; let me know if you’d like me to add instructions.

- [Mamba](https://conda.org/blog/2026-10-21-emudhra-security-updates/#mamba)
- [Conda’s libmamba solver](https://conda.org/blog/2026-10-21-emudhra-security-updates/#conda-libmamba-solver-and-conda-pypi)
- [Micromamba](https://mamba.readthedocs.io/en/latest/installation/micromamba-installation.html)
- [Bun](https://github.com/oven-sh/bun/security/advisories/GHSA-26q8-h657-pf84)
- [Deno](https://github.com/denoland/deno/releases/tag/v2.9.8)
- [Pixi](https://github.com/prefix-dev/pixi/releases/tag/v0.82.1)

## Part 2 — Why each update method is safe, and what it covers

**A01–A28 correspond to sections 01–28 in Part 1.** Each entry separates two questions: whether the first update can be obtained safely, and whether the resulting update fixes the relevant component. “Documented update command” and “confirmed incident fix” are not the same thing.

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

**Scope:** This assessment covers standard installations and is based on maintainer guidance and source inspection, not execution tests. Separately installed curl or custom certificate files need their own protection. Self-update can change shell/PATH settings.

[Back to action 07](#07-uv)

### A08. Python installations

**Download safety and ordering.** Conda/Homebrew/uv must already satisfy their own download conditions. For a python.org installer, obtain it through the protected browser and retain normal installer/publisher checks. An interpreter update is not a safe way to bypass an affected package manager that downloads it.

**What this fixes.** The fixed Python ssl verifier blocks the compromised keys. Ordinary users of that module, including normal pip connections, gain that protection even if an old root remains in a separate CA file.

**Limits and follow-up.** Native extensions or other executables can use another verifier. Older unsupported Python needs a maintained build/migration. Exact venv paths and Jupyter kernels can keep another interpreter: check inside the notebook and restart it. Upstream versions and distributor backports are alternative evidence, not interchangeable version-number guesses.

**uv guidance checked: 27 October 2026.**

| Claim | Sources | Quotes |
| --- | --- | --- |
| **uv-managed Python coverage:** update uv first, then run `uv python upgrade`. 0.13.6 supplies the fixed Python releases listed in 08; refreshed Python 3.11+ builds also include corrected pip and ensurepip bundles. | [0.13.6 Python builds](https://github.com/astral-sh/uv/releases/tag/0.13.6)<br><br>[Astral’s incident instructions for Python](https://github.com/astral-sh/uv/releases/tag/0.13.5)<br><br>[Upgrade command and scope](https://docs.astral.sh/uv/concepts/python-versions/#upgrading-python-versions) | **0.13.6:** “Add CPython 3.11.18, 3.12.16, 3.13.18, 3.14.10, and 3.15.2”.<br><br>**Incident instructions:** “After upgrading uv, run `uv python upgrade`”; “pip 26.2.2”; “bundled with `ensurepip`”.<br><br>**Scope:** “To upgrade all installed Python versions:”; `uv python upgrade`. |

**uv-managed environments:** Python upgrades stay within each installed release line and retain old copies that environments may still use. Exact patch pins do not move automatically; existing environments need their own pip update. Do not delete old Python folders while environments reference them.

Sources: [official release/download pages](https://www.python.org/downloads/), [Python’s fixed-runtime announcement](https://discuss.python.org/t/python-3-15-2-3-14-10-3-13-18-3-12-16-and-3-11-18-are-out/109678), [Conda install/version constraints](https://docs.conda.io/projects/conda/en/stable/commands/install.html), [uv Python upgrades](https://docs.astral.sh/uv/concepts/python-versions/).

[Back to action 08](#08-python-installations)

### A09. pip in each Python environment

**Download safety and ordering.** Route A relies on the actual interpreter’s confirmed compromised-key block. Route B names an exact wheel and SHA-256 checksum from the official Python advisory; pip rejects substituted contents that do not match it. Obtain that command through the protected browser/trusted advisory. This provides a specific first-download check instead of assuming old pip is safe.

**What this fixes.** pip 26.2.2 replaces its included certifi. OS certificate edits alone do not remove the old copy inside pip; updating importable application certifi also does not update pip’s copy.

**Limits and follow-up.** The system/custom CA configuration still needs checking. Include Conda, .venv, tox/nox, user installs, isolated tools and notebook kernels. ensurepip/virtualenv can create another old pip later; use 08/15 or repeat the verified first update in that new environment. Do not add persistent sitecustomize/CA overrides as an assumed fix.

Sources: [Python’s incident advisory and first-update command](https://discuss.python.org/t/information-for-python-users-regarding-emudhra-emsign-ca-certificate-security-incident/109576), [pip’s upgrade instructions](https://pip.pypa.io/en/stable/installation/#upgrading-pip), [ensurepip documentation](https://docs.python.org/3/library/ensurepip.html), [pip release notes](https://pip.pypa.io/en/stable/news/), [pip HTTPS trust behavior](https://pip.pypa.io/en/stable/topics/https-certificates/), [pip checksum checks](https://pip.pypa.io/en/stable/topics/secure-installs/).

[Back to action 09](#09-pip-in-each-python-environment)

### A10. Python certificate packages in each environment

**Download safety and ordering.** The corrected pip/Conda download route is established first. This ordering avoids fetching the certificate fix through a still-unconfirmed installer. If the actual client instead uses a custom CA list, update/check that selected list separately.

**What this fixes.** The minimum certifi is 2026.10.19. Conda’s ca-certificates minimum is 2026.10.19 on conda-forge and 2026.10.20 on Anaconda main. These repair environment/application bundles separately from pip’s included bundle.

**Limits and follow-up.** Check active pins (14) and used CA overrides (23). Conda Python/platform availability limits from A06 still apply. Updating base does not update every environment, and changing a package not used by the connection cannot establish protection.

Sources: [Conda’s incident instructions](https://conda.org/blog/2026-10-21-emudhra-security-updates/), [release requirements and platform limits](https://github.com/conda/conda/issues/16880), [documented offline packages and dependency limitation](https://docs.conda.io/projects/conda/en/latest/user-guide/concepts/installing-with-conda.html), [install options](https://docs.conda.io/projects/conda/en/stable/commands/install.html), [Python’s incident advisory and first-update command](https://discuss.python.org/t/information-for-python-users-regarding-emudhra-emsign-ca-certificate-security-incident/109576), [pip’s upgrade instructions](https://pip.pypa.io/en/stable/installation/#upgrading-pip), [ensurepip documentation](https://docs.python.org/3/library/ensurepip.html), [Requests certificate configuration](https://requests.readthedocs.io/en/stable/user/advanced/), [HTTPX SSL defaults and overrides](https://www.python-httpx.org/advanced/ssl/), [Render’s application remediation/verification guidance](https://render.com/docs/advisory-emudhra).

[Back to action 10](#10-python-certificate-packages-in-each-environment)

### A11. Node.js, npm/npx, Yarn, pnpm and Corepack

**Download safety and ordering.** Package-manager routes depend on their earlier fixes. The official installer route uses the protected browser and normal publisher checks. An old nvm/fnm/other downloader is not automatically safe because Node’s new release is fixed: verify its connection/artifact checking or use the verified distribution supplied by IT.

**What this fixes.** The Node security releases fix its TLS checking. Package managers using its normal JavaScript networking do not need a separate npm version upgrade just to replace certificates.

**Limits and follow-up.** Unsupported/EOL Node lines need a maintained fix/migration. Check .nvmrc/.node-version, Volta, editor, CI and container copies. Plain Homebrew node can change major; do not assume it is node@24. Additional CA settings/custom HTTPS code need review. Keep Corepack/integrity/signature checks enabled.

#### pnpm 12 and independently packaged managers

pnpm 12 is a self-contained executable; updating system Node does not prove its own networking fixed. The documented command is `pnpm self-update`, but inside a project with a `packageManager` pin it can change the project pin rather than the global installation. No incident-specific fixed pnpm 12 minimum was established here. Confirm its own vendor build or actual client; do not force a pnpm major upgrade as an assumed certificate fix.

`command -v node` on macOS/Linux or `Get-Command node -All` in PowerShell helps identify installed copies. `volta pin` changes a project setting and should be reviewed as a project change.

Sources: [Node’s download page](https://nodejs.org/en/download), [Node security release](https://nodejs.org/en/blog/vulnerability/october-2026-security-releases), [nvm](https://github.com/nvm-sh/nvm), [nvm-windows](https://github.com/nvm-windows/nvm), [fnm](https://github.com/Schniz/fnm), [Volta install command](https://docs.volta.sh/reference/install), [npm and Node relationship](https://docs.npmjs.com/about-npm-versions/), [Corepack documentation](https://github.com/nodejs/node/blob/main/deps/corepack/README.md), [pnpm’s current installation architecture](https://pnpm.io/installation/).

[Back to action 11](#11-nodejs-npmnpx-yarn-pnpm-and-corepack)

### A12. Pipenv

**Download safety and ordering.** The update runs through an already protected pip or pipx backend. This dependency can be completed first even when pipx’s detailed section appears later.

**What this fixes.** Pipenv’s fixed release updates its own included pip and CA packages; those are separate from the managed project’s pip.

**Limits and follow-up.** A tool update does not update all project environments. Keep the original installation method.

Sources: [Pipenv installation methods](https://pipenv.pypa.io/en/latest/installation.html), [Pipenv vendored-pip/CA fix](https://pipenv.pypa.io/en/latest/changelog.html).

[Back to action 12](#12-pipenv)

### A13. Python tools installed in separate environments

**Download safety and ordering.** The backend doing the download must be corrected before network maintenance. --skip-maintenance lets the maintainer inspect without triggering pipx’s automatic shared-library update. Use the shared environment’s own Python with the hash-verified pip route when necessary.

**What this fixes.** These tools run in separate environments. Updating their used installer/runtime/CA packages repairs those copies; updating only the project misses them.

**Limits and follow-up.** A top-level upgrade can leave older dependencies that still satisfy requirements. pipx may delegate to uv; update that backend too. Standalone binaries require vendor update/build evidence. Newly created environments need fixed starters, while old project environments need their own updates.

#### Maintainer check: pipx’s shared environment

`pipx environment --skip-maintenance --value PIPX_SHARED_LIBS` prints its shared environment’s location. Use that environment’s Python with 09 if its pip needs the first-update fix. Older pipx may not support the same flags; check `pipx --help` and identify the environment directly. Do not assume upgrading an app upgrades all its already-installed dependencies.

Sources: [pipx CLI and shared-pip operations](https://pipx.pypa.io/stable/reference/cli.html), [pipx shared environment/backend configuration](https://pipx.pypa.io/stable/reference/environment-variables.html), [uv isolated-tool upgrades](https://docs.astral.sh/uv/concepts/tools/), [Poetry self commands and Windows caveat](https://python-poetry.org/docs/cli/), [PDM CLI](https://pdm-project.org/en/latest/reference/cli/), [Hatch installation variants](https://hatch.pypa.io/latest/install/), [tox installation](https://tox.wiki/en/latest/installation.html), [Nox documentation](https://nox.thea.codes/en/stable/), [Poetry’s own environment and self commands](https://python-poetry.org/docs/cli/#self).

[Back to action 13](#13-python-tools-installed-in-separate-environments)

### A14. Project version files and saved environments

**Download safety and ordering.** Regeneration downloads run through the corrected tool. New artifact checksums must come through that protected route, not be copied from an untrusted response.

**What this fixes.** Corrected definitions and saved environments prevent a later rebuild restoring the old program.

**Limits and follow-up.** Check requirements*.txt, pyproject.toml, locks, Conda specs, Dockerfiles, .nvmrc/.node-version, Volta/packageManager settings, hook runtime versions and workflows. Review the dependency diff. Historical locks need not be rewritten; label unsuitable online rebuilds and provide a corrected active definition. Old model/source files are not automatically malicious.

**Why recreation can work.** A replacement removes the old environment’s installed copies, provided its creator and saved requirements select fixed versions. An old creator can still supply old starter pip, and unchanged requirements can reinstall old certificate packages. Complete 09 before other package downloads and check 10 after setup. Python treats project virtual environments as disposable; the incident advisory also supports updating existing environments. Keep project files outside the environment and test the replacement before removing the old one.

Sources: [uv targeted lock upgrades](https://docs.astral.sh/uv/concepts/projects/sync/), [Poetry update command](https://python-poetry.org/docs/cli/#update), [pip-tools targeted upgrades](https://pip-tools.readthedocs.io/en/stable/), [GitHub cache management](https://docs.github.com/en/actions/how-tos/manage-workflow-runs/manage-caches), [Python virtual-environment lifecycle](https://docs.python.org/3/library/venv.html), [Python incident guidance for existing environments](https://discuss.python.org/t/information-for-python-users-regarding-emudhra-emsign-ca-certificate-security-incident/109576).

[Back to action 14](#14-project-version-files-and-saved-environments)

### A15. virtualenv: the tool that creates Python environments

**Download safety and ordering.** The package update uses corrected pip/the verified existing manager. It is not safe to repair the creator by ignoring a warning from the installer fetching it.

**What this fixes.** virtualenv 21.14.12 includes pip 26.2.2 as its starter. New environments can then begin with fixed pip.

**Limits and follow-up.** Updating pre-commit alone may leave older virtualenv installed. Older ensurepip wheels and other creators can still recreate old pip; check 07–09. Existing environments do not automatically change when the creator changes.

Sources: [virtualenv seed fixes](https://virtualenv.pypa.io/en/latest/changelog.html).

[Back to action 15](#15-virtualenv-the-tool-that-creates-python-environments)

### A16. pre-commit: recreate saved hook environments

**Download safety and ordering.** Cache deletion itself does not download code. Recreating hooks does: Git, the environment creator and runtime downloaders must be protected first. An old fixed-version setting can still force a vulnerable download, so correct it before rebuilding.

**What this fixes.** A fixed creator plus recreation replaces old hook installers/runtimes without changing the source versions of the checks. install-hooks installs environments without running the hooks.

**Limits and follow-up.** Do not use pre-commit autoupdate as cache repair; it changes hook revisions. A shell’s default Node/Go is not the private hook copy. Rebuild Go-based executables with the corrected compiler. In CI, remove only affected pre-commit caches after correcting their inputs.

Sources: [virtualenv seed fixes](https://virtualenv.pypa.io/en/latest/changelog.html), [pre-commit cache and install-hooks commands](https://pre-commit.com/), [Go’s security release history](https://go.dev/doc/devel/release), [GitHub CLI’s patched Go build](https://github.com/cli/cli/releases/tag/v2.102.1).

[Back to action 16](#16-pre-commit-recreate-saved-hook-environments)

### A17. Code editors and AI coding tools

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

[Back to action 17](#17-code-editors-and-ai-coding-tools)

### A18. Development and cloud command-line tools

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

[Back to action 18](#18-development-and-cloud-command-line-tools)

### A19. Docker Desktop and container images

**Download safety and ordering.** Desktop comes through its supported updater or verified vendor installer obtained with 02. Image build steps then download through the corrected OS/package/runtime clients inside the image; host protection alone does not establish their safety. Repair/verify the base and installers before online dependency downloads.

**What this fixes.** Desktop 4.96.3 covers the identified bundled fixes; earlier October hotfixes were partial. New images must contain corrected CA/runtime/application packages. --pull fetches the selected base; --no-cache reruns build steps.

**Limits and follow-up.** An old exact digest still selects an old image even with --pull. Existing containers and remote Linux Docker Engine/CLI/plugins are separate. Use their vendor updates as applicable. Do not delete all Docker data or unrelated caches.

Sources: [Docker Desktop incident releases](https://docs.docker.com/desktop/release-notes/), [macOS installation/update distribution](https://docs.docker.com/desktop/setup/install/mac-install/), [Docker build cache and pull guidance](https://docs.docker.com/build/building/best-practices/), [host/container trust-store separation](https://docs.docker.com/engine/network/ca-certs/), [Docker Desktop Windows installation](https://docs.docker.com/desktop/setup/install/windows-install/).

[Back to action 19](#19-docker-desktop-and-container-images)

### A20. WSL, remote machines, Azure ML and CI

**Download safety and ordering.** The host OS update route does not protect every guest or remote environment. Each repeats the prerequisite checks. Ubuntu APT verifies signed repository information and package checksums using its archive keys, independently of website CAs. For Amazon Linux retain the configured RPM signature checks; AL2023 enables package GPG checks by default. Do not bypass authentication to force a CA update. For CI/cloud images, establish trusted base artifacts and corrected installers before downloads.

**What this fixes.** These systems have independent installed tools and trust stores. Updating where the code executes puts the relevant fix into the actual environment.

**Limits and follow-up.** wsl --update does not update Linux packages. Runner auto-update does not maintain the machine’s tools. setup-node selects project Node, not every action’s included Node. For Node 24, node-version: 24 must actually resolve to ≥24.22.1; caches/mirrors can be stale. Preserve reviewed action tags/SHAs. Updating local Azure CLI does not mutate an old Azure ML environment. AL2023 repository metadata signature checks are separate from package checks and disabled by default: do not claim a successful command proves updates were not withheld. Release pins can retain old packages; verify the installed CA/runtime fix.

Sources: [WSL commands](https://learn.microsoft.com/en-us/windows/wsl/basic-commands), [Ubuntu’s incident package fix](https://ubuntu.com/blog/emudhra-ca-compromise-fixes-available), [AWS’s Amazon Linux CA fix](https://aws.amazon.com/security/security-bulletins/2026-150-aws/), [Azure ML environment management](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-manage-environments-v2?view=azureml-api-2), [GitHub action runtime metadata](https://docs.github.com/en/actions/reference/workflows-and-actions/metadata-syntax#runs-for-javascript-actions), [self-hosted runner responsibilities](https://docs.github.com/en/actions/reference/runners/self-hosted-runners), [targeted GitHub cache deletion](https://docs.github.com/en/actions/how-tos/manage-workflow-runs/manage-caches), [setup-node’s documented version inputs](https://github.com/actions/setup-node), [Ubuntu APT signature/checksum verification](https://manpages.ubuntu.com/manpages/noble/man8/apt-secure.8.html), [Amazon Linux package vs repository verification](https://docs.aws.amazon.com/linux/al2023/ug/repo-metadata-signing.html).

[Back to action 20](#20-wsl-remote-machines-azure-ml-and-ci)

### A21. Electron apps your team develops

**Download safety and ordering.** The dependency download runs through the already protected package manager/Node, with normal artifact-integrity checks. The distributed app then follows the team’s normal signing/release process.

**What this fixes.** Electron’s advisory names these fixed branches. Rebuilding includes the corrected verifier in the app that users run.

**Limits and follow-up.** Updating only the build computer does not change old distributed apps. Extra included downloaders/runtimes need their own assessment. Documented vendor backports are also acceptable.

Sources: [Electron security advisory](https://github.com/electron/electron/security/advisories/GHSA-jr5w-w6x2-84p9), [npm install syntax](https://docs.npmjs.com/cli/v11/commands/npm-install/).

[Back to action 21](#21-electron-apps-your-team-develops)

### A22. Python publishing and signing tools

**Download safety and ordering.** Use corrected pip/the original package manager for the Python tool. Retrieve/review the action reference through the protected source/CI route. Do not remove the team’s immutable-SHA or signing requirements just to copy the example.

**What this fixes.** Publishing has separate network/signing clients. The specific Sigstore/action versions update those distributions; the build environment’s pip alone does not update them.

**Limits and follow-up.** Twine’s actual environment still needs checking. Do not replace your signing keys or release credentials solely because a tool is listed; investigate real compromise evidence separately.

Sources: [Python advisory’s publishing-tool requirements](https://discuss.python.org/t/information-for-python-users-regarding-emudhra-emsign-ca-certificate-security-incident/109576), [Sigstore 4.5.1](https://pypi.org/project/sigstore/4.5.1/), [PyPA publishing action v1.14.3](https://github.com/pypa/gh-action-pypi-publish/releases/tag/v1.14.3).

[Back to action 22](#22-python-publishing-and-signing-tools)

### A23. Python apps that make HTTPS connections

**Download safety and ordering.** The certifi update is downloaded through the already corrected installer. Restart/redeploy does not establish network safety by itself; the running client must select the corrected verifier/list.

**What this fixes.** Application certifi is separate from pip’s included list. Requests/HTTPX document their selected CA sources, so correcting the list actually used addresses application traffic.

**Limits and follow-up.** Check REQUESTS_CA_BUNDLE, CURL_CA_BUNDLE, SSL_CERT_FILE, SSL_CERT_DIR, verify=, SSL contexts and app-supplied files. IT should manage corporate extra roots. aiohttp, gRPC and native helpers can differ; updating an unused certifi does not certify them.

Sources: [Requests certificate configuration](https://requests.readthedocs.io/en/stable/user/advanced/), [HTTPX SSL defaults and overrides](https://www.python-httpx.org/advanced/ssl/), [Render’s application remediation/verification guidance](https://render.com/docs/advisory-emudhra).

[Back to action 23](#23-python-apps-that-make-https-connections)

### A24. Hugging Face and extra model downloaders

**Download safety and ordering.** Python package updates use the corrected installer. Native model downloaders may use another TLS implementation; establish their protection before asking them to download models or fixes.

**What this fixes.** Hugging Face documents hf_xet as a separate Rust downloader. That establishes an additional component to assess, not that it is vulnerable.

**Limits and follow-up.** Do not blindly upgrade the whole ML stack or redownload every model. Cached model weights do not mean every later metadata/tokenizer/runtime request is offline.

Sources: [Hugging Face’s Xet client documentation](https://github.com/huggingface/hub-docs/blob/main/docs/hub/xet/using-xet-storage.md), [Hub installation instructions](https://huggingface.co/docs/huggingface_hub/installation).

[Back to action 24](#24-hugging-face-and-extra-model-downloaders)

### A25. LLM API libraries, agent frameworks and MCP servers

**Download safety and ordering.** Updates run through the corrected installer/backend. Protecting a parent editor does not authenticate downloads made by an independent MCP subprocess, SDK helper or proxy; check those actual routes too.

**What this fixes.** These HTTPS connections carry API keys, prompts and responses. The selected Python/Node trust fix protects default clients; AWS and LiteLLM have additional specific vendor fixes.

**Limits and follow-up.** Framework names do not identify every transport. Check custom proxies, explicit CA lists and standalone MCP environments. A compatible trust fix does not automatically require updating every framework. Updating botocore does not update AWS CLI v2.

Sources: [HTTPX transport/trust configuration](https://www.python-httpx.org/advanced/ssl/), [AWS SDK bulletin](https://aws.amazon.com/security/security-bulletins/2026-150-aws/), [LiteLLM’s vendor fix and branch backports](https://github.com/BerriAI/litellm/releases/tag/v1.106.3).

[Back to action 25](#25-llm-api-libraries-agent-frameworks-and-mcp-servers)

### A26. Ollama, LM Studio and other local LLM apps

**Download safety and ordering.** An old app updater is not automatically safe because the app is listed here. Use a confirmed protected updater or verified official distribution through 02/IT. Model/runtime downloads can use separate components.

**What this fixes.** The vendor’s normal app update replaces its included components. Local inference with existing files and no network connection does not expose that operation to this HTTPS attack.

**Limits and follow-up.** The next model search/download, runtime update or API request is a separate network operation. Being listed establishes scope to check, not confirmed vulnerability or a confirmed fix.

Sources: [Ollama’s update FAQ](https://docs.ollama.com/faq), [LM Studio’s offline/network and runtime behavior](https://lmstudio.ai/docs/app/offline).

[Back to action 26](#26-ollama-lm-studio-and-other-local-llm-apps)

### A27. Inference, training, evaluation and notebooks

**Download safety and ordering.** The same verified installer/client prerequisites apply on the GPU/cloud/container system. A local successful update or model benchmark does not authenticate a remote environment’s downloads.

**What this fixes.** Correcting the component making HTTPS connections addresses this incident without unrelated GPU/framework changes.

**Limits and follow-up.** A kernel may use another Python from the notebook server. llama.cpp/native downloaders may not use Python certifi. Confirm the actual linked client/vendor build; model accuracy/performance tests are not certificate tests.

Sources: [application/container remediation guidance](https://render.com/docs/advisory-emudhra), [Hugging Face native-download architecture](https://github.com/huggingface/hub-docs/blob/main/docs/hub/xet/using-xet-storage.md), [Docker host/container trust separation](https://docs.docker.com/engine/network/ca-certs/).

[Back to action 27](#27-inference-training-evaluation-and-notebooks)

### A28. Running services, workers and scheduled jobs

**Download safety and ordering.** The build must obtain dependencies through corrected, verified routes. Deployment then selects that reviewed artifact through the organisation’s normal authenticated release process; a fresh deployment of an old image is not remediation.

**What this fixes.** Restart/redeployment puts the corrected packages into the process making real connections. Testing there checks the actual runtime/list in use.

**Limits and follow-up.** A new build in a registry does not prove rollout. curl in a service shell can use another certificate list from HTTPX/Requests/native clients. There is no universal safe redeploy command for every platform.

Sources: [Render’s deployment and verification advisory](https://render.com/docs/advisory-emudhra).

[Back to action 28](#28-running-services-workers-and-scheduled-jobs)

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
