from selenium import webdriver
from selenium.common.exceptions import NoSuchElementException
from selenium.webdriver.support.ui import Select
from selenium.webdriver.common.keys import Keys
import os
import sys
import pprint
def login(usr, pwd):
    # enter in username
    user = browser.find_element_by_css_selector('#user_session_username')
    user.send_keys(usr)
    # enter in password
    user_password = browser.find_element_by_css_selector('#user_session_password')
    user_password.send_keys(pwd)
    # click login
    login_button = browser.find_element_by_css_selector('#user_session_submit')
    login_button.click()
def get_page(url):
    browser.get(url)
def logout():
    browser.get("https://www.vhlcentral.com/logout")
def check_element_existence(element_path):
    try:
        browser.find_element_by_xpath(element_path)
    except NoSuchElementException:
        return False
    return True
# this function is for getting file paths for files inside executable
def resource_path(relative_path):
    try:
        # PyInstaller creates a temp folder
        base_path = sys._MEIPASS
    except Exception:
        base_path = os.path.abspath(".")
    return os.path.join(base_path, relative_path)


print('Peter Edmonds: AutoVHLv1.4\n USE AT YOUR OWN RISK!')
print('Peter Edmonds: AutoVHLv1.5\n USE AT YOUR OWN RISK!')

while True:
    username = input("enter your username: ")
@@ -62,7 +62,7 @@ def resource_path(relative_path):
        pass

# start browser
browser = webdriver.Chrome(executable_path=resource_path('chromedriver.exe'))
browser = webdriver.Chrome(executable_path=resource_path('chromedriver'))

# go to vhl home
get_page("https://www.vhlcentral.com/home")
@@ -172,7 +172,7 @@ def resource_path(relative_path):
        # load assignment page
        get_page(hw_urls[assignment_number])

        # if assignment is a multi assignemnt
        # if assignment is a multi assignment
        if check_element_existence(
                '//form[contains(@name, "RecitalForm")]//input[contains(@type, "text")]') and check_element_existence(
                '//form[contains(@name, "RecitalForm")]//input[contains(@type, "radio")]'):
@@ -190,12 +190,21 @@ def resource_path(relative_path):
                    select.select_by_visible_text(answers_book[assignment_number][question])
                elif current_input.tag_name == 'input':
                    if current_input.get_attribute('type') == 'radio':
                        print('question' + str(question) + 'is a button')
                        print('question ' + str(question) + 'is a button')
                        input_parent = current_input.find_element_by_xpath("./..")
                        input_label = input_parent.find_element_by_xpath("./label/span").text
                        print(input_label)
                        if check_element_existence(
                        '//form[contains(@name, "RecitalForm")]//input[contains(@type, "radio")]') and check_element_existence(
                        '//label/span[contains(text(),"cierto")]') and check_element_existence(
                        '//label/span[contains(text(),"falso")]'):
                            print('question ' + str(question) + ' is a true / false')
                            '//form[contains(@name, "RecitalForm")]//input[contains(@type, "radio")]') and\
                                input_label == 'cierto' or input_label == 'falso':
                                    print('question ' + str(question) + ' is a true / false')
                                    if input_label == answers_book[assignment_number][question]:
                                        current_input.click()
                                    else:
                                        current_input.send_keys(Keys.ARROW_DOWN)
                                        current_input = browser.switch_to.active_element
                                        current_input.click()

                        else:
                            # answer as a normal button
                            print('question ' + str(question) + ' is a normal button')
@@ -245,7 +254,7 @@ def resource_path(relative_path):
        elif check_element_existence(
                '//form[contains(@name, "RecitalForm")]//input[contains(@type, "radio")]') and check_element_existence(
            '//label/span[contains(text(),"cierto")]') and check_element_existence(
            '//label/span[contains(text(),"falso")]'):
                '//label/span[contains(text(),"falso")]'):
            print(str(assignment_number) + '= true / false')
            options = browser.find_elements_by_xpath('//label/span')
            for question in range(len(answers_book[assignment_number])):
