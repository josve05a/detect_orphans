# Detect Orphaned Wikipedia Articles

This Python script checks if a specific Wikipedia article is orphaned, meaning it has zero inbound links from other Wikipedia articles in the main namespace (ns0) and is not linked to by redirects from the main namespace. The script provides reasons for failure, such as if the article is not found in the dump, has inbound links, or other unknown issues.

## Prerequisites

Before running the script, ensure you have the following prerequisites installed in your Python environment:

- tqdm: A fast, extensible progress bar library
- mwparserfromhell: A Python parser for MediaWiki wikicode

You can install these libraries using pip:

`pip install tqdm mwparserfromhell`

## Usage

1. Clone this repository or create a new one on GitHub.

2. Place the `run.py` script in your local repository folder.

3. Run the script using Python:

`python run.py`

The script performs the following steps:

- Tests if the article "Nanhai_Chao" (you can change the test article in the script) is orphaned.
- If the test article is not orphaned, it provides reasons for failure.
- If the test article is orphaned, it proceeds to download the Wikipedia dump file (if not already downloaded) and collects a list of 10 orphaned articles based on the specified criteria.
- The list of orphaned articles is saved in `orphaned_articles_list.txt`.

## Customization

You can customize the script by changing the following parameters:

- `dump_url`: The URL of the Wikipedia dump file.
- `is_in_main_namespace(title)`: Modify this function to define your custom criteria for articles in the main namespace.
- `is_orphaned(title, dump_file_path)`: Modify this function to check for orphaned articles based on your criteria.
- `find_inbound_link_source(title, dump_file_path)`: Modify this function to find the source of inbound links to a specific article.

## License

This script is provided under the GNU General Public License v3.0.

Feel free to use and modify this script for your specific needs.
