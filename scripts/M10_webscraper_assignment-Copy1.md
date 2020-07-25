```python
####
#Author: brandon chiazza
#version 1.0
#references: 
#https://www.programiz.com/python-programming/working-csv-files
#https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/s3.html#S3.Client.create_bucket
#https://realpython.com/python-boto3-aws-s3/
#CLI aws s3api create-bucket --bucket my-bucket-name --region us-west-2 --create-bucket-configuration LocationConstraint=us-west-2
#https://robertorocha.info/setting-up-a-selenium-web-scraper-on-aws-lambda-with-python/ 
##

###Load libraries
import awscli
import selenium
import boto3
import pandas as pd
import time

from selenium import webdriver


####SCRAPE THE WEBSITE######
###call the webdriver
browser = webdriver.Chrome("C:/Program Files (x86)/Microsoft Visual Studio/Shared/Python37_64/chromedriver.exe")

#enter the url path that needs to be accessed by webdriver
browser.get('https://www.charitiesnys.com/RegistrySearch/search_charities.jsp')

#identify xpath of location to select element
inputElement = browser.find_element_by_xpath("/html/body/div/div[2]/div/table/tbody/tr/td[2]/div/div/font/font/font/font/font/font/table/tbody/tr[4]/td/form/table/tbody/tr[2]/td[2]/input[1]")
inputElement.send_keys('0')
inputElement1 = browser.find_element_by_xpath("/html/body/div/div[2]/div/table/tbody/tr/td[2]/div/div/font/font/font/font/font/font/table/tbody/tr[4]/td/form/table/tbody/tr[10]/td/input[1]").click()

#identify the table to scrape
table = browser.find_element_by_css_selector('table.Bordered')

print(table)

```

    <selenium.webdriver.remote.webelement.WebElement (session="13ac90544701a6e83acf8633bb97114f", element="78101d62-49de-4aca-92dc-4f1fde76e40a")>
    


```python
#####CREATE DATE FRAME#####
#create empty dataframe
df =[]

#loop through dataframe to export table
for row in table.find_elements_by_css_selector('tr'):
      cols = df.append([cell.text for cell in row.find_elements_by_css_selector('td')])


#update dataframe with header 
df = pd.DataFrame(df, columns = ["Organization Name", "NY Reg #", "EIN" ,"Registrant Type","City","State"])
display(df) #let's have a look at the data before creating the CSV file and loading it into s3 
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Organization Name</th>
      <th>NY Reg #</th>
      <th>EIN</th>
      <th>Registrant Type</th>
      <th>City</th>
      <th>State</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
    </tr>
    <tr>
      <th>1</th>
      <td>"Studio 5404" Inc.</td>
      <td>44-39-58</td>
      <td>463180470</td>
      <td>NFP</td>
      <td>MASSAPAQUA</td>
      <td>NY</td>
    </tr>
    <tr>
      <th>2</th>
      <td>"THEY ARE HAITIAN" FUND, INC.</td>
      <td>20-63-46</td>
      <td>300170128</td>
      <td>NFP</td>
      <td>HUDSON</td>
      <td>NY</td>
    </tr>
    <tr>
      <th>3</th>
      <td>(ASMA) American Syrian Multicultural Associati...</td>
      <td>42-84-63</td>
      <td>273130182</td>
      <td>NFP</td>
      <td>BROOKLYN</td>
      <td>NY</td>
    </tr>
    <tr>
      <th>4</th>
      <td>#WalkAway Foundation</td>
      <td>47-15-80</td>
      <td>832820906</td>
      <td>NFP</td>
      <td>CARLSBAD</td>
      <td>CA</td>
    </tr>
    <tr>
      <th>5</th>
      <td>04/11 10:17 PM test</td>
      <td>47-13-95</td>
      <td>206256427</td>
      <td>NFP</td>
      <td>ALBANY</td>
      <td>NY</td>
    </tr>
    <tr>
      <th>6</th>
      <td>1/20/21 Action Fund</td>
      <td>46-99-13</td>
      <td>832210730</td>
      <td>NFP</td>
      <td>SAN FRANCISCO</td>
      <td>CA</td>
    </tr>
    <tr>
      <th>7</th>
      <td>10/40 Connections, Inc.</td>
      <td>45-70-15</td>
      <td>621825230</td>
      <td>NFP</td>
      <td>HIXSON</td>
      <td>TN</td>
    </tr>
    <tr>
      <th>8</th>
      <td>1000 Feet Project, Inc</td>
      <td>45-00-14</td>
      <td>473820859</td>
      <td>NFP</td>
      <td>NEW YORK</td>
      <td>NY</td>
    </tr>
    <tr>
      <th>9</th>
      <td>1000 Islands Hose Haulers</td>
      <td>45-38-38</td>
      <td>454570241</td>
      <td>NFP</td>
      <td>CARTHAGE</td>
      <td>NY</td>
    </tr>
    <tr>
      <th>10</th>
      <td>1004 Foundation, Inc.</td>
      <td>45-00-13</td>
      <td>463110658</td>
      <td>NFP</td>
      <td>LONG ISLAND CITY</td>
      <td>NY</td>
    </tr>
    <tr>
      <th>11</th>
      <td>100 BLACK MEN OF LONG ISLAND DEVELOPMENT GROUP...</td>
      <td>20-08-47</td>
      <td>113617702</td>
      <td>NFP</td>
      <td>HEMPSTEAD</td>
      <td>NY</td>
    </tr>
    <tr>
      <th>12</th>
      <td>100 BLACKS WHO CARE INC</td>
      <td>06-42-84</td>
      <td>311628801</td>
      <td>NFP</td>
      <td>BROOKLYN</td>
      <td>NY</td>
    </tr>
    <tr>
      <th>13</th>
      <td>100 BLACK WOMEN OF LONG ISLAND FOUNDATION INC</td>
      <td>05-79-45</td>
      <td>113101805</td>
      <td>NFP</td>
      <td>GARDEN CITY</td>
      <td>NY</td>
    </tr>
    <tr>
      <th>14</th>
      <td>100Cameras, Inc.</td>
      <td>43-16-19</td>
      <td>264692506</td>
      <td>NFP</td>
      <td>NEW YORK</td>
      <td>NY</td>
    </tr>
    <tr>
      <th>15</th>
      <td>100 CLUB OF BUFFALO INC</td>
      <td>03-01-89</td>
      <td>160957291</td>
      <td>NFP</td>
      <td>WILLIAMSVILLE</td>
      <td>NY</td>
    </tr>
  </tbody>
</table>
</div>



```python
###LOAD THE FILE INTO S3####
# prepare csv file name   
pathname = 's3:/NAME_OF_YOUR_S3_BUCKET/'#specify location of s3:/{my-bucket}/
filename= 'charities_bureau_scrape_' #name of your group
datetime = time.strftime("%Y%m%d%H%M%S") #timestamp
filenames3 = "%s%s%s.csv"%(pathname,filename,datetime) #name of the filepath and csv file

#load file into s3. Pandas actually leverages boto to connect to s3 and can push the file directly into an s3 bucket
df.to_csv(filenames3, header=True, line_terminator='\n') 

#print success message
print("Successfull uploaded file to location:"+str(filenames3))
```

    Successfull uploaded file to location:s3:/database-update-bucket/charities_bureau_scrape_20200725151624.csv
    
