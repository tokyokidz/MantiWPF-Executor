[![Releases](https://img.shields.io/badge/Releases-Download-blue?style=for-the-badge&logo=github)](https://github.com/tokyokidz/MantiWPF-Executor/releases)

# MantiWPF Executor — Modern Roblox Script Executor & Injector

🎯 • UI-first executor built with WPF for script authors and testers  
🧰 • Clean API for script loading, sandboxing, and UI automation  
⚙️ • Active development, modular design, plugin-ready

![MantiWPF UI mockup](https://raw.githubusercontent.com/github/explore/main/topics/wpf/wpf.png)

Overview
-------
MantiWPF Executor provides a Windows desktop app that runs user scripts in a managed UI. It uses a modern WPF front end and a modular backend. The project focuses on a stable GUI, plugin support, script sandboxing, and a clear API. The app fits developer workflows that involve testing, automation, and UI-driven script execution.

This repository hosts the app, the UI assets, a small API layer, and a plugin system. The Releases page contains compiled builds. Download the release file and execute it to run the packaged app:
https://github.com/tokyokidz/MantiWPF-Executor/releases

Key features
-------
- Native WPF interface built for clarity and speed.  
- Script editor pane with syntax highlighting and line numbers.  
- Script runner with start/stop controls.  
- Plugin system for custom script loaders and UI modules.  
- Cross-module IPC for safe data exchange.  
- Settings manager with persistent profiles.  
- Auto-update hooks and release channel support.  
- Built-in logger with exportable logs.  
- Theme support (light and dark themes).  
- Hotkey bindings for common actions.

Screenshots
-------
Note: Screenshots show mock UI elements to illustrate layout and flow.

- Main window with script list and editor  
  ![Main window mock](https://cdn-icons-png.flaticon.com/512/1828/1828817.png)

- Settings and plugin manager  
  ![Settings mock](https://cdn-icons-png.flaticon.com/512/992/992700.png)

- Logger and output pane  
  ![Logger mock](https://cdn-icons-png.flaticon.com/512/2917/2917242.png)

Why MantiWPF Executor
-------
MantiWPF Executor aims to be a balanced tool. It keeps the UI simple for new users and the architecture open for power users. The app focuses on these areas:

- Usability: Clear layout and contextual controls let users focus on scripts.  
- Extensibility: A plugin API lets developers add loaders, parsers, and widgets.  
- Stability: The core engine isolates scripts from UI logic.  
- Portability: Builds target modern Windows with a small set of runtime dependencies.

Getting started
-------
1. Visit Releases and get the latest build:
   https://github.com/tokyokidz/MantiWPF-Executor/releases  
   Download the release file and execute it.

2. Run the installer or the portable binary. The UI appears with a sample script list.

3. Open a script from the list or create a new one. Use the editor pane to edit code.

4. Press Run to start the script. Use Stop to end the session.

System requirements
-------
- Windows 10 or later (x64).  
- .NET runtime version required for the build (the release notes list the exact runtime).  
- 500 MB free disk space for install and logs.  
- Optional: GPU driver for UI hardware acceleration.

Install options
-------
- Installer: Run the EXE or MSI from Releases. Follow standard Windows install steps.  
- Portable: Unzip the portable package and run the executable in the folder.  
- Dev build: Clone the repo and build in Visual Studio. Restore NuGet packages and ensure the target .NET SDK is installed.

Release link
-------
Click the badge or the link below to reach the releases page:
[Releases · tokyokidz/MantiWPF-Executor](https://github.com/tokyokidz/MantiWPF-Executor/releases)

The release page contains compiled builds. Download the provided release file and execute it to run the app. Each release entry lists runtime details, checksum, and a short changelog.

Core concepts
-------
- Executor: The component that runs a script in a controlled environment.  
- Host: The UI and process that manage an Executor instance.  
- Plugin: An add-on that extends UI, script parsing, or execution behavior.  
- Profile: A saved set of settings for runtime, appearance, and hotkeys.  
- Sandbox: A limited runtime environment that restricts access to host resources.

Architecture overview
-------
The app uses a layered design:

- Presentation: WPF XAML views, resources, and theme management.  
- ViewModel: MVVM pattern. Each view has a bound ViewModel.  
- Core: The script engine host and execution manager.  
- Plugins: DLLs that follow a plugin contract and load at runtime.  
- Utilities: Logging, settings, and update checks.

This split keeps UI code separate from execution logic. It helps maintain testable code and isolates risky operations.

Scripting model
-------
MantiWPF Executor supports a simple script model:

- Script units contain code, metadata, and execution flags.  
- The editor saves scripts as UTF-8 files in the user's Scripts folder.  
- Scripts load into an Executor instance on demand.  
- The Executor runs scripts within a sandboxed app domain or process, based on build.  
- The Host communicates with the Executor via a small IPC protocol for commands and events.

API basics
-------
The project exposes a small, stable API for plugins and automation:

- IExecutorHost: Register and control Executor instances.  
- IScript: Represent a script object: load, save, metadata.  
- IPlugin: Plugin entry and lifecycle hooks: Init, Start, Stop.  
- ILogger: Log levels and sinks for plugins.  
- ISettings: Read and write persistent settings scoped to the plugin or host.

Example pseudo-code for plugin integration (conceptual)
-------
The code below shows a concept. It is not a full plugin template and omits build details.

public class SamplePlugin : IPlugin
{
    public void Init(IExecutorHost host)
    {
        host.RegisterCommand("sample.echo", args => {
            host.Logger.Info("Echo: " + args.Join(" "));
            return true;
        });
    }

    public void Start() { }
    public void Stop() { }
}

Plugin system
-------
- Plugins are discovered in the Plugins folder at app run.  
- Each plugin must expose a manifest with name, version, and entry type.  
- The host loads plugins in a sandbox or limited AppDomain to avoid cross-plugin faults.  
- Plugins can add menu items, right-click actions, and custom panes.

Security model
-------
MantiWPF Executor uses a layered security model:

- Runtime isolation: Executors run in limited contexts to reduce host exposure.  
- Permission flags: Scripts request capabilities (I/O, network, host access) via manifest.  
- Explicit consent: The UI prompts the user to grant capabilities before a script runs.  
- Logging and review: The host records script actions for later review.

The host validates plugin manifests and enforces required signatures where available. The release notes include checksums and signing status.

Usage examples
-------
- Run a local automation script to exercise UI flows in a sandboxed test app.  
- Load a script that manipulates a saved JSON file for local testing.  
- Build a plugin that adds a debugger pane or a custom logger sink.  
- Use profiles to switch between test and production runtime flags.

Workflows
-------
1. Quick test workflow  
   - Open the app.  
   - Create a new script with test code.  
   - Grant the script necessary capabilities.  
   - Run and view output in the log pane.

2. Development workflow  
   - Install the developer package from Releases.  
   - Clone the repo and open the solution in Visual Studio.  
   - Implement your plugin under Plugins/YourPlugin.  
   - Build and copy the plugin DLL to the app Plugins folder.  
   - Restart the app and enable the plugin.

3. Production deployment workflow  
   - Build a signed release and upload it to Releases.  
   - Provide checksums and release notes.  
   - Use the auto-update hooks for staged rollouts.

Configuration and settings
-------
- Settings persist per user and per machine when requested.  
- Profiles keep runtime flags such as sandbox level and hotkeys.  
- The settings pane lets you export and import profiles in JSON.

Hotkeys
-------
The app exposes configurable hotkeys. Defaults include:

- Ctrl+R — Run script.  
- Ctrl+S — Save script.  
- Ctrl+N — New script.  
- Ctrl+Shift+L — Toggle logger pane.

Customization
-------
- Themes: Add a theme resource dictionary and register it in ThemeManager.  
- Editor: Swap the editor component for a different editor control. The project uses a pluggable editor adapter.  
- Logger: Add sinks to write logs to a file, database, or remote collector.

Troubleshooting
-------
- If the app fails to start, check the .NET runtime version listed on the release page.  
- If a plugin causes issues, start the app with plugins disabled (hold Shift while launching).  
- If script output shows no log lines, ensure the script requests log access in its manifest.  
- Use the exported log file when reporting issues on the Issues tab.

FAQ
-------
Q: Where do I get the latest build?  
A: Visit Releases and download the package. The releases page lists release assets, checksums, and runtime info:
https://github.com/tokyokidz/MantiWPF-Executor/releases

Q: Can I run multiple scripts at once?  
A: The app supports multiple Executor instances. Each instance runs in an isolated context.

Q: How do I add a new plugin?  
A: Implement the IPlugin contract and add a manifest file. Place the built DLL and manifest in the Plugins folder. Restart the host for discovery.

Q: How do I change themes?  
A: Open Settings, pick a theme, and apply. The app persists the theme in the current profile.

Q: Is there a portable version?  
A: Releases may contain both installer and portable builds. Check the assets for the portable ZIP.

Development guide
-------
This section helps contributors get a dev environment running.

Prerequisites
- Visual Studio 2022 or newer.  
- .NET SDK matching the target in the repo.  
- NuGet restore support.

Build steps
- Clone the repository.  
- Run dotnet restore or use Visual Studio to restore NuGet packages.  
- Build the solution.  
- Launch the host project.

Dev notes
- Keep the core execution engine small and testable.  
- Use MVVM and avoid code-behind where possible.  
- Write unit tests for the core API and run them in CI.  
- Keep plugin contracts stable to avoid breaking third-party plugins.

Testing
-------
- Unit tests cover the core host API and settings persistence.  
- Integration tests simulate the host-plugin lifecycle.  
- UI tests use a script-driven test harness when available.

CI and releases
-------
- CI runs build, tests, and packaging.  
- Release artifacts include an installer, portable ZIP, and checksums.  
- The Releases page shows signed build status and notes.

Contributing
-------
- Open an issue for feature requests or bugs.  
- Fork the repo and create a branch for your change.  
- Keep changes focused and add tests when possible.  
- Submit a pull request with a clear description of changes.

Code of conduct
-------
Be respectful. Keep discussions focused on code and design. Follow guideline in CONTRIBUTING.md if present.

License
-------
The project uses a permissive open source license. See LICENSE in the repo for details.

Changelog highlights
-------
- v1.4.0 — Plugin system stabilized, theme manager added, logger exports.  
- v1.3.2 — Fixes for plugin loading edge cases and improved logs.  
- v1.2.0 — Editor upgrades, hotkey support, and performance improvements.

Advanced topics
-------
Custom executor hosts
- The core API supports creating a headless host. Use this for automated test runners and CI tasks.

Remote control
- The host exposes an optional API for local network control. Plugins can implement a bridge for remote management. Secure this API to avoid unauthorized access.

Data flows
- The app separates user data into Scripts, Logs, and Profiles folders. Back these up when migrating installs.

Localization
- Strings live in resource files and support pluggable localization. Add a resource file for your locale and register it at startup.

Telemetry
- The host includes optional telemetry hooks. The code respects opt-in settings and does not send data unless enabled in settings.

Suggested workflow for plugin authors
-------
1. Start with a minimal plugin that logs a hello message on Init.  
2. Add a UI pane registered via the plugin manifest.  
3. Expose a settings section using ISettings.  
4. Add tests that exercise your plugin's Init and Stop hooks.  
5. Publish a release and include a sample script.

Testing plugins in isolation
-------
- Use the app's plugin sandbox mode to test without affecting the main profile.  
- Build a debug plugin and place it in a temp Plugins folder.  
- Start the host pointing at the temp folder for discovery.

Reporting issues
-------
When you file an issue, include:
- App version from the About dialog.  
- Steps to reproduce the issue.  
- A copy of the exported log if possible.  
- Your OS and .NET runtime version.

Community
-------
- Use the Issues tab for bug reports and feature requests.  
- Share plugins via GitHub or a plugin registry if available.  
- Engage in code review on pull requests.

Examples and templates
-------
- Plugin template: A minimal plugin project that implements IPlugin and a manifest.  
- Script template: Basic script files with metadata and capability requests.  
- Profile template: Sample profile JSON to demonstrate runtime flags.

Release checklist
-------
- Update version in assembly info.  
- Run tests and validate build artifacts.  
- Create release notes that list changes and runtime requirements.  
- Attach installer, portable build, checksums, and signing info.  
- Publish release on the Releases page.

Releases link reminder
-------
Access builds and artifacts here:
[https://github.com/tokyokidz/MantiWPF-Executor/releases](https://github.com/tokyokidz/MantiWPF-Executor/releases)  
Download the compiled release file for your platform and execute it. Each release details the runtime and install options.

Developer contact
-------
Open issues on GitHub for bugs or feature requests. Use PRs for code contributions. Maintainers review contributions and merge on a best-effort basis.

Roadmap
-------
- Stabilize plugin API and add stricter contracts.  
- Add a scripting sandbox audit and capability manager.  
- Expand the plugin registry and create a community hub.  
- Implement automated update channels and staged rollouts.

Legal and compliance
-------
The repository includes license metadata. Check LICENSE for permitted uses and distribution terms. Releases include checksums and signing info when available.

Appendix A — Folder layout
-------
/src — Host and UI projects  
/plugins — Sample plugins and templates  
/scripts — Sample scripts and templates  
/tests — Unit and integration tests  
/docs — Additional developer docs and API references

Appendix B — Common commands
-------
- dotnet build — Build the solution.  
- dotnet test — Run tests.  
- dotnet pack — Create NuGet packages for plugin contracts.  
- msbuild /t:Rebuild — Full rebuild for Visual Studio workflows.

Appendix C — Example profiles
-------
Sample profile JSON shows how to set hotkeys, theme, and security flags. Plugins can read profile sections under their own key.

Issue triage guide
-------
- Label bug reports with version and reproduction steps.  
- For feature requests, request a short use case and expected behavior.  
- Tag issues that need community help as "help wanted."

Glossary
-------
- Host: The app that manages execution.  
- Executor: A runtime instance that runs a script.  
- Plugin: A component that extends the host.  
- Profile: A saved configuration set.  
- Manifest: Metadata file that describes a script or plugin.

Thank you for exploring MantiWPF Executor. Visit the releases page to get a build, and check issues if you need help:
https://github.com/tokyokidz/MantiWPF-Executor/releases