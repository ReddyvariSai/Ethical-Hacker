## Recon-ng

Recon-ng is an OSINT framework that is similar to the Metasploit exploitation framework or the Social-Engineering Tooklit (SET). If consists of a series of modules that can be run in their own workspaces. The modules can be configured to run with option settings that are specific to the module. This simplifies running Recon-ng at the command line because options for the modules are independently set within the workspace. When you run the module, it uses these settings to perform its searches.

As the name suggests, Recon-ng is used to perform a wide range of reconnaissance activities on different settings that you provide. Some modules are available with the Kali installation and others are available for download and installation in the Recon-ng modules marketplace.

---
### Create a workspace.

Recon-ng has auto complete. Press the tab button to complete commands and command options. Use the tab key twice to list the available commands and options at different places in the command line. This is very handy.

+ To run Recon-ng, open a new terminal window and enter recon-ng. You can also start the program by going to the Kali tools menu, searching for the app, and clicking the icon.
+ Note that the terminal prompt changes to indicate that you are working within the Recon-ng framework. Enter help to get a sense of the commands that are available.
+ Recon-ng uses workspaces to isolate investigations from one another. Workspaces can be created for different parts of a test or different customers for example. Type workspaces help to view options for the workspaces command.
+ Create a workspace named test by entering workspaces create followed by the workspace name. Note that the prompt has changed to indicate that you are in this workspace.
+ Type help to see the commands that are available within workspaces.

---
###  Investigate modules.

Recon-ng is a modular framework. Modules are Python programs with different functions. They are stored in an external marketplace that permits developers to create their own modules and contribute them for use by others.

Return to the Recon-ng prompt. Enter the modules search command. This will display the currently installed modules.

---
### Investigate the module marketplace.

Recon-ng will not function without modules. In this step, we will install modules from the Recon-ng marketplace. The module marketplace is a GitHub public repository. Search the web for recon-ng-marketplace to view the repository. Explore the folders to learn more about the modules.

+ In the terminal, view help for the marketplace command. Use the search option to list all the modules that are currently available.

[recon-ng][default] > marketplace search

+ Note that the modules are organized by their category and type. This appears as a path prepended to the name of the module. You can filter the output by adding a search term to the marketplace search command. Try a few different search terms that are related to OSINT information to get a sense of the modules that are available.
+ To learn more about individual modules, use the marketplace info command followed by the full name of the module, including its category and type. It is easier to select the name of the module and copy and paste it into the command line.

---
###  Install a new module.

Recon-ng accesses modules from the Github repository and downloads them to Kali when they are installed.


+ Search the marketplace modules using bing as a search term. Locate a module that requires no dependencies or API keys.
+ View information for this module.
+ To install the module, copy the full name, including the path, to the clipboard.
+ Enter the marketplace install command followed by the full name of the module.

[recon-ng][default] > marketplace install recon/domains-hosts/bing_domain_web

+ After installation, enter the modules search command to verify that the new module is now available.
+ Repeat the process to install the hackertarget module.

---
###  Run the new modules

+ Create a new workspace. Name it as you wish.
+ To start working with a module, it must be initialized. Enter modules load hackertarget to begin working with the module. Note that the prompt changes to reflect the loaded module.
+ Each module is its own environment. The developers of recon-ng have taken care to keep the framework consistent, so the same commands are available for each module. However, the options can vary. Type info at the module prompt to view important details about the module.
+ Instead of passing options at the command line, in Recon-ng you set the options and then enter a simple command to execute the module. Use the options set source command to set the only option for this module. Complete the command by specifying the target as hackxor.net.
+ Verify the option setting with the info command.
+ Type run to execute the module.
+ Inspect the output of the command. The output is stored in a database so you can refer to it later. The data that is stored is specific to the workplace in which it was gathered.
+ Enter the dashboard command. This queries the Recon-ng database and provides a summary of the information that has been gathered. It is specific to this workspace.
+ The show command displays the data for specific categories. Enter the show hosts command to display the list of hosts that were discovered.
+ Now repeat the process with the bing module. Compare the results with the hackertarget module.

---
### Investigate the web interface.
Recon-ng has a web interface that simplifies and improves viewing results that are stored in Recon-ng databases. It also allows easy export of the results tables for reporting purposes.

+ Open a new terminal.
+ Enter the recon-web command to start the Recon-ng server process. Note the command output.
+ In a new browser tab, access the webpage using the URL information provided in the output.
+ The web interface shows data from the default workspace when first opened. Click the orange workspace name at the top of the page to display data from different workspaces.


