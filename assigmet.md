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

# 2025-01-28


- Fatiha Mahmud Dheeraerahere, amk1002300@student.hamk.fi

Report of the project:
In this project, we are providing four codes, which will do broswer automation and software testing using RPA . The first code will collect the titles and prices of the books from the bookstore website and store them in an excel file and will also highlight the lowest price of the book. The second code will check if 'sort by prices' works correctly. The third code will check shopping cart functionality, and the fourth code will check if search box of the website works fine.

Code for Titles and prices(Also highlight the lowest price):
from robocorp.tasks import task from robocorp import browser

from RPA.HTTP import HTTP from RPA.Excel.Files import Files

@task def bookstore_python(): """Open the bookstore website Collect their titles and prices, highlights the lowest price, Saves the information in an excel file""" browser.configure( slowmo=100, ) open_website() titles,prices=collect_books_data() fill_excel_worksheet(titles,prices)

def open_website(): """Navigates to the given URL""" browser.goto("https://fspacheco.github.io/rpa-challenge/kirjakauppa.html")

def collect_books_data(): """Collects the titles and prices of all the books""" page = browser.page() titles=page.locator("div.kirjan-nimi").all_text_contents() prices_text=page.locator("div.hinta").all_text_contents()

prices=[]
for p in prices_text:
    prices.append(float(p.replace("$","")))
return titles,prices    
def fill_excel_worksheet(titles,prices): '''Saves the data to excel worksheet''' excel=Files() excel.create_workbook("bookstore.xlsx", sheet_name='books') excel.append_rows_to_worksheet([('Title','Price')])

for idx,t in enumerate(titles):
    excel.append_rows_to_worksheet([(t,prices[idx])])
excel.auto_size_columns("A",width=50)
# highlights the lowest price
lowest_price_index = prices.index(min(prices))
excel.set_styles("B2",color="yellow")
excel.save_workbook()
excel.close_workbook()
Code to check sorting by price:
from robocorp.tasks import task from robocorp import browser

@task def bookstore_python(): """Open the website and check if 'sort by prices' work correctly""" browser.configure( slowmo=100, ) open_website() test_price_sorting()

def open_website(): """Navigates to the given URL""" browser.goto("https://fspacheco.github.io/rpa-challenge/kirjakauppa.html")

def test_price_sorting(): """Collects the prices before and after sorting and compares them""" page = browser.page() prices_text=page.locator("div.hinta").all_text_contents()

prices=[]
for p in prices_text:
    prices.append(float(p.replace("$","")))
expected_prices=sorted(prices)
# collects prices after chosing sort by price:
page.locator("#sortSelect").select_option("title")
page.locator("#sortSelect").select_option("price")

prices_after=page.locator("div.hinta").all_text_contents()

sorted_prices=[]
for p in prices_after:
    sorted_prices.append(float(p.replace("$","")))

print("Expected order (lowest to highest):", expected_prices)
print("Actual order after clicking sort:", sorted_prices)

if sorted_prices == expected_prices:
    print("Success: Prices are sorted correctly!")
else:
    print("Error: Prices are not sorted correctly!")
Code to check functionality of shopping cart:
from robocorp.tasks import task from robocorp import browser

@task def bookstore_phython(): '''Check the bugs in website''' browser.configure( slowmo=100, ) open_website() check_shopping_cart()

def open_website(): '''Opens the following URL''' browser.goto("https://fspacheco.github.io/rpa-challenge/kirjakauppa.html") def check_shopping_cart(): '''Check if items are added correctly to shopping cart''' page=browser.page() page.locator(".esine:nth-child(1) > .lisaa-ostoskoriin").click() cart_quantity=page.locator(".cart-count").inner_text() # checks if shopping cart is working correctly if cart_quantity == 1: print("Shopping cart is working correctly") else: print(f"Error: Added 1 item but cart shows {cart_quantity} items!") print("Shopping cart is not working correctly")

Code to check if search box works correctly:
from robocorp.tasks import task from robocorp import browser

@task def bookstore_python(): """Opens the website and check if search box works correctly""" browser.configure( slowmo=100, ) open_website() test_search_box() def open_website(): """Navigates to the given URL""" browser.goto("https://fspacheco.github.io/rpa-challenge/kirjakauppa.html")

def test_search_box(): page = browser.page()

# Step 1: Get the title of first book on the page
book_titles = page.locator(".kirjan-nimi").all()
first_book_title = book_titles[0].inner_text()
print("Searching for book:", first_book_title)

# Step 2: Find the search box and type the book title
page.locator("#searchInput").fill(first_book_title)
page.wait_for_timeout(1000)

first_book = page.locator('.esine').all()


display_style = first_book[0].evaluate('(element) => window.getComputedStyle(element).display')
print(f'FIRST {first_book} display {display_style}')    
if display_style == 'none':
    print("Error: Book is hidden after search!")
else:
    print("Everything is fine and books are  found!")

# 2025-01-29
- Fatiha Mahmud Dheeraerahere, amk1002300@student.hamk.fi

Project Work
In the assignment we create a bot to read test files from a note taking app and fill a webpage from the file and create a pdf.
Here is the code:
from robocorp.tasks import task
from robocorp import browser 
from RPA.HTTP import HTTP  
from RPA.FileSystem import FileSystem  
from RPA.PDF import PDF
from datetime import datetime

@task
def expense_tracker_bot():
    browser_instance = browser 
    browser_instance.configure(slowmo=100)
    open_expense_page(browser_instance)
    download_expense_file()
    expense_records = fill_form_with_data(browser_instance)
    capture_webpage_snapshot(browser_instance)
    generate_pdf_summary(expense_records)


def open_expense_page(browser_instance):
    browser_instance.goto("https://fspacheco.github.io/rpa-challenge/expense-tracker.html")

def submit_expense_entry(browser_instance, expense):
    page = browser_instance.page()
    page.fill("#description", expense["Item"])
    page.fill("#amount", str(expense["Cost"])) 
    page.fill("#date", expense["TransactionDate"])  
    page.select_option("#category", expense["Category"])
    page.click("text= Add Expense")


def download_expense_file():
    http_client = HTTP()
    http_client.download(url="https://fspacheco.github.io/rpa-challenge/assets/list-expenses.txt", overwrite=True)

def fill_form_with_data(browser_instance):
    """Fetch the expense data from the text file and fill out the form"""
    file_system = FileSystem()
    file_content = file_system.read_file("list-expenses.txt")
    
    expense_data = []  

    for line in file_content.splitlines():
        columns = line.split()
        if len(columns) >= 4:  
            vendor_name = columns[1].strip() 
            amount = float(columns[2].strip().replace(",", ".")) 
            transaction_date = columns[0].strip()  
            category = columns[3].strip().lower()

            date_obj = datetime.strptime(transaction_date, "%d/%m")
            formatted_date = date_obj.strftime("2024-%m-%d")

            
            if "food" in category:
                category = "Food"
            elif "utilit" in category:
                category = "Utilities"
            elif "shop" in category or "shopping" in category or "shoping" in category:
                category = "Shopping"
            elif "entert" in category:
                category = "Entertainment"
            elif "oth" in category or "oter" in category:
                category = "Other"
            elif "transport" in category or "trasport" in category or "tranport" in category:
                category = "Transportation"

            
            expense_record = {
                "Item": vendor_name,
                "Cost": amount,
                "TransactionDate": formatted_date,
                "Category": category
            }
            expense_data.append(expense_record)  
            submit_expense_entry(browser_instance, expense_record) 

    return expense_data  


def capture_webpage_snapshot(browser_instance):
    """Take a screenshot of the current webpage"""
    page = browser_instance.page()
    page.screenshot(path="output/expense_summary.png")

def generate_pdf_summary(expense_data):
    """Create a PDF report with expense details and summary using HTML to PDF conversion."""
    pdf = PDF()
    category_totals = {}
    vendor_totals = {}

    
    for expense in expense_data:
        category = expense["Category"]
        vendor = expense["Item"]
        cost = expense["Cost"]

        
        category_totals[category] = category_totals.get(category, 0) + cost

        
        vendor_totals[vendor] = vendor_totals.get(vendor, 0) + cost


    top_category = max(category_totals, key=category_totals.get)
    top_vendor = max(vendor_totals, key=vendor_totals.get)

    
    html_content = "<h1>Expense Report</h1>"
    html_content += "<table border='1'><tr><th>Date</th><th>Vendor</th><th>Category</th><th>Cost ($)</th></tr>"
    for expense in expense_data:
        html_content += f"<tr><td>{expense['TransactionDate']}</td><td>{expense['Item']}</td><td>{expense['Category']}</td><td>{expense['Cost']:.2f}</td></tr>"
        html_content += "</table>"
        html_content += f"<h2>Highest Spending Category: {top_category} (${category_totals[top_category]:.2f})</h2>"
        html_content += f"<h2>Highest Spending Vendor: {top_vendor} (${vendor_totals[top_vendor]:.2f})</h2>"
        pdf.html_to_pdf(html_content, "output/expense_summary_report.pdf")
