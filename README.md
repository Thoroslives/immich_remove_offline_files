# Immich Remove Offline Files
A simple way to remove orphaned offline assets from Immich's database.

This Python script assists in managing the Immich database by detecting and removing orphaned media assets. 

These orphans may occur if files are deleted from the filesystem without being properly removed from Immich. 

Orphaned assets can be checked via the repair page within the admin interface of Immich.

## Prerequisites

Before running the script, ensure you have Python installed on your machine. The script requires Python 3.x.

### Download the Script

Simply download the script directly. Follow these steps:

1. Navigate to the GitHub page where the script is hosted.
2. Find the script file `immich_remove_offline_files.py`.
3. Right-click on the file and select "Save link as..." to save the script to your local machine.

Alternatively, if you are familiar with `curl` or `wget`, you can download the script using a command line tool. For example:

```bash
curl -O https://raw.githubusercontent.com/Thoroslives/immich_remove_offline_files/main/immich_remove_offline_files.py
```
### Install Dependencies

The script requires several Python packages to function correctly.
- `requests`
- `halo`
- `tabulate`
- `tqdm`

These can be installed using the following command:
```bash
pip install requests halo tabulate tqdm
```
Ensure all dependencies are installed correctly before attempting to run the script.

### Prepare Configuration
To use the script, you will need:
- An **Admin API key** from Immich for fetching reports.
- A **User-specific API key** for deleting assets.
Store these keys securely and use them as required when running the script.

Instructions for which can be found in the Immich docs - [Obtain the API key](https://immich.app/docs/features/command-line-interface#obtain-the-api-key)

## Usage

To run the script, navigate to the directory containing the script and execute:
```bash
python3 immich_remove_offline_files.py
```
### Optional Arguments

- `--admin_apikey [ADMIN_API_KEY]`: Immich admin API key for fetching reports.
- `--user_apikey [USER_API_KEY]`: User-specific Immich API key for deletion.
- `--immichaddress [IMMICH_ADDRESS]`: Full web address for Immich, including protocol and port.
- `--no_prompt`: Enables deleting orphaned media assets without confirmation.

### Examples

To run the script with prompts for necessary inputs:
```bash
python3 immich_remove_offline_files.py
```
To run the script without prompts (useful for automation):
```bash
python3 immich_remove_offline_files.py --admin_apikey "your_admin_api_key" --user_apikey "your_user_api_key" --immichaddress "http://IPADDRESS:port"
```
## How It Works

The script performs the following steps:
1. Fetches a report of orphaned media assets from the Immich server using the admin API key.
2. Displays the list of orphaned assets and asks for confirmation before deletion (unless `--no_prompt` is used).
3. Deletes the orphaned assets using the user API key, provided the confirmation was `yes`.

## Error Handling

The script includes basic error handling to manage common issues such as connectivity problems, invalid API keys, and permissions issues. 

Below are some of the errors you might encounter and the suggested steps to resolve them:

### Common Errors

- **Connection Errors**: If the script cannot reach the Immich server, ensure that the server address is correct and that your network connection is stable.
- **API Key Errors**: If you receive an error related to the API key, check that your API keys are correct and have the necessary permissions for the operations you are attempting.
- **Permission Errors**: Ensure the user API key has appropriate permissions to delete assets. Since the results will display all orphaned assets located in the Immich database using the admin API, the delete API of the user only allows deletion of the assets that the user has permission to manage. If you encounter a `400 error`, it may be necessary to assess the assets to determine which library they are located in and use the corresponding user API key that has the necessary permissions to delete those specific assets.

### Debugging Tips

- Ensure that all prerequisites are correctly installed and up-to-date.
- Check the command line for typos in your script arguments.
- Use the verbose or debug mode if available to get more detailed log output that can help in diagnosing problems.

## Contributions

Contributions to this project are welcome! Please feel free to submit issues, forks, or pull requests.

## License

This project is licensed under the GNU Affero General Public License version 3 (AGPLv3) to align with the licensing of Immich, which this script interacts with. For more details on the rights and obligations under this license, see the [GNU licenses page](https://opensource.org/license/agpl-v3).


