import requests
import mwparserfromhell
import bz2
import os
from tqdm import tqdm

# Define the URL of the Wikipedia dump file
dump_url = "https://dumps.wikimedia.org/enwiki/20230901/enwiki-20230901-pages-articles.xml.bz2"

# Define a function to check if an article is in the main namespace
def is_in_main_namespace(title):
    return not title.startswith("Wikipedia:") and not title.startswith("File:")

# Define a function to check if an article is orphaned
def is_orphaned(title, dump_file_path):
    # Store redirect titles
    redirect_titles = set()

    # Open the dump file for reading
    with bz2.open(dump_file_path, 'rt', encoding='utf-8') as dump_file:
        for line in dump_file:
            # Check if the line represents an article
            if line.startswith('<page>'):
                # Parse the page content using mwparserfromhell
                page = mwparserfromhell.parse(line)
                # Extract the title of the article
                article_title = page.filter_headings()[0].title.strip()
                
                # Check if the article is in the main namespace
                if is_in_main_namespace(article_title):
                    # Find all links in the article
                    links = page.filter_wikilinks()
                    
                    # Check if it's a redirect
                    is_redirect = False
                    for link in links:
                        if link.title.startswith("#REDIRECT"):
                            is_redirect = True
                            # Store the redirect title
                            redirect_titles.add(article_title)
                            break

                    # Check if the article has zero inbound links (excluding redirects)
                    if title == article_title and not is_redirect and title not in redirect_titles:
                        return True

    return False

# Define a function to find the source of inbound links to the test page
def find_inbound_link_source(title, dump_file_path):
    # Open the dump file for reading
    with bz2.open(dump_file_path, 'rt', encoding='utf-8') as dump_file:
        source_page = None
        for line in dump_file:
            # Check if the line represents an article
            if line.startswith('<page>'):
                # Parse the page content using mwparserfromhell
                page = mwparserfromhell.parse(line)
                # Extract the title of the article
                article_title = page.filter_headings()[0].title.strip()

                # Check if the article is in the main namespace
                if is_in_main_namespace(article_title):
                    # Find all links in the article
                    links = page.filter_wikilinks()
                    
                    # Check if the test page is linked from this article
                    for link in links:
                        if link.title == title:
                            source_page = article_title
                            break

                    if source_page:
                        break

    return source_page

# Define the paths for the dump file and output file
dump_file_path = "enwiki-20230901-pages-articles.xml.bz2"
output_file_path = "orphaned_articles_list.txt"

# Test if "Nanhai_Chao" is orphaned and provide reasons for failure
if is_orphaned("Nanhai_Chao", dump_file_path):
    print("'Nanhai_Chao' is identified as an orphan.")
else:
    # Check if "Nanhai_Chao" is not found in the dump
    if not os.path.exists(dump_file_path):
        print("Error: The Wikipedia dump file does not exist.")
    else:
        # Find the source of inbound links to "Nanhai_Chao"
        source_page = find_inbound_link_source("Nanhai_Chao", dump_file_path)

        if source_page:
            print("Error: 'Nanhai_Chao' is linked from the article:", source_page)
        else:
            print("Error: 'Nanhai_Chao' could not be identified as an orphan for an unknown reason.")

    # Exit the script
    exit()

# Check if the dump file already exists; if not, download it with a progress bar
if not os.path.exists(dump_file_path):
    print("Downloading the Wikipedia dump file...")

    # Get the file size for the progress bar
    response = requests.get(dump_url, stream=True)
    file_size = int(response.headers.get('content-length', 0))

    # Initialize tqdm for the progress bar
    with open(dump_file_path, 'wb') as dump_file:
        with tqdm(total=file_size, unit='B', unit_scale=True, unit_divisor=1024) as pbar:
            for data in response.iter_content(chunk_size=1024):
                dump_file.write(data)
                pbar.update(len(data))

    print("Download complete.")

# Process the dump file and collect orphaned articles
print("Collecting orphaned articles...")
collect_orphaned_articles(dump_file_path, output_file_path)
print("Collection complete. Orphaned articles are saved in 'orphaned_articles_list.txt'.")
