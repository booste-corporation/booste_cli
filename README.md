![BoosteLogo](diagrams/Logo_Primary_NonTransparent.png)

# Booste CLI 
The [Booste](https://www.booste.io) CLI (command line interface) makes remote development feel the same as developing locally.
Write code using local IDEs and code editors, and run it in the cloud with a single execution command.

![SingleCommand](diagrams/oneword.png)

Booste remote evironments are shareable across your team, eliminating dependency issues, platform conflicts, and compute restrictions of local hardware. Website and API endpoints ran through Booste are published to public URLs and updated live as you save code. It is the perks of a browser-based IDE, without giving up your preferred local coding workflow.


# Table of Contents
[Terminology](#terminology)
- [Codebox](#terminology-codebox)
- [Active Filesync](#terminology-active-filesync)
- [Local ID](#terminology-local-id)
- [State](#terminology-state)
- [Name](#terminology-name)

[Installation](#installation)
- [Supported Platforms](#installation-supported-platforms)
- [Supported Tech Stacks](#installation-supported-stacks)

[CLI Usage](#usage)
- [Get Help](#usage-help)
- [Login](#usage-login)
- [Logout](#usage-logout)
- [Execute Code In Codebox](#usage-vanilla)
- [Configure Codebox (ssh)](#usage-activate)
- [List All Codeboxes](#usage-list)
- [Set a Default Codebox](#usage-default)
- [Start Codebox](#usage-start)
- [Stop Codebox](#usage-stop)
- [Restart Codebox](#usage-restart)
- [Create a Codebox](#usage-new)
- [Join a Codebox](#usage-join)
- [See Codebox Info](#usage-info)
- [Leave a Codebox](#usage-leave)
- [Delete a Codebox](#usage-delete)
- [Report a Bug](#usage-report)
- [Ask a Question](#usage-ask)
- [Reset the Client (bug cleaning)](#usage-reset)

[End User License Agreement](#eula)


# <a name="terminology"></a> Terminology 

### <a name="terminology-codebox"></a> Codebox
Codebox refers to the remote environment in which teams run their code.

At the current state of Booste, each codebox is an isolated server instance, shared only by other verified team members in that codebox. The environment, including packages and interpreters, are shared by the team. Each team member has their own repo, sitting side-by-side in the codebox.

![BoosteCodebox](diagrams/Shared.png)

### <a name="terminology-active-filesync"></a> Active Filesync
Active filesync is a background process that syncronizes the codebox files to your local Booste directory.

The synced directory sits at the user level:
```bash
~/Booste
```

While filesync is running, any files created, modified, or deleted within this local directory will be automatically uploaded to the codebox. The sync happens in less than one second for most text-based file saves. 

Larger repos placed in the filesync directory will slow the sync, so be sure to only include source code and ignore dependencies. Virtual environments, Docker images, packages, and interpreters should be installed via the [activate command](#usage-activate), not uploaded through filesync.

Notes on current CLI version:
 - Filesync pushes code from local to remote, but does not yet pull. Files generated by your process will exist on the remote side, but not locally.
 - Filesync starts only when the first [one-line-command](#usage-activate) or [activate command](#usage-activate) is ran. It will remain live until remote or local shutdown.

### <a name="terminology-local-id"></a> Local ID 
A simple ID, which you'll use to specify that codebox for various Booste commands.

### <a name="terminology-state"></a> State 
The running state of the server hosting the codebox. The state may be changed with the [start](#usage-start) and [stop](#usage-stop) commands.

### <a name="terminology-name"></a> Name 
The name given to the codebox by its creator.



# <a name="installation"></a> Installation

If you have not already, download and install [python3](https://www.python.org/downloads/) and [pip3](https://pip.pypa.io/en/stable/).

Run this command in your terminal to install the CLI tool:
```bash
pip3 install booste-cli
```
We recommend using the most recent version.



# <a name="installation-supported-platforms"></a> Supported Platforms
As of release 0.1.25
![Supported](diagrams/SupportedPlatforms.png)

### Supported:
- Linux
- MacOS
- Windows Subsystem For Linux (WSL)<sub>1</sub>

1) Booste can be installed and ran in [Windows 10 WSL](https://docs.microsoft.com/en-us/windows/wsl/install-win10), with some setup. [See here for WSL setup instructions.](wsl.md)

### Not Supported:
- Windows Command Prompt
- Windows PowerShell


# <a name="installation-supported-stacks"></a> Supported Tech Stacks
As of release 0.1.25

### Supported:
- Any<sub>1</sub> interpreted language, such as Python, Node.js, bash.
- Any<sub>1</sub> compiled language<sub>2</sub>, such as C, C++, Java, Go, Rust.
- Web servers running through ports 3000, or 8000-8100.

1) A member of the codebox may need to install the language interpreter/compiler into the codebox, via the activate command.
2) The compilation process runs in the codebox, and the resulting compiled file will be created in the codebox. 
   Remote files do not currently sync down to local, so the compiled file will not exist locally.
   When typing a one-liner, the path to the compiled file path will not be recognized locally, but it does indeed exist remotely and will run.

### Not Supported:
Apps with GUI based engines.
- Desktop GUI, such as Electron
- AR/VR
- Game engines, such as Unity



# <a name="usage"></a> CLI Usage

### <a name="usage-help"></a> Get Help
```bash
booste
```
```bash
booste --help
```
Prints a documentation-like help page to the console.

### <a name="usage-login"></a> Login
```bash
booste login {optional: username}
```
Verifies your user account as valid and logs you in indefinitely.

You may pass your Booste username directly into the command, or enter it when prompted. The password entry prompt will follow. For password resets, please contact us at password.help@booste.io.

For account creation, go to www.booste.io/cli/new_user/.

### <a name="usage-logout"></a> Logout
```C
booste logout
```
Logs the current active user out of the system. There may only be one logged-in account per device, and only one logged-in device per user.

### <a name="usage-vanilla"></a> One-Line Commands
```C
booste {codebox local ID} {unix command to be ran remotely}
```
Executes the given command in the specified codebox.
For example, to run a python file in codebox #2, use:
```bash
booste 2 python3 path/to/file.py
```

If a default codebox is set, the local ID may be omitted:
```bash
booste default 2
...
...
booste python3 path/to/file.py
```

This command will fail if pointing toward a directory or file that is not in the filesync directory.

### <a name="usage-activate"></a> Codebox Configuration
```C
booste activate {optional: codebox local ID}
```
Connects you into the selected codebox. You are placed into an [ssh](https://www.ssh.com/ssh/) session, with the ability to interact with the codebox via Unix commands. 

You may pass a codebox local ID directly into the command, or enter it when prompted.
If a default codebox is set, the local ID may be omitted.

It is through Activate that users set up the codebox environment, through tools such as [pip](https://pip.pypa.io/en/stable/), [npm](https://www.npmjs.com/), or [gem](https://rubygems.org/). When setting up the codebox, make sure that packages are available globally (no virtual environments or containers).

Within the activated session, you can leave the codebox by typing the command
```bash
logout
```

### <a name="usage-list"></a> List All Codeboxes
```bash
booste list
```
Lists the codeboxes to which you have access.

### <a name="usage-default"></a> Set A Default Codebox
```bash
booste default {optional: codebox local ID / "clear"}
```
Saves a codebox as the default codebox, so that local IDs do not need to be explicitly provided with each command.

You may pass a codebox local ID directly into the command, or enter it when prompted.
If "clear" is passed in rather than a local ID, any pre-existing default codebox will be removed.

### <a name="usage-start"></a> Start A Codebox
```C
booste start {optional: codebox local ID}
```
Starts the server hosting your codebox.

You may pass a codebox local ID directly into the command, or enter it when prompted.
If a default codebox is set, the local ID may be omitted.

The codebox will remain running until either the remote or local machines are shut down.

This will not activate a codebox.

### <a name="usage-stop"></a> Stop A Codebox
```C
booste stop {optional: codebox local ID}
```
Booste stop will stop the server hosting your codebox.

You may pass a codebox local ID directly into the command, or enter it when prompted.
If a default codebox is set, the local ID may be omitted.

### <a name="usage-restart"></a> Restart A Codebox
```C
booste restart {optional: codebox local ID}
```
Booste stop will stop and then restart the server hosting your codebox.

You may pass a codebox local ID directly into the command, or enter it when prompted.
If a default codebox is set, the local ID may be omitted.

### <a name="usage-new"></a> Create A New Codebox
```bash
booste new {optional: codebox name}
```
Creates a new codebox and joins you into it as the first team member.

You may pass the desired codebox name directly into the command, or enter it when prompted. A password entry and password re-entry prompt will follow.

Codebox names are not required to be unique, though for clarity we recommend avoiding duplicates with your team.

Codeboxes are password protected, and this password is needed for others to join your codebox. Be sure to save it. For codebox password resets, please contact us at password.help@booste.io.

### <a name="usage-join"></a> Join An Existing Codebox
```bash
booste join {optional: codebox full ID}
```
Adds you as a member into an existing codebox.

You may pass the desired codebox id directly into the command, or enter it when prompted. A password entry prompt will follow.

The full codebox "ID" and the password must be given to you by a team member, to ensure security.

### <a name="usage-info"></a> See Codebox Information
```bash
booste info {optional: codebox local ID}
```
Prints detailed information about the codebox, including:
- Name
- Unique ID
- Owner - The user who created it
- Members
- Instance Type - The hosting virtual machine, in [AWS EC2 terminology](https://aws.amazon.com/ec2/instance-types/)
- Server Region
- Lifespan - The time that a codebox remains live before being automatically stopped. The default is 4 hrs.
- State
- IP - The public IP, used to access web servers through ports 3000 or 8000-8100

You may pass a codebox local ID directly into the command, or enter it when prompted.
If a default codebox is set, the local ID may be omitted.

### <a name="usage-leave"></a> Leave a Codebox
```bash
booste leave {optional: codebox local ID}
```
Removes you as a member from a codebox that you previously joined.

You may pass a codebox local ID directly into the command, or enter it when prompted.
If a default codebox is set, the local ID may be omitted.

You are free to re-join the codebox at any time with the "join" command as it does not stop the the server hosting your codebox.

To leave a codebox you created, use Delete.

### <a name="usage-delete"></a> Permanently Delete a Codebox
```bash
booste delete {optional: codebox local ID}
```
Removes all members from a codebox that you previously created, and deletes it.

You may pass a codebox local ID directly into the command, or enter it when prompted.
If a default codebox is set, the local ID may be omitted.

You will be prompted to enter the codebox's password. For password resets, please contact us at password.help@booste.io.

You cannot revive a deleted codebox.

### <a name="usage-report"></a> Report a Bug
```bash
booste report
```
Files a bug report. You will be prompted to explain the bug.
If bugs become preventative, try running "reset" (below)

### <a name="usage-ask"></a> Ask a Question 
```bash
booste ask
```
Texts your question directly to a founder's cell phone. You will be prompted to input your question and reply-to phone number.

### <a name="usage-reset"></a> Reset the Client (bug cleaning)
```bash
booste reset
```
Clears caches, closes hanging ssh tunnels, and kills filesync.
Use if the [one-line commands](#usage-vanilla) or [activate](#usage-activate) command throw errors or hang.
Use if filesync appears to not be running; it will force a restart.



## <a name="eula"></a> License 
[View EULA (End-User License Agreement)](Booste_EULA.pdf)
