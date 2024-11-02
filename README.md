LinkedIn Easy Apply Bot
Automatically apply to LinkedIn Easy Apply jobs. This bot answers the application questions as well!

This is for educational purposes only. I am not responsible if your LinkedIn account gets suspended or for anything else.

This bot is written in Python using Selenium.

Setup
To run the bot, open the command line in the cloned repo directory and install the requirements using pip with the following command:

from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.common.action_chains import ActionChains
import time

# Configuration dictionary
config = {
    "LINKEDIN_EMAIL": "your-linkedin-email",
    "LINKEDIN_PASSWORD": "your-linkedin-password",
    "KEYWORDS": "your-job-search-keywords",
    "LOCATION": "your-job-search-location",
    "CV_PATH": "path-to-your-cv",
    # Add other configuration fields if needed
}

# Initialize the WebDriver
driver = webdriver.Chrome()  # Adjust to your browser driver
driver.get("https://www.linkedin.com/login")

# Login to LinkedIn
email_input = driver.find_element(By.ID, "username")
email_input.send_keys(config["LINKEDIN_EMAIL"])

password_input = driver.find_element(By.ID, "password")
password_input.send_keys(config["LINKEDIN_PASSWORD"])
password_input.send_keys(Keys.RETURN)

time.sleep(3)  # Wait for the login process to complete

# Go to the LinkedIn Jobs section
driver.get("https://www.linkedin.com/jobs/")
time.sleep(3)

# Search for jobs based on keywords and location
search_keywords = driver.find_element(By.XPATH, "//input[@aria-label='Search jobs']")
search_keywords.send_keys(config["KEYWORDS"])

search_location = driver.find_element(By.XPATH, "//input[@aria-label='Search location']")
search_location.clear()
search_location.send_keys(config["LOCATION"])
search_location.send_keys(Keys.RETURN)

time.sleep(3)  # Wait for search results to load

# Example: Loop through job listings and apply (basic structure)
jobs = driver.find_elements(By.CLASS_NAME, "job-card-container__link")
for job in jobs:
    try:
        job.click()
        time.sleep(2)
        
        # Click on the Easy Apply button if available
        easy_apply_button = driver.find_element(By.XPATH, "//button[contains(text(), 'Easy Apply')]")
        easy_apply_button.click()
        time.sleep(2)
        
        # Upload CV if prompted (based on LinkedIn's form layout)
        upload_cv = driver.find_element(By.XPATH, "//input[@type='file']")
        upload_cv.send_keys(config["CV_PATH"])
        
        # Submit the application
        submit_button = driver.find_element(By.XPATH, "//button[contains(text(), 'Submit application')]")
        submit_button.click()
        
        time.sleep(2)  # Wait before moving to the next job
    except Exception as e:
        print("Skipping job due to:", e)
        continue

# Close the driver
driver.quit()
