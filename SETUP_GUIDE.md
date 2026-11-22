# Set up your computer (Part 0 of the Streamlit guide)

Do these steps once, in order, before Part 1 of [README.md](README.md). When every step is done, go back to the guide.

You will build and test the app **on your own computer, in VS Code**, not in Colab. Steps 0.1–0.6 install everything you need. If you already have something, run its check command, and skip the step if the check passes.

> **How to open a terminal:**
> - **In VS Code (any system):** menu *Terminal → New Terminal*. Use this one whenever you can.
> - **Windows, outside VS Code:** Start menu → type *PowerShell* → open it.
> - **macOS:** Spotlight (⌘ Space) → type *Terminal* → open it.
> - **Linux:** your applications menu → *Terminal*.

### Step 0.1: Install VS Code

Download VS Code from <https://code.visualstudio.com> and install it with the default options. You'll use it to get your repository, edit files, run commands and talk to the AI assistant.

**Done when:** VS Code opens.

### Step 0.2: Install Git

First check whether Git is already installed. Open a terminal and type:

```
git --version
```

If it prints a version number (for example `git version 2.47.1`), skip to the next part of this step. Otherwise, install Git:

| System | How to install Git |
| --- | --- |
| **Windows** | Download from <https://git-scm.com/download/win> and run the installer. Keep all the default options; they include *Git Credential Manager*, which handles your GitHub login. Or, in PowerShell: `winget install --id Git.Git -e` |
| **macOS** | Run `git --version` in Terminal. If Git is missing, macOS offers to install the *Command Line Tools*: click **Install**. Or run `xcode-select --install` |
| **Linux (Ubuntu or Debian)** | `sudo apt update && sudo apt install git` |
| **Linux (Fedora)** | `sudo dnf install git` |

**Close and reopen VS Code and any terminals** after installing, then run `git --version` again.

Then tell Git who you are. Use the **email address of your GitHub account**:

```
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

**Done when:** `git --version` prints a version number.

### Step 0.3: Install Python

Check first. On Windows type `python --version`; on macOS and Linux type `python3 --version`.

If it prints **3.11, 3.12 or 3.13**, skip this step. If Python is missing or older, install **3.13**:

| System | How to install Python |
| --- | --- |
| **Windows** | Download from <https://www.python.org/downloads/>. On the first installer screen, **tick "Add python.exe to PATH"**, then click *Install Now*. |
| **macOS** | Download the macOS installer from <https://www.python.org/downloads/> and run it. |
| **Linux (Ubuntu or Debian)** | `sudo apt install python3 python3-venv python3-pip` |

Close and reopen your terminal, then run the check again.

In VS Code, also install the **Python** extension: click the Extensions icon in the left bar, search for *Python*, and install the one by Microsoft.

**Done when:** the check prints 3.11, 3.12 or 3.13.

### Step 0.4: Make sure your repository on GitHub is complete

The AI assistant can only use what is in your repository.

1. **If your notebooks live in Colab or Google Drive,** put them in the repository. Either:
   - in Colab, choose *File → Save a copy in GitHub* and pick your repository, or
   - choose *File → Download → Download .ipynb* and upload the file on GitHub (*Add file → Upload files*).
2. **Do the same for the data files your notebooks read,** if they are not in the repository yet. Files over 100 MB can't go on GitHub; the AI assistant will help you make a smaller version in Part 3, so for now just note where they are.
3. **Make the repository public:** on GitHub, go to *Settings → General → Danger Zone → Change visibility*. Before you do, check that:
   - your data is allowed to be published, and
   - the repository has no passwords, API keys or personal data.
4. Check that your notebooks run from top to bottom without errors.

**Done when:** your final notebooks are visible on your repository's GitHub page, and the repository is marked **Public**.

### Step 0.5: Clone your repository onto your computer

"Cloning" means downloading a copy of the repository that stays connected to GitHub, so you can send changes back later.

**Easiest way: with VS Code (Windows, macOS and Linux)**
1. On your repository's GitHub page, click the green **Code** button and copy the **HTTPS** link. It looks like `https://github.com/your-name/your-repo.git`.
2. In VS Code, open the Command Palette: **Ctrl+Shift+P** on Windows and Linux, **⌘⇧P** on macOS.
3. Type **Git: Clone**, press Enter, paste the link, and press Enter again.
4. Choose a folder to save it in, such as *Documents*.
5. Click **Open** when VS Code asks whether to open the cloned repository.
6. If VS Code or your browser asks you to sign in to GitHub, do it. This also lets you push changes later.

**Or with a terminal**

```
cd Documents
git clone https://github.com/your-name/your-repo.git
cd your-repo
code .
```

`code .` opens the folder in VS Code. If it doesn't work, use *File → Open Folder…* in VS Code instead.

**If you already cloned it earlier,** open the folder in VS Code and run `git pull` in the terminal to get the latest version.

**Done when:**
- VS Code shows your repository's files in the left panel, and
- running `git status` in VS Code's terminal says `On branch main` (or `master`).

### Step 0.6: Set up an AI assistant that works on your computer

**Why the assistant you use in Colab or a browser isn't enough:** to build the app, the assistant must read *all* the files in your repository, create new files, install packages, run the app and tests, and push to GitHub. Assistants inside Colab, or chat websites, only see what you paste or the notebook that is open, and can't run anything on your computer.

| Assistant | Where | Reads your whole repository? | Runs commands (install, test, run the app)? | Works for Part 3? |
| --- | --- | --- | --- | --- |
| Gemini in Colab | Browser, inside one notebook | No, only the open notebook | Only notebook cells, in Google's cloud | ❌ |
| gemini.google.com, claude.ai, chatgpt.com, Copilot on the web | Browser chat | No, only what you paste or upload | No | ❌ |
| **GitHub Copilot**, *Agent* mode | VS Code | Yes | Yes, after you approve | ✅ |
| **Claude Code** | VS Code extension or terminal | Yes | Yes, after you approve | ✅ |
| **Gemini Code Assist**, *Agent* mode | VS Code | Yes | Yes, after you approve | ✅ |

The key words are **in VS Code** and **agent mode**: the assistant works inside your repository folder and runs commands for you. In ordinary chat or "Ask" mode it can only answer questions.

Pick **one** of the three below. If you already pay for one of them, use that one.

**A. GitHub Copilot (free plan available; free Pro for verified students through GitHub Education)**
1. In VS Code, open Extensions, search for **GitHub Copilot** and install it.
2. Sign in with your GitHub account when asked.
3. Open the chat: the chat icon at the top of VS Code, or *View → Chat*.
4. Below the message box, change the mode from *Ask* to **Agent**.

**B. Claude Code (needs a paid Claude plan)**
1. In VS Code, open Extensions, search for **Claude Code** (published by Anthropic) and install it.
2. Click the Claude icon that appears in VS Code and sign in with your Claude account.
3. Claude Code always works as an agent; there is no mode to switch.

**C. Gemini Code Assist (free with a Google account)**
1. In VS Code, open Extensions, search for **Gemini Code Assist** (published by Google) and install it.
2. Click the Gemini icon in the left bar and sign in with your Google account.
3. In the Gemini chat, switch on **Agent** mode.

**Test it.** With your repository open in VS Code, ask the assistant:

```
List the notebooks and data files in this folder, then run "git status" and tell me the result.
```

**Done when:** the assistant lists *your* files and shows the `git status` result, after asking your permission to run the command. If it says it can't see files or run commands, you are not in agent mode, or the folder isn't open in VS Code.

---

All done? Go back to [README.md](README.md) and tick the Part 0 checklist.
