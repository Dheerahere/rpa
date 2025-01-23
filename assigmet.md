# 2025-1-22

- Fatiha Mahmud Dheera, Dheerahere, amk1002300@student.hamk.fi

# Report of the project:
In this project,we created a bot by **UIPath Studio** which will open the page of **Hamk Finna** and it will search for the name of the book. Here are the steps:
1. We added the browser and searched for the hamk finna page.
2. Then we added the **type into** activity to seach for the name of the book **robotics**.
3. After that, we added the **click** activity to search it.
4. Then we added the **click** acitivity to open the book.

# 2025-1-22

- Fatiha Mahmud Dheera, Dheerahere, amk1002300@student.hamk.fi
# Report of the project:
In this project, we used Sema4.ai in VS Code to generate a code to search for the name of the book.
It will automatically open the page of **Project Gutenberg** to search for the book **The alchemist**.
In this project, we did one type and click as per the requirement of the project.
Here, we are adding the pictures of the project.

# 2025-o1-23

- Fatiha Mahmud Dheera, Dheerahere, amk1002300@student.hamk.fi

# Report of the project:
In this project,we created a bot which will fill the data from excel file into the music collection website.
It will add that automatically and then it take screenshot of the project .
And at the end,it will export that data as pdf file.
We are adding the code of the project in the following:
# Code:
```
from robocorp.tasks import task
from robocorp import browser

from RPA.HTTP import HTTP
from RPA.Excel.Files import Files
from RPA.PDF import PDF

@task
def music_form_filling_manager():
    '''Fills the music collection from excel file and export it as a pdf'''
    browser.configure(
        slowmo=100,
    )
    download_excel_file()
    open_the_website()
    log_in()
    fill_form_with_excel_data()
    collect_results()
    export_as_pdf()
def download_excel_file():
    '''Dowloads excel file from the given URL'''
    http=HTTP()
    http.download(url="https://fspacheco.github.io/rpa-challenge/assets/music-collection-sample.xlsx", overwrite=True)    
def open_the_website():
    '''Navigates to the given URL'''
    browser.goto("https://fspacheco.github.io/rpa-challenge/music-collection.html")
def log_in():
    """Fills in the login form and clicks the 'Login'button"""
    page=browser.page()
    page.fill("#username","student")
    page.fill("#password","rpa@hamk")
    page.locator("button:nth-child(4)").click()
def fill_and_add(music_data):
    '''Fills the form and adds to the collection'''
    page=browser.page()
    page.fill("#artist", music_data["Artist"])
    page.fill("#album", music_data["Album"])
    page.fill("#year", str(music_data["Release Year"]))
    page.select_option("#format",music_data["Format"])
    page.select_option("#condition",music_data["Condition"])
    page.click("#addToCollection")
def fill_form_with_excel_data():
    """Read data from excel and fill in the music form"""
    excel = Files()
    excel.open_workbook("music-collection-sample.xlsx")
    worksheet = excel.read_worksheet_as_table("music-collection",header=True)
    excel.close_workbook()

    for music_data in worksheet:
        fill_and_add(music_data)

def collect_results():
    """Take a screenshot of the page"""
    page = browser.page()
    page.screenshot(path="output/music_summary.png")

def export_as_pdf():
    """Export the data to a pdf file"""
    page = browser.page()
    music_results_html = page.locator("#fullMusicCollectionTable").inner_html()

    pdf = PDF()
    pdf.html_to_pdf('<table>'+music_results_html +'</table>', "output/musicdata.pdf")
```
