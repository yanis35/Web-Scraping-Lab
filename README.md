# Web-Scraping-Lab
Let's try to put what we learned in the previous lesson into use.

You can see the bad IP list screenshot from https://www.projecthoneypot.org/list_of_ips.php . We parsed the IP addresses in this table from this site and saved them in a text file to be able to make searches on them in our web access logs in the upcoming lessons.






First, we need to determine which data we will parse with BeatifulSoup. For this, the shortest way is to use the inspection feature of our web browser:






When we examine the source code, we see that the data we need is in the first column of an HTML table. Let's write the code to read this data:

                    

import requests
from bs4 import BeautifulSoup

URL = "https://www.projecthoneypot.org/list_of_ips.php" 
html_content = requests.get(URL)
soup = BeautifulSoup(html_content, 'html.parser')

ip_addresses = [] 
for row in soup.select('table.manmx tr')[1:]:
    ip_cell = row.select_one('td a.bnone') 
    if ip_cell:
        ip_addresses.append(ip_cell.text.strip())

for ip in ip_addresses: 
    print(ip)

                  
Copy





Let's examine our code line by line. First, as always, we start by importing the libraries we will need:

                    

import requests
from bs4 import BeautifulSoup

                  
Copy
In the next step, we assign the address from which we will retrieve data to a variable and then give this variable as a parameter to the get method of the requests library:

                    

URL = "https://www.projecthoneypot.org/list_of_ips.php" 
html_content = requests.get(URL)

                  
Copy


We could have also combined these two lines into one as follows, but we didn't because we might have to retype the URL over and over again if we needed to reuse it for any reason later in the code:

                    

html_content = requests.get("https://www.projecthoneypot.org/list_of_ips.php")

                  
Copy
We assign the output that we imported in the “html_content” as a parameter to the “ BeautifulSoup” object that we initialized with the “ soup ” variable, and in the second parameter, we specify that we want html_parser t o be used as the parser .

                    

soup = BeautifulSoup(html_content, 'html.parser')

                  
Copy
Now, we are ready to read the data, but first, we need a space to store the data we read. At this stage, we create an empty Python list object named ip_addresses to add the IP addresses we found:

                    

ip_addresses = []

                  
Copy
Now, it's time to start reading the data. First, we want BeatifulSoup to find the rows of the tables assigned the “manmx” class. Since the first line contains the header information, we tell it to skip the first line with [1:] which will take everything after line 1.

                    

for row in soup.select('table.manmx tr')[1:]:

                  
Copy
When we examined the source code, we saw that the IP Information we needed was in the " a " tag assigned to the “ bnone ” class. Now we are looking for:

                    

ip_cell = row.select_one('td a.bnone')

                  
Copy
Then, before we continue, we do a simple check to make sure we found what we're looking for:

                    

if ip_cell:

                  
Copy
If our code has come this far without any problems, it means that the IP information we need is right in front of us. It is a great idea to take this IP address and add it to the "ip_addresses" list we created before:

                    

ip_addresses.append(ip_cell.text.strip())

                  
Copy
While doing this, you can use the .strip() method, formatting at the beginning or end of the IP address, etc. We also do not forget to remove whitespace characters placed for certain purposes, so that they do not cause us issues when using this list in the future.

Now we have a list of IP addresses. We can print this list to the screen, save it to a text file, or print it to a database. In our example code, we printed the list on the screen and used the following code to do this:

                    

for ip in ip_addresses: 
    print(ip)

                  
Copy
If we wanted to print the output to the “iplist.txt” file instead of printing it to the screen, we would simply change the above part of our code as follows:

                    

with open('iplist.txt', 'w') as file: 
    for ip in ip_addresses: 
        file.write(ip + '\n')

                  
Copy
We could even do this to print both to the screen and to the file:

                    

with open('iplist.txt', 'w') as file: 
    for ip in ip_addresses: 
        print(ip)
        file.write(ip + '\n')

                  
Copy
As you can see in this lab, Python offers very powerful tools for web scraping, but this alone is not enough. We also have HTML, XML, LXML, etc. Being familiar with the formats and knowing them even at a basic level is a must for effective web scraping. Fortunately, LetsDefend has great courses where you can find answers to all of these.



