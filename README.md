# immich_remove_offline_files
A simple way to remove orphaned offline assets from Immich's database.

This Python script assists in managing the Immich database by detecting and removing orphaned media assets. 

These orphans may occur if files are deleted from the filesystem without being properly removed from Immich. 

Orphaned assets can be checked via the repair page within the admin interface of Immich.

## Prerequisites

Before running the script, ensure you have Python installed on your machine. The script requires Python 3.x.

You will also need to install the following Python packages:
- `requests`
- `halo`
- `tabulate`
- `tqdm`

These can be installed using the following command:
```bash
pip install requests halo tabulate tqdm
```
## Configuration

To use the script, you will need:
- An **Admin API key** from Immich for fetching reports.
- A **User-specific API key** for deleting assets.

Instructions for which can be found in the Immich docs - [Obtain the API key](https://immich.app/docs/features/command-line-interface#obtain-the-api-key)

## Usage

To run the script, navigate to the directory containing the script and execute:
```bash
python3 orphan_asset_cleaner.py
```
### Optional Arguments

- `--admin_apikey [ADMIN_API_KEY]`: Immich admin API key for fetching reports.
- `--user_apikey [USER_API_KEY]`: User-specific Immich API key for deletion.
- `--immichaddress [IMMICH_ADDRESS]`: Full web address for Immich, including protocol and port.
- `--no_prompt`: Enables deleting orphaned media assets without confirmation.

### Examples

To run the script with prompts for necessary inputs:
```bash
python3 orphan_asset_cleaner.py
```
To run the script without prompts (useful for automation):
```bash
python3 orphan_asset_cleaner.py --admin_apikey your_admin_key --user_apikey your_user_key --immichaddress http://yourimmichserver.com:port
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
- **Permission Errors**: Make sure the user API key has the appropriate permissions to delete assets. This may be becuase the results will show all orphaned assets located on the immich database using the admin API, but the delete API of the user only has the permissions to delete the assets for that user, if you run into a `400 error` you may need to asesess the assets to see what library they are located in and use the requisite user API key to delete successfully.

### Debugging Tips

- Ensure that all prerequisites are correctly installed and up-to-date.
- Check the command line for typos in your script arguments.
- Use the verbose or debug mode if available to get more detailed log output that can help in diagnosing problems.

## Contributions

Contributions to this project are welcome! Please feel free to submit issues, forks, or pull requests.

## License

This project is licensed under the GNU Affero General Public License version 3 (AGPLv3) to align with the licensing of Immich, which this script interacts with. For more details on the rights and obligations under this license, see the [GNU licenses page](https://opensource.org/license/agpl-v3).


