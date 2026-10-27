# eMudhra certificate incident: developer update guide

**Guidance checked: 26 October 2026.** For macOS and Windows, including Python/JavaScript projects, LLM tools, remote systems and containers.

## Part 1 — What to do

> [!IMPORTANT]
> You will not have all these tools. Only follow items for software you already use. Skip the rest. Do not install a program just because it appears in this guide.

Read the operating-system and browser guidance, then find your tools in the list below and work through those items. Project/CI/server items are for whoever maintains those systems. If you do not recognise a tool or know how it was installed, ask the person supporting that setup rather than trying every command.

### Order of work

Each row is a separate item. Sections 01–27 have matching **A-numbers in Part 2**, explaining the safety of the update method, its place in the order, and what it fixes. Section 28 lists other tools without update instructions.

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
| [12](#12-project-version-files-and-saved-environments) | Project version files and saved environments | You maintain project requirements, lockfiles, runtime settings or CI caches. |
| [13](#13-virtualenv-the-tool-that-creates-python-environments) | virtualenv: the tool that creates Python environments | People who use pre-commit, tox or nox (which normally use virtualenv), or virtualenv directly. |
| [14](#14-pre-commit-recreate-saved-hook-environments) | pre-commit: recreate saved hook environments | You use pre-commit in a project or CI. |
| [15](#15-code-editors-and-ai-coding-tools) | Code editors and AI coding tools | You use one of the listed apps. Update only yours. |
| [16](#16-development-and-cloud-command-line-tools) | Development and cloud command-line tools | You use gh, Git LFS, Azure CLI, AWS CLI, gcloud, or separately installed curl/OpenSSL tools. |
| [17](#17-docker-desktop-and-container-images) | Docker Desktop and container images | You use Docker or development containers. |
| [18](#18-wsl-remote-machines-azure-ml-and-ci) | WSL, remote machines, Azure ML and CI | Your code runs in these places, or you maintain their environments. |
| [19](#19-electron-apps-your-team-develops) | Electron apps your team develops | You build and distribute an Electron app; otherwise skip. |
| [20](#20-python-publishing-and-signing-tools) | Python publishing and signing tools | You publish packages with Twine, Sigstore or PyPA’s publishing Action. |
| [21](#21-python-apps-that-make-https-connections) | Python apps that make HTTPS connections | Your programs use Requests, HTTPX, SDKs or other HTTP libraries. |
| [22](#22-hugging-face-and-extra-model-downloaders) | Hugging Face and extra model downloaders | You use huggingface_hub, hf_xet, Transformers or Datasets downloads. |
| [23](#23-llm-api-libraries-agent-frameworks-and-mcp-servers) | LLM API libraries, agent frameworks and MCP servers | You use provider SDKs, LangChain, LlamaIndex, LiteLLM or MCP services. |
| [24](#24-ollama-lm-studio-and-other-local-llm-apps) | Ollama, LM Studio and other local LLM apps | You use one of these local model apps. |
| [25](#25-inference-training-evaluation-and-notebooks) | Inference, training, evaluation and notebooks | You run vLLM, SGLang, Transformers, llama.cpp, notebooks or cloud/GPU jobs. |
| [26](#26-running-services-workers-and-scheduled-jobs) | Running services, workers and scheduled jobs | You maintain deployed apps, background workers, agents or scheduled jobs. |
| [27](#27-python-tools-installed-in-separate-environments) | Python tools installed in separate environments | You use uv tools/uvx, Poetry, PDM, Hatch, pip-tools, tox or nox in an environment separate from your projects. |
| [28](#28-other-tools) | Other tools | You use one of the additional tools listed. |

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

> [!IMPORTANT]
> **These instructions do not apply to Python bundled with macOS or Xcode Command Line Tools.** Apple’s certificate fix for those command-line tools is still awaiting a macOS update. See [A03’s explanation and workaround guidance](#a03-macos-command-line-certificates).

**Applies to:** People who use Python installed through Conda, uv, Homebrew or python.org.

**Before:** Complete the relevant update first: [Conda 06](#06-conda-miniconda-anaconda-and-miniforge), [Homebrew 04](#04-homebrew), or [uv 07](#07-uv).

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
| Homebrew — example for existing Python 3.12 | `brew upgrade python@3.12` |
| python.org — Python 3.14 or 3.15 | Run the official [3.14.10 installer](https://www.python.org/downloads/release/python-31410/) or [3.15.2 installer](https://www.python.org/downloads/release/python-3152/), matching your existing release line. |

**Existing uv environments:** Those tied to an exact older Python version may keep using it and need a separate environment update. Project version settings and lockfiles are covered in [12](#12-project-version-files-and-saved-environments).

Python 3.10 or older no longer receives upstream fixes. Get help choosing a supported build if your existing supplier cannot provide the fix; do not change release lines just to copy an example.

[Update evidence and download protection → A08](#a08-python-installations)

### 09. pip in each Python environment

> [!IMPORTANT]
> **These instructions do not apply to pip bundled with Apple/Xcode’s Python.** Apple’s macOS certificate update for command-line tools is still pending; its notice does not confirm a fix for bundled pip. See [A09’s explanation](#apples-bundled-pip).

**Applies to:** People who use pip in separately managed Python installations or environments. **Repeat in each environment you use.**

**Before:** Activate the environment and complete its Python update in [08](#08-python-installations). If that environment’s Python has not received the fix, or you are unsure, use [A09’s alternative update](#pip-update-before-the-python-fix) instead.

Run:

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

Reopen the terminal and check `pnpm --version` if you updated pnpm. Project version pins are covered separately in [12](#12-project-version-files-and-saved-environments).

[Update evidence and download protection → A11](#a11-nodejs-npmnpx-yarn-pnpm-and-corepack)

### 12. Project version files and saved environments

**Applies to:** You maintain project requirements, lockfiles, runtime settings or CI caches.

**Before:** Fix the installer/runtime that will regenerate the files first. If the project tool has its own environment, complete [27](#27-python-tools-installed-in-separate-environments) for that tool before using it here.

1. Correct active requirements that would reinstall an affected version. For Python, update any old version pins in `.python-version`, `pyproject.toml` or environment definitions to allow a [fixed version from 08](#08-python-installations). Refresh affected lockfiles with the project’s usual tool.
2. Use the existing project tool to update affected packages, not every dependency:

| Project tool | Existing certifi example |
| --- | --- |
| uv | `uv lock --upgrade-package certifi`, then `uv sync --locked` |
| Poetry | `poetry update certifi` |
| pip-tools | `pip-compile --upgrade-package certifi`, with the project’s normal file arguments; then its normal sync |
| Conda | Update the existing environment definition and regenerate it through the project’s normal workflow. |

3. Refresh affected tool/image caches after fixing their source settings. Keep unrelated data and historical experiment records.

[Why this update method is safe, and what it covers → A12](#a12-project-version-files-and-saved-environments)

### 13. virtualenv: the tool that creates Python environments

**Applies to:** People who use pre-commit, tox or nox (which normally use virtualenv), or virtualenv directly.

**Before:** Update your package manager, for example [09](#09-pip-in-each-python-environment) for pip or [06](#06-conda-miniconda-anaconda-and-miniforge) for Conda.

**Repeat for each environment where pre-commit, tox, nox or virtualenv is installed.** Activate that environment, then use your existing installer to update virtualenv to **21.14.12 or later**. For example, with pip:

```bash
python -m pip install --upgrade "virtualenv>=21.14.12"
```

[Update evidence and download protection → A13](#a13-virtualenv-the-tool-that-creates-python-environments)

### 14. pre-commit: recreate saved hook environments

**Applies to:** People who use pre-commit in a project or CI. Skip this section if you do not.

**Before:** Complete the applicable Git steps in [03](#03-macos-command-line-certificates)/[05](#05-git-for-windows), and update pre-commit’s own Python and virtualenv using [08](#08-python-installations) and [13](#13-virtualenv-the-tool-that-creates-python-environments). Correct old runtime settings using [12](#12-project-version-files-and-saved-environments).

**1. Clear pre-commit’s shared cache once.** This removes saved hook environments for all projects using that cache:

```bash
pre-commit clean
```

**2. In each project you use that has `.pre-commit-config.yaml`, rebuild its hook environments:**

```bash
pre-commit install-hooks
```

Keep the existing hook revisions. If you use pre-commit in CI, refresh its saved cache too; see [18](#18-wsl-remote-machines-azure-ml-and-ci).

[Rebuild evidence and version checks → A14](#a14-pre-commit-recreate-saved-hook-environments)

### 15. Code editors and AI coding tools

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

**After updating:** check separately downloaded runtimes, extensions/MCP servers and remote copies. **For tools without a confirmed fixed version, leave the certificate check pending; updating the app alone is not proof.** See the per-tool table in A15.

[Why this update method is safe, and what it covers → A15](#a15-code-editors-and-ai-coding-tools)

### 16. Development and cloud command-line tools

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

[Why this update method is safe, and what it covers → A16](#a16-development-and-cloud-command-line-tools)

### 17. Docker Desktop and container images

**Applies to:** You use Docker or development containers.

**Before:** Use the normal Desktop updater/verified official installer. Correct image dependency definitions before rebuilding.

1. Update Docker Desktop to **4.96.3 or a later fixed release** with its updater or official [macOS](https://docs.docker.com/desktop/setup/install/mac-install/) / [Windows](https://docs.docker.com/desktop/setup/install/windows-install/) installer.
2. Update affected certificates/runtimes/packages in the Dockerfile/base image and project locks. Change an old pinned image digest to a corrected image.
3. Rebuild through the project workflow; example:

```bash
docker build --pull --no-cache -t TEAM_IMAGE:PATCHED .
```

4. Recreate containers and deploy the new image. Updating Desktop does not repair an old image or running container.

[Why this update method is safe, and what it covers → A17](#a17-docker-desktop-and-container-images)

### 18. WSL, remote machines, Azure ML and CI

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

[Why this update method is safe, and what it covers → A18](#a18-wsl-remote-machines-azure-ml-and-ci)

### 19. Electron apps your team develops

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

[Why this update method is safe, and what it covers → A19](#a19-electron-apps-your-team-develops)

### 20. Python publishing and signing tools

**Applies to:** You publish packages with Twine, Sigstore or PyPA’s publishing Action.

**Before:** Protect the environment doing the publishing using 08–10; CI downloads follow 18.

| Tool | Do this |
| --- | --- |
| Python Sigstore | In its pip-installed environment: `python -m pip install --upgrade "sigstore>=4.5.1"`; otherwise use its original installer. |
| Twine / other publisher | Update its used Python/pip/certifi; no universal fixed Twine version was established. |
| PyPA publishing Action | Use **v1.14.3 or a later fixed release**, or the verified full commit SHA required by team policy. |

```yaml
- uses: pypa/gh-action-pypi-publish@v1.14.3
```

[Why this update method is safe, and what it covers → A20](#a20-python-publishing-and-signing-tools)

### 21. Python apps that make HTTPS connections

**Applies to:** Your programs use Requests, HTTPX, SDKs or other HTTP libraries.

**Before:** Complete 10 for the application’s environment, and check any custom certificate settings.

1. Ensure the **certifi actually used** is **≥2026.10.19**, where that is the selected list.
2. Have the maintainer check custom certificate files/settings.
3. Restart/redeploy, then test the application’s own connection library—not just curl.

[Why this update method is safe, and what it covers → A21](#a21-python-apps-that-make-https-connections)

### 22. Hugging Face and extra model downloaders

**Applies to:** You use huggingface_hub, hf_xet, Transformers or Datasets downloads.

**Before:** Protect Python downloads using 09–10. A separate native downloader needs its own evidence.

```bash
python -m pip show huggingface-hub hf-xet transformers datasets
```

Apply 21 to Python connections. Check the vendor’s fix for extra downloaders such as **hf_xet**, then update through the project’s normal protected workflow if needed. **No fixed/vulnerable hf_xet version was confirmed here: leave that check pending rather than assuming certifi repaired it.**

[Why this update method is safe, and what it covers → A22](#a22-hugging-face-and-extra-model-downloaders)

### 23. LLM API libraries, agent frameworks and MCP servers

**Applies to:** You use provider SDKs, LangChain, LlamaIndex, LiteLLM or MCP services.

**Before:** Protect the actual environment/library making each connection using the earlier relevant steps.

| Used component | Do this |
| --- | --- |
| Python library using certifi | Follow 21 in that environment. |
| JavaScript library/MCP server using normal Node HTTPS | Follow 11 for the Node actually running it. |
| Separate downloader / included runtime | Require its own vendor fix or connection verification. |
| boto3 / Bedrock | Use **botocore ≥1.43.117** with compatible boto3, through the existing project workflow. |
| LiteLLM | Fixed corresponding lines: **1.103.8, 1.104.6, 1.105.4, 1.106.3**, or later vendor-confirmed fixed releases. Update library/proxy/container copies separately. |

Existing pip-installed LiteLLM 1.106 example: `python -m pip install "litellm==1.106.3"`. Use the project lock workflow where applicable; do not downgrade a later fixed release or move every project to this line.

[Why this update method is safe, and what it covers → A23](#a23-llm-api-libraries-agent-frameworks-and-mcp-servers)

### 24. Ollama, LM Studio and other local LLM apps

**Applies to:** You use one of these local model apps.

**Before:** Use the normal vendor updater/verified official distribution, with its download checks intact.

| App | Do this |
| --- | --- |
| Ollama on macOS/Windows | Accept its automatic desktop update and choose **Restart to update**, or use the current official installer. |
| LM Studio | Update the app normally; also check separately downloaded model-running runtimes in its runtime tools. |
| Similar app | Check its own model/runtime downloader and API connections. |

**No incident-fixed minimum for these apps was confirmed here.** Obtain vendor confirmation/test results before marking the relevant downloader fixed.

[Why this update method is safe, and what it covers → A24](#a24-ollama-lm-studio-and-other-local-llm-apps)

### 25. Inference, training, evaluation and notebooks

**Applies to:** You run vLLM, SGLang, Transformers, llama.cpp, notebooks or cloud/GPU jobs.

**Before:** Apply earlier fixes in the environment doing downloads/API calls, not only the laptop submitting the job.

1. Update its used certificates, runtime, API libraries and extra downloaders.
2. Rebuild images/environments and use them in fresh jobs (17–18).
3. Check the notebook kernel’s Python/packages and restart it.
4. Verify its actual network client before new downloads/API traffic.

**Do not automatically upgrade CUDA, GPU drivers, model weights or every ML framework for this incident.**

[Why this update method is safe, and what it covers → A25](#a25-inference-training-evaluation-and-notebooks)

### 26. Running services, workers and scheduled jobs

**Applies to:** You maintain deployed apps, background workers, agents or scheduled jobs.

**Before:** Correct their definitions, packages and images with the relevant earlier steps.

1. Build the updated environment/image using the normal release process.
2. Deploy it and restart affected web services, workers, scheduled jobs, notebook servers and agent/model processes.
3. Confirm the **running** program uses the new version/image.
4. Test its own HTTPS library against normal and affected-certificate sites. Do not send real credentials or data to test sites.

[Why this update method is safe, and what it covers → A26](#a26-running-services-workers-and-scheduled-jobs)

### 27. Python tools installed in separate environments

**Applies to:** People who use Python tools such as Poetry, PDM, Hatch, pip-tools, tox or nox, or tools installed or run through uv. Follow the matching case below for each tool and environment you use.

A tool can have its **own environment: its own Python and packages**, separate from your project. Those copies need the certificate fixes too.

**If you installed tools using `uv tool install` — update all of them**

uv creates a separate environment for each tool installed this way. For example, `uv tool install poetry` creates one for Poetry. You can see your installed tools with `uv tool list`.

Complete [07](#07-uv) first, then **run this once to update every installed uv tool and its packages**:

```bash
uv tool upgrade --all
```

This covers the tools listed by `uv tool list`. Their Python versions also need [08](#08-python-installations); this command does not guarantee a Python update. If you only use uv for project environments, continue with the project’s environment updates instead.

**If you installed tools into an environment using pip or Conda**

Apply [08](#08-python-installations), [09](#09-pip-in-each-python-environment) and [10](#10-python-certificate-packages-in-each-environment) in **each environment containing those tools**. If you already did that, you do not need to repeat it. Where the tool uses virtualenv, also follow [13](#13-virtualenv-the-tool-that-creates-python-environments).

**If your commands start with `uvx`**

uvx can create temporary tool environments that the command above does not update. Complete [07](#07-uv), then follow [A27’s uvx instructions](#tools-you-run-with-uvx) before using those tools again.

**If you installed Poetry using its own installer**

Poetry has its own environment. Apply [08](#08-python-installations)–[10](#10-python-certificate-packages-in-each-environment) there first, then run `poetry self update`. On Windows, rerun the [official PowerShell installer](https://python-poetry.org/docs/#installation) instead, using Windows protection from [01](#01-operating-system-certificate-protection) and a fixed Python from 08 to run it. If you cannot locate Poetry’s own environment, ask its maintainer to identify it.

If an update fails or an old version setting prevents the fix, resolve it before using the tool online. [A27](#a27-python-tools-installed-in-separate-environments) lists the required versions and checks.

[Update evidence and download protection → A27](#a27-python-tools-installed-in-separate-environments)

### Final Checklist

- [ ] Relevant tools updated; unused tools marked **not applicable**.
- [ ] The actual program/environment was checked, not a different installed copy.
- [ ] Current project files/caches will not bring the old version back.
- [ ] Running apps/jobs were restarted or redeployed where needed.
- [ ] Relevant application/service connection tests pass; unconfirmed vendor/runtime checks remain **pending confirmation**.

### 28. Other tools

I wasn’t sure whether anyone on the team uses these tools, so I haven’t included update instructions. They also need updating; let me know if you’d like me to add instructions.

- [Pipenv](https://pipenv.pypa.io/en/latest/changelog.html#pipenv-2026-10-2-2026-10-21)
- [pipx](https://pipx.pypa.io/stable/explanation/how-pipx-works.html)
- [Mamba](https://conda.org/blog/2026-10-21-emudhra-security-updates/#mamba)
- [Conda’s libmamba solver](https://conda.org/blog/2026-10-21-emudhra-security-updates/#conda-libmamba-solver-and-conda-pypi)
- [Micromamba](https://mamba.readthedocs.io/en/latest/installation/micromamba-installation.html)
- [Bun](https://github.com/oven-sh/bun/security/advisories/GHSA-26q8-h657-pf84)
- [Deno](https://github.com/denoland/deno/releases/tag/v2.9.8)
- [Pixi](https://github.com/prefix-dev/pixi/releases/tag/v0.82.1)

## Part 2 — Why each update method is safe, and what it covers

**A01–A27 correspond to sections 01–27 in Part 1.** Each entry separates two questions: whether the first update can be obtained safely, and whether the resulting update fixes the relevant component. “Documented update command” and “confirmed incident fix” are not the same thing.

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

**Checked: 27 October 2026.**

| Claim | Sources | Quotes |
| --- | --- | --- |
| **Python fix:** the five minimum versions in 08 block the compromised keys in Python’s `ssl` module, including normal pip connections, even with an older certificate bundle. | [Python’s incident advisory](https://discuss.python.org/t/information-for-python-users-regarding-emudhra-emsign-ca-certificate-security-incident/109576)<br><br>[Fixed-release announcement](https://discuss.python.org/t/python-3-15-2-3-14-10-3-13-18-3-12-16-and-3-11-18-are-out/109678) | **Advisory:** “regardless of whether the emSign root CA certificate is in use by ‘certifi’”.<br><br>**Release:** “These keys are now blocked by the `ssl` module”. |
| **Existing uv environments:** the shared Python upgrade alone does not move environments tied to an exact old patch or installation path. This applies to security patches too. | [Astral’s upgrade limitations](https://docs.astral.sh/uv/concepts/python-versions/#upgrading-python-versions) | “an explicitly requested patch version”; “it will not be transparently upgraded to a new version”. |
| **Older versions:** python.org’s 3.11–3.13 fixes are source-only; use a supplier’s fixed build. Python 3.10 and older no longer receive upstream security fixes. | [Python’s release announcement and maintainer’s explanation](https://discuss.python.org/t/python-3-15-2-3-14-10-3-13-18-3-12-16-and-3-11-18-are-out/109678)<br><br>[Python’s incident advisory](https://discuss.python.org/t/information-for-python-users-regarding-emudhra-emsign-ca-certificate-security-incident/109576) | **3.11–3.13:** “source-only security releases”.<br><br>**Older:** “Python versions 3.10 and older are end-of-life”. |

**Existing uv environments:** Environments that follow a Python release line normally pick up the updated patch automatically. Those tied to an exact older patch or installation path can keep using the old copy; they also need to be moved to a fixed Python. Project version settings and lockfiles are covered separately in [12](#12-project-version-files-and-saved-environments). Changing those files alone does not update an existing environment.

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

Saved requirements are covered in [12](#12-project-version-files-and-saved-environments). Custom certificate files need their own repair in [21](#21-python-apps-that-make-https-connections).

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

### A12. Project version files and saved environments

**Download safety and ordering.** Regeneration downloads run through the corrected tool. New artifact checksums must come through that protected route, not be copied from an untrusted response.

**What this fixes.** Corrected definitions and saved environments prevent a later rebuild restoring the old program.

**Limits and follow-up.** Check requirements*.txt, .python-version, pyproject.toml, locks, Conda specs, Dockerfiles, .nvmrc/.node-version, Volta/packageManager settings, hook runtime versions and workflows. Review the dependency diff. Historical locks need not be rewritten; label unsuitable online rebuilds and provide a corrected active definition. Old model/source files are not automatically malicious.

**Why recreation can work.** A replacement removes the old environment’s installed copies, provided its creator and saved requirements select fixed versions. An old creator can still supply old starter pip, and unchanged requirements can reinstall old certificate packages. Complete 09 before other package downloads and check 10 after setup. Python treats project virtual environments as disposable; the incident advisory also supports updating existing environments. Keep project files outside the environment and test the replacement before removing the old one.

Sources: [uv Python version settings](https://docs.astral.sh/uv/concepts/python-versions/#project-python-versions), [uv targeted lock upgrades](https://docs.astral.sh/uv/concepts/projects/sync/), [Poetry update command](https://python-poetry.org/docs/cli/#update), [pip-tools targeted upgrades](https://pip-tools.readthedocs.io/en/stable/), [GitHub cache management](https://docs.github.com/en/actions/how-tos/manage-workflow-runs/manage-caches), [Python virtual-environment lifecycle](https://docs.python.org/3/library/venv.html), [Python incident guidance for existing environments](https://discuss.python.org/t/information-for-python-users-regarding-emudhra-emsign-ca-certificate-security-incident/109576).

[Back to action 12](#12-project-version-files-and-saved-environments)

### A13. virtualenv: the tool that creates Python environments

**Checked: 27 October 2026.**

| Claim | Sources | Quotes |
| --- | --- | --- |
| **Fix:** Python’s incident guidance requires virtualenv 21.14.12 or later. That release bundles fixed pip 26.2.2 for creating new environments. | [Python’s incident guidance](https://discuss.python.org/t/information-for-python-users-regarding-emudhra-emsign-ca-certificate-security-incident/109576)<br><br>[virtualenv 21.14.12 release notes](https://virtualenv.pypa.io/en/latest/changelog.html#v21-14-12-2026-10-21) | **Incident minimum:** “virtualenv: 21.14.12”.<br><br>**Release:** “the embedded pip wheel from `26.2.1` to `26.2.2`”. |
| **Command:** the pip example adapts virtualenv’s documented installation command: we require the incident-fixed version, omit `--user` to update the active environment and add `--upgrade`. | [virtualenv’s installation instructions](https://virtualenv.pypa.io/en/latest/how-to/install.html)<br><br>[pip’s upgrade option](https://pip.pypa.io/en/stable/cli/pip_install/#cmdoption-U) | **virtualenv:** `python -m pip install --user virtualenv`.<br><br>**pip:** “Upgrade all specified packages to the newest available version.” |
| **Download protection:** repair the installer before it downloads virtualenv. Python’s security guidance supports using corrected pip to obtain further fixes. For Conda, complete 06 first; its initial-download verification limit remains in A06. | [Python security guidance](https://discuss.python.org/t/information-for-python-users-regarding-emudhra-emsign-ca-certificate-security-incident/109576)<br><br>[pip repair evidence → A09](#a09-pip-in-each-python-environment)<br><br>[Conda’s incident instructions](https://conda.org/blog/2026-10-21-emudhra-security-updates/#conda-2694) · [Conda evidence and limit → A06](#a06-conda-miniconda-anaconda-and-miniforge) | **Python security guidance:** “From this secure installation you can bootstrap into a secure installation of certifi, uv, or other dependencies.”<br><br>**Conda’s prerequisite command:** `conda install --name base conda=26.9.4`. |
| **Update the tool’s own copy:** pre-commit, tox and Nox normally use virtualenv installed alongside the tool. This is supported by our source inspection and Nox’s documentation; it is why 13 repeats the update in each tool’s environment. | [pre-commit’s Python environment creation](https://github.com/pre-commit/pre-commit/blob/v4.7.0/pre_commit/languages/python.py)<br><br>[tox’s dependency declaration](https://github.com/tox-dev/tox/blob/main/pyproject.toml)<br><br>[Nox’s environment backends](https://nox.thea.codes/en/stable/config.html#configuring-a-sessions-virtualenv) | **pre-commit:** `[sys.executable, '-mvirtualenv', envdir]`.<br><br>**tox dependency:** `"virtualenv>=21.1"`.<br><br>**Nox:** “The `virtualenv` backend runs from Nox’s interpreter”. |

**Existing environments:** this updates the tool that creates environments; it does not replace Python or pip in environments already created. Those still need [08](#08-python-installations)–[10](#10-python-certificate-packages-in-each-environment). For pre-commit’s saved environments, follow [14](#14-pre-commit-recreate-saved-hook-environments).

**Coverage:** new environments also need a fixed Python from 08. Settings that explicitly select an older pip must be corrected. If tox or Nox is configured to use another environment creator, update that creator instead. If your existing Conda channel cannot supply the required virtualenv version, resolve that with your maintainer rather than replacing its package with pip.

[Back to action 13](#13-virtualenv-the-tool-that-creates-python-environments)

### A14. pre-commit: recreate saved hook environments

**Checked: 27 October 2026.**

| Claim | Sources | Quotes |
| --- | --- | --- |
| `pre-commit clean` followed by `pre-commit install-hooks` replaces cached Python hook environments with ones using corrected Python and pip, provided the updates and version selections in 08, 12 and 13 are complete. | [Documented clean command](https://pre-commit.com/#pre-commit-clean) · [Documented install-hooks command](https://pre-commit.com/#pre-commit-install-hooks)<br><br>[Python hook creation code](https://github.com/pre-commit/pre-commit/blob/v4.7.0/pre_commit/languages/python.py#L214-L228)<br><br>[virtualenv 21.14.12 release notes](https://virtualenv.pypa.io/en/latest/changelog.html#v21-14-12-2026-10-21)<br><br>[Python fix → A08](#a08-python-installations) · [Creator fix → A13](#a13-virtualenv-the-tool-that-creates-python-environments) | **Clean:** “Clean out cached pre-commit files.”<br><br>**Install:** “Install all missing environments for the available hooks.”<br><br>**Python creation:** `[sys.executable, '-mvirtualenv', envdir]`.<br><br>**virtualenv:** “the embedded pip wheel from `26.2.1` to `26.2.2`”. |
| The earlier Git and Python repairs are needed to protect downloads during rebuilding. Our source inspection shows Git fetches hook code and the new Python environment runs pip to install it; certificate verification must remain enabled. | [Git download code](https://github.com/pre-commit/pre-commit/blob/v4.7.0/pre_commit/store.py#L177-L193)<br><br>[Python hook installation code](https://github.com/pre-commit/pre-commit/blob/v4.7.0/pre_commit/languages/python.py#L214-L228)<br><br>[Git protection → A03](#a03-macos-command-line-certificates) / [A05](#a05-git-for-windows) · [Python protection → A08](#a08-python-installations) | **Git:** `git_cmd('fetch', 'origin', '--tags')`.<br><br>**Python installer:** `('python', '-mpip', 'install', '.', *additional_dependencies)`. |

**Version checks:** Check Python inside the recreated hook environments against [08](#08-python-installations), and pip against **26.2.2 or later**. Correct old `language_version` / `default_language_version` settings before rebuilding. These checks concern the hook’s environment, which can differ from your terminal’s default.

**Scope:** The evidence above covers the standard Python hook installation path. Hooks that use an existing system environment need that environment’s earlier updates. If you configure separate pre-commit cache locations, repeat the clean/rebuild for each one; CI caches are handled in 18.

[Back to action 14](#14-pre-commit-recreate-saved-hook-environments)

### A15. Code editors and AI coding tools

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

[Back to action 15](#15-code-editors-and-ai-coding-tools)

### A16. Development and cloud command-line tools

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

[Back to action 16](#16-development-and-cloud-command-line-tools)

### A17. Docker Desktop and container images

**Download safety and ordering.** Desktop comes through its supported updater or verified vendor installer obtained with 02. Image build steps then download through the corrected OS/package/runtime clients inside the image; host protection alone does not establish their safety. Repair/verify the base and installers before online dependency downloads.

**What this fixes.** Desktop 4.96.3 covers the identified bundled fixes; earlier October hotfixes were partial. New images must contain corrected CA/runtime/application packages. --pull fetches the selected base; --no-cache reruns build steps.

**Limits and follow-up.** An old exact digest still selects an old image even with --pull. Existing containers and remote Linux Docker Engine/CLI/plugins are separate. Use their vendor updates as applicable. Do not delete all Docker data or unrelated caches.

Sources: [Docker Desktop incident releases](https://docs.docker.com/desktop/release-notes/), [macOS installation/update distribution](https://docs.docker.com/desktop/setup/install/mac-install/), [Docker build cache and pull guidance](https://docs.docker.com/build/building/best-practices/), [host/container trust-store separation](https://docs.docker.com/engine/network/ca-certs/), [Docker Desktop Windows installation](https://docs.docker.com/desktop/setup/install/windows-install/).

[Back to action 17](#17-docker-desktop-and-container-images)

### A18. WSL, remote machines, Azure ML and CI

**Download safety and ordering.** The host OS update route does not protect every guest or remote environment. Each repeats the prerequisite checks. Ubuntu APT verifies signed repository information and package checksums using its archive keys, independently of website CAs. For Amazon Linux retain the configured RPM signature checks; AL2023 enables package GPG checks by default. Do not bypass authentication to force a CA update. For CI/cloud images, establish trusted base artifacts and corrected installers before downloads.

**What this fixes.** These systems have independent installed tools and trust stores. Updating where the code executes puts the relevant fix into the actual environment.

**Limits and follow-up.** wsl --update does not update Linux packages. Runner auto-update does not maintain the machine’s tools. setup-node selects project Node, not every action’s included Node. For Node 24, node-version: 24 must actually resolve to ≥24.22.1; caches/mirrors can be stale. Preserve reviewed action tags/SHAs. Updating local Azure CLI does not mutate an old Azure ML environment. AL2023 repository metadata signature checks are separate from package checks and disabled by default: do not claim a successful command proves updates were not withheld. Release pins can retain old packages; verify the installed CA/runtime fix.

Sources: [WSL commands](https://learn.microsoft.com/en-us/windows/wsl/basic-commands), [Ubuntu’s incident package fix](https://ubuntu.com/blog/emudhra-ca-compromise-fixes-available), [AWS’s Amazon Linux CA fix](https://aws.amazon.com/security/security-bulletins/2026-150-aws/), [Azure ML environment management](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-manage-environments-v2?view=azureml-api-2), [GitHub action runtime metadata](https://docs.github.com/en/actions/reference/workflows-and-actions/metadata-syntax#runs-for-javascript-actions), [self-hosted runner responsibilities](https://docs.github.com/en/actions/reference/runners/self-hosted-runners), [targeted GitHub cache deletion](https://docs.github.com/en/actions/how-tos/manage-workflow-runs/manage-caches), [setup-node’s documented version inputs](https://github.com/actions/setup-node), [Ubuntu APT signature/checksum verification](https://manpages.ubuntu.com/manpages/noble/man8/apt-secure.8.html), [Amazon Linux package vs repository verification](https://docs.aws.amazon.com/linux/al2023/ug/repo-metadata-signing.html).

[Back to action 18](#18-wsl-remote-machines-azure-ml-and-ci)

### A19. Electron apps your team develops

**Download safety and ordering.** The dependency download runs through the already protected package manager/Node, with normal artifact-integrity checks. The distributed app then follows the team’s normal signing/release process.

**What this fixes.** Electron’s advisory names these fixed branches. Rebuilding includes the corrected verifier in the app that users run.

**Limits and follow-up.** Updating only the build computer does not change old distributed apps. Extra included downloaders/runtimes need their own assessment. Documented vendor backports are also acceptable.

Sources: [Electron security advisory](https://github.com/electron/electron/security/advisories/GHSA-jr5w-w6x2-84p9), [npm install syntax](https://docs.npmjs.com/cli/v11/commands/npm-install/).

[Back to action 19](#19-electron-apps-your-team-develops)

### A20. Python publishing and signing tools

**Download safety and ordering.** Use corrected pip/the original package manager for the Python tool. Retrieve/review the action reference through the protected source/CI route. Do not remove the team’s immutable-SHA or signing requirements just to copy the example.

**What this fixes.** Publishing has separate network/signing clients. The specific Sigstore/action versions update those distributions; the build environment’s pip alone does not update them.

**Limits and follow-up.** Twine’s actual environment still needs checking. Do not replace your signing keys or release credentials solely because a tool is listed; investigate real compromise evidence separately.

Sources: [Python advisory’s publishing-tool requirements](https://discuss.python.org/t/information-for-python-users-regarding-emudhra-emsign-ca-certificate-security-incident/109576), [Sigstore 4.5.1](https://pypi.org/project/sigstore/4.5.1/), [PyPA publishing action v1.14.3](https://github.com/pypa/gh-action-pypi-publish/releases/tag/v1.14.3).

[Back to action 20](#20-python-publishing-and-signing-tools)

### A21. Python apps that make HTTPS connections

**Download safety and ordering.** The certifi update is downloaded through the already corrected installer. Restart/redeploy does not establish network safety by itself; the running client must select the corrected verifier/list.

**What this fixes.** Application certifi is separate from pip’s included list. Requests/HTTPX document their selected CA sources, so correcting the list actually used addresses application traffic.

**Limits and follow-up.** Check REQUESTS_CA_BUNDLE, CURL_CA_BUNDLE, SSL_CERT_FILE, SSL_CERT_DIR, verify=, SSL contexts and app-supplied files. IT should manage corporate extra roots. aiohttp, gRPC and native helpers can differ; updating an unused certifi does not certify them.

Sources: [Requests certificate configuration](https://requests.readthedocs.io/en/stable/user/advanced/), [HTTPX SSL defaults and overrides](https://www.python-httpx.org/advanced/ssl/), [Render’s application remediation/verification guidance](https://render.com/docs/advisory-emudhra).

[Back to action 21](#21-python-apps-that-make-https-connections)

### A22. Hugging Face and extra model downloaders

**Download safety and ordering.** Python package updates use the corrected installer. Native model downloaders may use another TLS implementation; establish their protection before asking them to download models or fixes.

**What this fixes.** Hugging Face documents hf_xet as a separate Rust downloader. That establishes an additional component to assess, not that it is vulnerable.

**Limits and follow-up.** Do not blindly upgrade the whole ML stack or redownload every model. Cached model weights do not mean every later metadata/tokenizer/runtime request is offline.

Sources: [Hugging Face’s Xet client documentation](https://github.com/huggingface/hub-docs/blob/main/docs/hub/xet/using-xet-storage.md), [Hub installation instructions](https://huggingface.co/docs/huggingface_hub/installation).

[Back to action 22](#22-hugging-face-and-extra-model-downloaders)

### A23. LLM API libraries, agent frameworks and MCP servers

**Download safety and ordering.** Updates run through the corrected installer/backend. Protecting a parent editor does not authenticate downloads made by an independent MCP subprocess, SDK helper or proxy; check those actual routes too.

**What this fixes.** These HTTPS connections carry API keys, prompts and responses. The selected Python/Node trust fix protects default clients; AWS and LiteLLM have additional specific vendor fixes.

**Limits and follow-up.** Framework names do not identify every transport. Check custom proxies, explicit CA lists and standalone MCP environments. A compatible trust fix does not automatically require updating every framework. Updating botocore does not update AWS CLI v2.

Sources: [HTTPX transport/trust configuration](https://www.python-httpx.org/advanced/ssl/), [AWS SDK bulletin](https://aws.amazon.com/security/security-bulletins/2026-150-aws/), [LiteLLM’s vendor fix and branch backports](https://github.com/BerriAI/litellm/releases/tag/v1.106.3).

[Back to action 23](#23-llm-api-libraries-agent-frameworks-and-mcp-servers)

### A24. Ollama, LM Studio and other local LLM apps

**Download safety and ordering.** An old app updater is not automatically safe because the app is listed here. Use a confirmed protected updater or verified official distribution through 02/IT. Model/runtime downloads can use separate components.

**What this fixes.** The vendor’s normal app update replaces its included components. Local inference with existing files and no network connection does not expose that operation to this HTTPS attack.

**Limits and follow-up.** The next model search/download, runtime update or API request is a separate network operation. Being listed establishes scope to check, not confirmed vulnerability or a confirmed fix.

Sources: [Ollama’s update FAQ](https://docs.ollama.com/faq), [LM Studio’s offline/network and runtime behavior](https://lmstudio.ai/docs/app/offline).

[Back to action 24](#24-ollama-lm-studio-and-other-local-llm-apps)

### A25. Inference, training, evaluation and notebooks

**Download safety and ordering.** The same verified installer/client prerequisites apply on the GPU/cloud/container system. A local successful update or model benchmark does not authenticate a remote environment’s downloads.

**What this fixes.** Correcting the component making HTTPS connections addresses this incident without unrelated GPU/framework changes.

**Limits and follow-up.** A kernel may use another Python from the notebook server. llama.cpp/native downloaders may not use Python certifi. Confirm the actual linked client/vendor build; model accuracy/performance tests are not certificate tests.

Sources: [application/container remediation guidance](https://render.com/docs/advisory-emudhra), [Hugging Face native-download architecture](https://github.com/huggingface/hub-docs/blob/main/docs/hub/xet/using-xet-storage.md), [Docker host/container trust separation](https://docs.docker.com/engine/network/ca-certs/).

[Back to action 25](#25-inference-training-evaluation-and-notebooks)

### A26. Running services, workers and scheduled jobs

**Download safety and ordering.** The build must obtain dependencies through corrected, verified routes. Deployment then selects that reviewed artifact through the organisation’s normal authenticated release process; a fresh deployment of an old image is not remediation.

**What this fixes.** Restart/redeployment puts the corrected packages into the process making real connections. Testing there checks the actual runtime/list in use.

**Limits and follow-up.** A new build in a registry does not prove rollout. curl in a service shell can use another certificate list from HTTPX/Requests/native clients. There is no universal safe redeploy command for every platform.

Sources: [Render’s deployment and verification advisory](https://render.com/docs/advisory-emudhra).

[Back to action 26](#26-running-services-workers-and-scheduled-jobs)

### A27. Python tools installed in separate environments

**Checked: 27 October 2026.** These commands combine Python’s incident guidance with the installers’ documented operations. The added package requirements are our adaptations, not tool-specific incident instructions.

| Claim | Sources | Quotes |
| --- | --- | --- |
| **Released fixes and order:** use pip 26.2.2+, certifi 2026.10.19+ and virtualenv 21.14.12+ wherever those components are used. Repair the downloading pip first using 09, including its hash-verified alternative when Python is not yet fixed. | [Python’s incident guidance](https://discuss.python.org/t/information-for-python-users-regarding-emudhra-emsign-ca-certificate-security-incident/109576)<br><br>[First pip update → A09](#pip-update-before-the-python-fix) | **Targets:** “certifi 2026.10.19”; “pip 26.2.2”; “virtualenv 21.14.12”.<br><br>**Order:** “use this secure pip installation to upgrade certifi and uv”. |
| **All installed uv tools:** `uv tool upgrade --all` is Astral’s command for updating every installed tool. Tool upgrades include their dependencies and use the uv repaired in 07. Existing version constraints still apply. | [Astral’s command for upgrading all tools](https://docs.astral.sh/uv/guides/tools/#upgrading-tools)<br><br>[What a tool upgrade includes](https://docs.astral.sh/uv/concepts/tools/#upgrading-tools)<br><br>[Astral’s incident advisory](https://github.com/astral-sh/uv/security/advisories/GHSA-hq85-5cpj-mr62) · [Current target and protection → A07](#a07-uv) | **All tools:** `uv tool upgrade --all`.<br><br>**Included packages:** “To upgrade all packages in a tool environment”.<br><br>**Incident advisory:** “uv 0.13.4 removes this root CA from its vendored CA bundle”. |
| **uvx needs its own selection.** Our `--with` requirement selects a fixed dependency for that invocation; `--isolated` avoids reusing an installed tool. Repeat the requirement on future invocations. | [Astral’s extra-dependency syntax](https://docs.astral.sh/uv/concepts/tools/#including-additional-dependencies)<br><br>[Astral’s installed-tool selection](https://docs.astral.sh/uv/concepts/tools/#tool-versions) | **Dependency syntax:** `uvx --with <extra-package> <tool>`.<br><br>**Isolation:** “ignore the installed version”. |
| **Poetry’s update route:** protect its own runtime before self-update. On Windows, the official installer uses PowerShell followed by Python; protect both through 01 and 08. This ordering is our application of the incident guidance. | [Poetry’s self-update command and Windows warning](https://python-poetry.org/docs/cli/#self)<br><br>[Official installer](https://python-poetry.org/docs/#installation)<br><br>[Windows protection → A01](#a01-operating-system-certificate-protection) · [Python protection → A08](#a08-python-installations) · [Certificate packages → A10](#a10-python-certificate-packages-in-each-environment) | **Update scope:** “the Poetry version and its dependencies in its current runtime environment”.<br><br>**Windows warning:** “`self` commands that update or remove packages may be problematic”.<br><br>**Installer command excerpts:** `Invoke-WebRequest`; `py -`. |

#### Tools you run with uvx

`uvx` can keep temporary tool environments in its cache; updating installed tools with `uv tool upgrade --all` does not update those cached copies. After completing 07, for a tool that uses certifi, add these options immediately after `uvx` in your usual command:

```text
--isolated --with "certifi>=2026.10.19"
```

Keep the rest of your command, including the tool name and its arguments. Use these options **each time** you run that tool: they select an environment meeting the requirement for that invocation. They do not repair every cached copy. If the tool also uses virtualenv, add `--with "virtualenv>=21.14.12"` before the tool name. Do not add packages the tool does not use. The Python selected for the command also needs the fix in 08.

#### Check the tool’s own packages

For a pip-installed tool, use `python -m pip show certifi virtualenv` in **its own environment**. For a uv-installed tool, use `uv tool list --show-paths`, then `uv pip show --python "TOOL_PYTHON" certifi virtualenv`, replacing `TOOL_PYTHON` with the Python executable inside that tool’s environment. These checks do not install missing packages; uv documents its inspection commands in the [CLI reference](https://docs.astral.sh/uv/reference/cli/#uv-pip-show).

Where installed, certifi must be **2026.10.19 or later** and virtualenv **21.14.12 or later**. For pip/Conda environments, use the applicable update in 10 or 13. `uv tool upgrade --all` updates these dependencies in installed tool environments too; if saved constraints block a fixed version, revise them using [Astral’s reinstall guidance](https://docs.astral.sh/uv/concepts/tools/#upgrading-tools). Do not manually modify a uv tool environment with pip.

These package updates do not guarantee a Python runtime upgrade, fix every vendor-bundled executable, or repair environments the tool previously created. Apply 08–10 to those environments too. Project version files remain in [12](#12-project-version-files-and-saved-environments).

[Back to action 27](#27-python-tools-installed-in-separate-environments)

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
