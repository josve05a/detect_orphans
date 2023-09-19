# Detect Orphaned Wikipedia Articles

This Python script checks if a specific Wikipedia article is orphaned, meaning it has zero inbound links from other Wikipedia articles in the main namespace (ns0) and is not linked to by redirects from the main namespace. The script provides reasons for failure, such as if the article is not found in the dump, has inbound links, or other unknown issues.

## Prerequisites

Before running the script, ensure you have Python installed on your system. You also need to install the required libraries by running the following commands:

`pip install tqdm mwparserfromhell requests`


## Usage

1. Clone this repository or create a new one on GitHub.

2. Place the `run.py` script in your local repository folder.

3. Run the script using Python:

`python run.py`


The script performs the following steps:

- Checks if the example page "Nanhai_Chao" (you can change the test article in the script) is orphaned.
- If the test article is not orphaned, it provides reasons for failure.
- If the test article is orphaned, it proceeds to download the Wikipedia dump file (if not already downloaded) and identifies orphaned articles based on the specified criteria.
- The list of orphaned articles is saved in `orphaned_articles.txt`.

## Customization

You can customize the script by changing the following parameters:

- `example_page`: Replace this with the title of the Wikipedia article you want to test for orphan status.

Please note that this script is designed to work with English Wikipedia. If you need to analyze a different language edition of Wikipedia, you may need to modify the dump URL in the script accordingly.

## License

This script is provided under an open-source license. Feel free to use and modify it for your specific needs.
