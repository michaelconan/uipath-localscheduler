# Automatic Bot Scheduling

Creates scheduled task to invoke bot with Task Scheduler using the [UiRobot command line interface](https://docs.uipath.com/robot/docs/robot-command-line-interface).

Version update functionality assumes that UiPath Orchestrator packages follow a specific structure:
- Each automation project has a JSON-formatted `automation.info` file in the root project folder
    - `.info` file format:
    ```
    {
        "guid": "<guid>",
        "name": "<name>",
        "version": "<version>",
        "environment": "<environment>",
        "main_workflow": "Main.xaml"
    }
    ```
- Orchestrator packages are named using the GUID mentioned above, optionally with a version suffix (e.g., <guid>-v<#>) if an entirely new package is loaded for a process

UiRobot Command: `UiRobot.exe execute [--process <Package_ID> | --file <File_Path>] [--folder <Orchestrator_Folder_ID>] [--input <Input_Parameters>]`

Supports the following actions:
1. Schedule Orchestrator bot (via Robot Runner)
2. Schedule local bot (by selecting project.json file)
3. Updating Orchestrator bots to the latest version
4. Importing scheduled task from XML file (generated from process)
5. Adding input arguments to options 1 or 2

Scheduling actions perform the following (using [schtasks](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/schtasks)):
- Creates a schedule task in Microsoft task scheduler
- Exports a `.xml` file of the scheduled task to be imported or shared
- Generates a `.bat` (windows batch) file to execute the bot via script directly