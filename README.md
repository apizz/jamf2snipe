# JAMF2Snipe 
## Import/Sync Computers from JAMF to Snipe-IT
```
usage: jamf2snipe [-h] [-v] [--dryrun] [-d]

optional arguments:
  -h, --help     show this help message and exit
  -v, --verbose  Sets the logging level to INFO and gives you a better idea of
                 what the script is doing.
  --dryrun       This checks your config and tries to contact both the JAMFPro
                 and Snipe-it instances, but exits before updating or syncing
                 any assets.
  -d, --debug    Sets logging to include additional DEBUG messages.
```
## Requirements:
This tool requires that python3 be installed on your system with the requests, json, time, and configparser python libs available. You will also need network access to both your jamf and snipe environments. You will also need a jamf username and password that has access to the API and write permissions for computer assets. You will need a snipe api key for a user that has edit/create permissions for assets and models. 

## Overview:
What does it do? This tool will sync assets between a JAMFPro instance and a snipe-it instance. The tool searches for assets based of of the serial number, and not the existing asset tag. If assets exist in JAMF and are not in snipe, the tool will create an asset, and try to match it with the model information available in snipe. If it can't find an appropriate model, it will create one. You need to set the manufacturer_id for Apple devices in the settings.conf file. When an asset is first created, it will fill out only the most basic information. When the asset alread exists in your Snipe Inventory, the tool will sync the information you specify in the settings.conf file and make sure that the asset_tag field in jamf matches the asset tag in snipe, where snipe's info is considered the authority. Lastly, if the asset_tag field is blank in JAMF when it is being created, then the tool will look for a 4 or more digit number in the computer name. If it fails to find one, it will use JAMFID-<jamfid#> as the asset tag in snipe. This way, you can easily filter this out and run scripts against it to correct in the future. 


## Installation:
Copy the files to your system (recommend installing to /opt/jamf2snipe/* ). Make sure you meet all the system requirement. Edit the settings.conf to match your current environment. The script will look for a valid settings.conf in /opt/jamf2snipe/settings.conf, /etc/jamf2snipe/settings.conf, or in the current folder (in that order): so either copy the file to one of those locations, or be sure that the user running the program is in the same folder as the settings.conf. 

## Configuration: 
All of the settings that are listed in the settings.conf are required except for the api-mapping section. It's recommended that you install these files to /opt/jamf2snipe/ and run them from there. You will need valid subsets of from JAMF's api to associate fields into snipe. More information can be found in the ./jamf2snipe file about associations and valid subsets. 
