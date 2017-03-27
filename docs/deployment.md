# Deploying GitHub Desktop

GitHub Desktop currently supports Windows and macOS. How it gets installed varies on what operating system you are running.

 - on macOS
   - the `GitHub Desktop.zip` can be unpacked and added to your Applications list
 - on Windows you have two options:
   - the `GitHubDesktopSetup.exe` can be run to install for the current user
   - the `GitHubDesktopSetup.msi` can be run to install a machine-wide version of GitHub Desktop at `%PROGRAMFILES(x86)\GitHub Desktop Installer\desktop.exe` - each logged-in user will then be able to run GitHub Desktop

## Information for Administrators

If you manage a network of computers and want to install GitHub Desktop, here is more information about how things work:

 - `%LOCALAPPDATA%\desktop\` - contains the latest versions of the app, and maybe some older versions if the user has updated from a previous version.
 - `%LOCALAPPDATA%\GitHub Desktop\` - contains user-specific data that Electron requires to run. Also contains log files if errors have occurred while running the app.

