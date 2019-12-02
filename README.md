# Booste CLI 
The Booste CLI (command line interface) is a tool to easily run code in perosnal or shared development environments, rather than on local devices.

This is to avoid dependency issues, platform conflicts, and compute restrictions of local hardware.

## Installation

Run this command in your terminal to install the CLI tool via [pip](https://pip.pypa.io/en/stable/):

## Terminology 

### Codebox
Codebox refers to the remote platform in which teams run their code.

At the current state of Booste, each codebox is an isolated server instance, shared only by other verified team members in that codebox.

### Active Filesync
Active filesync is a background process that syncronizes the codebox files to your local Booste directory.

For macOS and Linux users, the synced directory is found at the path /home/{your-user-name}/dev/Booste.

When activated into a codebox, any files created, edited, or deleted within this local directory will be automatically uploaded to the codebox. The sync happens in less than 0.5 seconds for most text-based file saves.

At the current state of the Booste CLI, filesync pushes code from local to remote, but does not pull.

### Local ID 
A simple ID, which you'll use to specify that codebox for the "activate", "start", and "stop" commands.
### ID 
The unique ID of that codebox in the Booste system, to be shared with team members who wish to join that codebox, with the command "join".
### State 
The running state of the server hosting the codebox. The state may be changed with the "start" and "stop" commands.
### Name 
The name given to the codebox by its creator.

```bash
pip install booste-cli
```
## CLI Usage

### Login
```bash
booste login {optionally enter username here}
```
Booste login verifies your user account as valid and logs you in indefinitely.

You may pass your Booste username directly into the command, or enter it when prompted. The password entry prompt will follow. For password resets, please contact us at password.help@booste.io.

### Logout
```C
booste logout
```
Booste logout logs the current active user out of the system. There may only be one logged-in account per device.

### Codebox Activation

```C
booste activate {enter local codebox ID here}
```

Booste activate connects you into the selected codebox. Active filesync with the codebox begins, and you are placed within Booste's SSH wrapper, with the ability to interact with the codebox via Unix commands

Within the activated session, you can leave the codebox by typing the command
```bash
deactivate
```

It is vital to use this rather than forcefully backing out of the CLI  with Ctrl+C or Cmd+C, as deactivate triggers the shutdown of the active filesync. Filesync will fail in future activation attempts if deactivate is not used. Should this happen, stop the codebox and start it again.

### List All Codeboxes

```bash
booste list
```
Lists the codeboxes to which you have access.

### Start A Codebox

```C
booste start {optionally enter local codebox ID here}
```
Booste start will start the server hosting your codebox.

You may pass the desired local id directly into the command, or enter it when prompted.

This will not activate a codebox.

### Stop A Codebox

```C
booste stop {optionally enter local codebox ID here}
```
Booste stop will stop the server hosting your codebox.
You may pass the desired local id directly into the command, or enter it when prompted.

### Create A New Codebox

```bash
booste new {optionally enter codebox ID here}
```
Creates a new codebox and joins you into it as the first team member.

You may pass the desired codebox name directly into the command, or enter it when prompted. A password entry and password re-entry prompt will follow.

Codebox names are not required to be unique, though for clarity we recommend avoiding duplicates.

Codeboxes are password protected, and this password is needed for others to join your codebox. Be sure to save it. For codebox password resets, please contact us at password.help@booste.io.

### Join An Existing Codebox

```bash
booste join {optionally enter codebox ID here}
```
Adds you as a member into an existing codebox.

You may pass the desired codebox id directly into the command, or enter it when prompted. A password entry prompt will follow.

The full codebox "ID" and the password must be given to you by a team member, to ensure security.

### Leave a Codebox

```bash
booste leave {optionally enter existing codebox ID here}
```
Removes you as a member in a current codebox.

You may pass the desired local id directly into the command, or enter it when prompted.

You are free to re-join the codebox at any time with the "join" command as it does not stop the the server hosting your codebox.

## License 

Please read the document at the root of this repository for the end-user licensing agreement.
