# Comprehensive Windows Development Environment Setup Guide
This guide provides a structured, step-by-step walkthrough for configuring a robust development environment on Windows. The instructions are logically ordered to ensure dependencies are met and to streamline the setup process for system-level and application programming.

## Phase 0: Optional System Debloating & Clean Installation (Advanced Users)

This preliminary phase is **optional** and recommended for advanced users who want to start with the cleanest, most performant Windows installation possible before setting up their development tools. This process involves using a third-party utility to apply deep system tweaks, followed by a complete reinstallation of the operating system.

> **⚠️ Warning:** This process will erase all data on your computer. Ensure you have a complete backup of all important files before you begin.

The workflow is based on the methods demonstrated in this video: [**Windows 11 Setup Guide by Chris Titus Tech**](https://youtu.be/0PA1wgdMeeI?si=0jIhjGDUuJGoPVbu). It is highly recommended to watch the video to understand how the utility works.

### The Recommended Process:

1. **Watch the Video:** Understand the process and the "Microwin" utility demonstrated.
2. **Apply Recommended Tweaks:** Run the utility and apply a set of tweaks to optimize your system. A solid starting configuration includes:
   - **Essential Tweaks:**
   - <img width="633" height="514" alt="Screenshot 2025-09-04 231749" src="https://github.com/user-attachments/assets/1fbe1360-0904-4c63-9c0f-7df495725e0a" />
   - **Advanced Tweaks:**
   - <img width="603" height="454" alt="Screenshot 2025-09-04 231756" src="https://github.com/user-attachments/assets/54ea3168-1b46-4ced-91c4-64a688ee79ab" />
3. **Format and Reinstall:** After creating your optimized installation media, format your primary drive.
4. **Install a Fresh Windows:** Perform a clean installation of Windows.
5. **Update Windows Completely:** Once installed, immediately go to **Settings > Windows Update** and install all available updates until no more are found. This ensures your base system is secure and stable.

After completing this phase, you are ready to set up the development environment starting from Phase 1.

## Phase 1: Core System & Shell Preparation

This phase prepares the foundational elements of your system, including the command-line interface and virtualization technologies.

### 1. Configure PowerShell Execution Policy

To allow local and signed remote scripts to run, you must adjust PowerShell's execution policy. RemoteSigned is a recommended and balanced policy for a development environment.

Open PowerShell **as an Administrator** and run the following command:

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope LocalMachine
```

- You may be prompted for confirmation; press Y and then Enter.

### 2. Install Windows Subsystem for Linux (WSL 2)

WSL 2 is essential for running Linux environments directly on Windows and is a prerequisite for tools like Docker.

1. Open PowerShell **as an Administrator** and run the installation command:
   ```powershell
   wsl --install
   ```

2. This command enables required features, downloads the Linux kernel, sets WSL 2 as the default, and installs the Ubuntu distribution.
3. **Restart your computer** when prompted to complete the installation.

## Phase 2: Package Managers

Package managers automate the installation and management of software.

### 1. Install Chocolatey (System-Wide Package Manager)

Chocolatey simplifies the installation of Windows applications and tools.

1. Visit the [Chocolatey installation page](https://chocolatey.org/install#individual) and scroll down to **Setp 2**.
2. Open PowerShell **as an Administrator**.
3. Copy the installation command in section "**Install Chocolatey for Individual Use:**" from the website and execute it.
4. After completion, **close and reopen** PowerShell as an Administrator.
5. Verify the installation by running: `choco -v`.

### 2. Install Scoop (User-Space Package Manager)

Scoop installs programs to your user directory, avoiding UAC permission prompts. It's required for tools like the Supabase CLI.

1. Open a new PowerShell window (as a normal user) and run:
   ```powershell
   Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
   Invoke-RestMethod -Uri https://get.scoop.sh | Invoke-Expression
   ```

2. **Security Note:** This command downloads and executes a script from the internet. The source get.scoop.sh is official and trusted, but it's good practice to be aware of what such commands do.
3. Verify the installation: `scoop --version`.

## Phase 3: Core Development Kits & Runtimes

This section covers essential compilers, runtimes, and version control.

### 1. Install Git & GitHub CLI

1. Open PowerShell **as an Administrator** and run the following command:
   ```powershell
   choco install git gh -y
   ```

2. **Restart your terminal**.
3. Configure Git with your user information:
   ```powershell
   git config --global user.name "Your Name"
   git config --global user.email "your.email@example.com"
   ```

4. Authenticate the GitHub CLI, which will open a browser to log in:
   ```powershell
   gh auth login
   ```

5. Link your active GitHub account to your local Git configuration:
   ```powershell
   gh auth setup-git
   ```

6. Verify installations: `git --version` and `gh --version`.

### 2. Install C++ Development Tools

The Visual C++ build tools are required for compiling native modules for many tools, including Node.js and Python packages.

1. Download the **Visual Studio 2022 Community Installer** from the [official website](https://visualstudio.microsoft.com/downloads/).
2. Run the installer.
3. **Option A (Full IDE):** For full C++ development, select the **"Desktop development with C++"** workload.
4. **Option B (Tools Only):** If you only need the compilers for other programs, a lighter option is to go to the "Individual components" tab and install the latest **"MSVC..."** toolset and **"Windows SDK"**. This is often sufficient.
5. Proceed with the installation.

### 3. Install Java Development Kit (JDK)

Install a Long-Term Support (LTS) version for stability.

1. Open PowerShell **as an Administrator** and run the following command, Install OpenJDK 21 (LTS):
   ```powershell
   choco install openjdk --version=21 -y
   ```

2. Verify in a **new terminal**: `java -version` and `javac -version`.

### 4. Install Python

1. Install the latest stable version of Python:
   ```powershell
   choco install python -y
   ```

2. Verify in a **new terminal**: `python --version` and `pip --version`.

### 5. Install Node.js via NVM (Node Version Manager)

NVM is the recommended way to manage multiple Node.js versions.

1. Ensure any previous Node.js installations are uninstalled.
2. Install nvm-windows:
   ```powershell
   choco install nvm -y
   ```

3. **Open a new Administrator terminal** and enable NVM:
   ```powershell
   nvm on
   ```

4. Install the latest Node.js LTS version (e.g., 22.x), Preferably use 22:
   ```powershell
   nvm install 22
   nvm use 22
   ```

5. Verify installation: `node -v` and `npm -v`.

## Phase 4: Specialized Development Platforms

### 1. Install Docker Desktop

Docker is crucial for containerized applications and relies on the WSL 2 backend.

1. Download **Docker Desktop for Windows** from the [official website](https://docs.docker.com/desktop/install/windows-install/).
2. Run the installer, ensuring **"Use WSL 2 instead of Hyper-V"** is checked.
3. Follow the on-screen instructions and **restart your PC**.
4. Verify the installation: `docker --version` and `docker run hello-world`.

### 2. Install and Configure Android Studio

Necessary for Android development (e.g., for React Native or Flutter).

1. Download **Android Studio** from the [official developer website](https://developer.android.com/studio).
2. Follow the standard installation wizard.
3. After installation, open Android Studio and navigate to **More Actions → SDK Manager**.
4. In the **SDK Tools** tab, check the following components "NOT: in botton window checklist 'show package details'":
   1. **Android SDK Command-line Tools (latest)**
   2. **NDK to last version, and version (27.1.12297006)**
5. Click **Apply** to download and install the selected components.

## Phase 5: Environment Configuration

Properly configured environment variables are essential for the command line and other tools to locate your installed SDKs.

1. In the Windows Search bar, type `env` and select **"Edit the system environment variables"**.
2. In the System Properties window, click the **"Environment Variables..."** button.
3. Under **System variables**, check if the following variables were added by the installers. If not, add them by clicking **"New..."**:
   - `JAVA_HOME`: Should point to your JDK installation path (e.g., `C:\Program Files\Java\jdk-21`).
   - `ANDROID_HOME`: Create this and set its value to your Android SDK location (typically `C:\Users\{your_username}\AppData\Local\Android\Sdk`).
4. Under **User variables**, select the **`Path`** variable and click **"Edit..."**. Add the following line if it is not already present:
   ```
   C:\Users\{your_username}\AppData\Local\Android\Sdk\platform-tools
   ```
5. **Close and reopen all terminal windows** for the changes to take effect.
6. Verify the configuration:
   ```powershell
   echo $env:JAVA_HOME
   echo $env:ANDROID_HOME
   adb --version
   ```

## Phase 6: Essential CLI Tools & IDEs

Install additional command-line utilities and Integrated Development Environments (IDEs) to enhance your workflow.

### 1. Install General-Purpose CLI Tools

- **ImageMagick & FFmpeg:** Powerful tools for image and video manipulation.
  ```powershell
  choco install imagemagick -y
  choco install ffmpeg -y
  ```

- **Global Node.js Packages:**
  ```powershell
  npm install --global yarn vercel eas-cli
  ```

- **Supabase CLI:**
  ```powershell
  scoop install supabase
  ```

- Verify all installations in a new terminal:
  ```powershell
  magick -version
  ffmpeg -version
  yarn --version
  vercel --version
  eas --version
  supabase --version
  ```

### 2. Install IDEs and Code Editors

- **Visual Studio Code (Recommended):** A versatile and powerful code editor.
  ```powershell
  choco install vscode -y
  ```

- **Cursor:** An AI-powered code editor. Download from the [official website](https://cursor.com/). (Optional)
- **Windsurf:** An AI-powered code editor. Download from the [official website](https://windsurf.com/). (Optional)

## Phase 7: Useful Utilities

This is a curated list of recommended utilities to improve productivity and system management. Install them as needed using Chocolatey or by downloading from their websites.

### System Tools
- [Revo Uninstaller](https://www.revouninstaller.com/start-freeware-download-portable/) — Thoroughly removes applications along with leftovers.
- [IObit Unlocker](https://www.iobit.com/en/iobit-unlocker.php) — Unlocks and manages locked files and folders.
- [TakeOwnershipPro](https://www.majorgeeks.com/files/details/takeownershippro.html) — Quickly take ownership of files and folders.

### Editing & File Management
- [HxD Hex Editor (English)](https://mh-nexus.de/en/downloads.php?product=HxD20) — Fast and modern hex editor for binary file editing.
- [WinRAR](https://www.win-rar.com/download.html?&L=0) — Powerful archive manager for compression and extraction.
- [Notepad++](https://notepad-plus-plus.org/downloads/) — Advanced text editor with programming language support.

### Remote & Optimization
- [UltraViewer](https://www.ultraviewer.net/en/) — Remote desktop software for easy access.
- [Wise Memory Optimizer](https://www.wisecleaner.com/wise-memory-optimizer.html) — Optimizes RAM usage and improves performance.

### Media & Creativity
- [OBS Studio](https://obsproject.com/) — Screen recording and live streaming software.
- [CapCut](https://www.capcut.com/) — Simple and powerful video editing tool.
- [VLC Media Player](https://www.videolan.org/vlc/) — Versatile media player supporting many formats.

### AI Tools
- [Ollama](https://ollama.com/download/windows) — Run large language models locally.

---

## ✅ Final Verification Checklist

After completing all steps, open a new PowerShell window and ensure the following:

- PowerShell execution policy is set to `RemoteSigned`.
- WSL 2 and Ubuntu are installed and accessible (`wsl -l -v`).
- `choco`, `scoop`, `git`, and `gh` commands work.
- `java -version` shows the correct JDK version.
- `python --version` and `pip --version` work correctly.
- `node -v` and `npm -v` show the NVM-managed versions.
- `docker ps` executes without errors.
- Environment variables (`JAVA_HOME`, `ANDROID_HOME`) are correctly pointing to their installation directories.
- All additional CLI tools (`ffmpeg`, `yarn`, `supabase`, etc.) are recognized.
- Your preferred IDEs are installed and launch correctly.
