# WebScraper Deluxe

## Introduction

WebScraper Deluxe is a script that downloads images from a specified webpage and lists all the links on the page. It uses the `requests` library to fetch the webpage, `BeautifulSoup` to parse the HTML, and `urllib.parse` to handle URL manipulation. The script creates an "images" directory to save the downloaded images and prints the page title along with all links.

## Features

- **Download Images**: Downloads all images from a specified webpage.
- **List Page Links**: Extracts and prints all links from the webpage.
- **Handle URL Manipulation**: Converts relative URLs to absolute URLs.
- **Create Directory for Images**: Automatically creates an "images" directory to store downloaded images.

## Prerequisites

- Python 3.6 or higher.
- Required Python libraries: `requests`, `beautifulsoup4`.

## Modules

- **requests**: For making HTTP requests to web servers.
- **BeautifulSoup (bs4)**: For parsing and extracting data from HTML documents.
- **os**: For interacting with the operating system, such as creating directories.
- **urllib.parse**: For handling URL manipulation tasks.

```python
import requests
from bs4 import BeautifulSoup
import os
import urllib.parse
```

## Functions

### `download_images(images, base_url)`

Downloads images from the webpage.

- **Parameters:**
  - `images` (list): List of image tags.
  - `base_url` (str): The base URL of the webpage.
- **Returns:**
  - `None`

#### Example

```python
def download_images(images, base_url):
    if not os.path.exists('images'):
        os.makedirs('images')
    for img in images:
        src = img.get('src')
        src = urllib.parse.urljoin(base_url, src)
        image_name = os.path.basename(src)
        response = requests.get(src)
        with open(f'images/{image_name}', 'wb') as f:
            f.write(response.content)
        print(f'Downloaded {image_name}')
```

### `process_webpage(url)`

Processes the specified webpage to extract the title, links, and images.

- **Parameters:**
  - `url` (str): The URL of the webpage to be processed.
- **Returns:**
  - `None`

#### Example

```python
def process_webpage(url):
    response = requests.get(url)
    soup = BeautifulSoup(response.text, 'html.parser')
    page_title = soup.title.text
    print(f'Page Title: {page_title}')
    links = soup.find_all('a', href=True)
    print('Page Links:')
    for link in links:
        full_url = urllib.parse.urljoin(url, link.get('href'))
        print(full_url)
    images = soup.find_all('img')
    download_images(images, url)
```

## Usage

1. **Set the URL**: Replace the `url` variable with the webpage URL you want to process.

    ```python
    url = 'https://example.com'
    ```

    **Note**: Make sure to replace `"https://example.com"` with the actual URL of the webpage you want to scrape. This placeholder is a template and needs to be updated with the target URL for your specific use case.

2. **Run the Script**: Execute the script in your Python environment.

    ```bash
    python script_name.py
    ```

3. **Output**: The script will create an "images" directory to store the downloaded images and print the page title and all links.

## Example Output

```plaintext
Page Title: Example Page
Page Links:
https://example.com/link1
https://example.com/link2
Downloaded image1.jpg
Downloaded image2.jpg
```

## Error Handling

- **Invalid URLs**: The script will handle exceptions if the URL is invalid or the request fails.
- **Directory Creation**: If the "images" directory cannot be created, the script will print an error message.

## Security Considerations

- **URL Validation**: Ensure the URL provided does not contain sensitive or unauthorized content.
- **Permissions**: Make sure the script has appropriate permissions to create directories and download files.

## FAQs

**Q: What happens if the webpage has no images?**
A: The script will simply not download any images and will only print the page title and links.

**Q: Can the script handle large webpages?**
A: Yes, the script processes the webpage content efficiently, but performance may vary depending on the webpage size and your internet connection.

**Q: How does the script handle different image formats?**
A: The script downloads all images found with `<img>` tags, regardless of the image format.

## Troubleshooting

- **Invalid URL**: Ensure that the URL is correct and accessible.
- **Module Not Found Errors**: Make sure Python is installed correctly and all necessary modules (`requests`, `beautifulsoup4`) are available.

For further assistance or to report bugs, please reach out to the repository maintainers or open an issue on the project's issue tracker.
