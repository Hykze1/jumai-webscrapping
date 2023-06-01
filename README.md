# jumai-webscrapping
webscapping 


import pandas as pd
from bs4 import BeautifulSoup
import requests
import re # stands for reugular expression regular expressions; 
#they simply match themselves. You can concatenate ordinary characters, so last matches the string 'last'
import numpy as np 
import datetime

## jumai iphone site
url = "https://www.jumia.com.ng/catalog/?q=iphones"

headers = {'user-agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/113.0.0.0 Safari/537.36'}
r = requests.get(url, headers=headers)
r = requests.get(url)
##print(r) 
# you are to get 200 which shows your request is granted otherwise try other site like https://ticker.finology.in/ etc
soup = BeautifulSoup(r.content,"html.parser")
#boxes = soup.findAll("article", class_ = "prd _fb col c-prd")[4]
#we must put the specific index to get all the a tags by line 22
##print(boxes.find("a").text)

boxes = soup.findAll("article", class_ = "prd _fb col c-prd")
print(len(boxes)) # so we have 40 iphones phone displayed

#tag = soup.findAll("a", class_ = "core")
#print(tag)
today = [datetime.date.today()]
#print(today)
# now we have to iterate to the get the name, description of the phone and color, new price , old price and number of reviews
# by using a nested loop
v = []
w  = []
for i in boxes:
    # create a place holder for our list inside the nested loop
    name_List = []
    name_desc = []
    name = i.find("h3", class_ = "name")
    
    for i in name:
        #print(i.text)
        
        name_split = (name.text.split()) # we spilt the name so we can seperate the name from the specfication
        #print(name_split) # see the spilted name 
        name_split1 = (name_split[0] +" " + name_split[1]).strip()
        name_desc_color = name_split[1:]
        #print(name_split1) # printing the name 
        #print(name_desc_color) # printing the description and the color
        ![web 1](https://github.com/Hykze1/jumai-webscrapping/assets/100960483/4bd3bbcc-8064-4413-841e-9433f70b111a)

        name_List.append(name_split1)
        name_desc.append(name_desc_color)

        #print(name_List) # printing the name
        
        #print(name_desc) # and printing the description and color
    # now we put them in  Dataframe
        
        # i can do this but because i am using a nested for loop i dont need to give it a name , thus
        #tem_df = pd.DataFrame({"PRODUCT NAME":name_List})

        tem_df = pd.DataFrame({"":name_List}) # thus i take the "PRODUCT SPEC & COLOR" away

        # i can do this but because i am using a nested for loop i dont need to give it a name , thus
        #tem_df1 = pd.DataFrame({"PRODUCT SPEC & COLOR":name_desc})
        tem_df1 = pd.DataFrame({"":name_desc}) # thus i take the "PRODUCT SPEC & COLOR" away and continue the same manner for the rest
    #print(tem_df)
    #print(tem_df1)
    w.append(tem_df.transpose())
    v.append(tem_df1.transpose())
#print(w)
#print(v)

X = []
for i in boxes :
    old_price_list = []
    old_price = i.find("div", class_ = "old")
    for i in old_price or []:
        #print(old_price)
        old_price_list.append(i.text)
    #print(old_price_list)
        #tem_df2 = pd.DataFrame({"OLD PRICE":old_price_list})
        tem_df2 = pd.DataFrame({"":old_price_list})
    #print(tem_df2)
    X.append(tem_df2.transpose())
#print(X)

y = []
for i in boxes:
    new_price = i.find("div", class_ = "prc")
    new_price_list = []
    for i in new_price:
        new_price_list.append(i.text)
        #print(new_price.text)
    #print(new_price_list)
        #tem_df3 = pd.DataFrame({"NEW PRICE":new_price_list})
        tem_df3 = pd.DataFrame({"":new_price_list})
    #print(tem_df3)
    y.append(tem_df3.transpose())
#print(y)

z = []
for i in boxes:
    review_list = []
    review = i.find("div", class_ = "stars _s")
    for i in review or []:
    #print(review.text)
        review_list.append(i.text)
    #print(review_list)
    #tem_df4 = pd.DataFrame({"REVIEWS":review_list})
    tem_df4 = pd.DataFrame({"":review_list})
    #dftest = tem_df4.replace(r'^\s*$', np.NAN, regex=True)# replace empty list with NAN
    #print(tem_df4)
    z.append(tem_df4.transpose())
#print(z)

lst = [w,X,y,z]
#print(lst)

df5 = pd.DataFrame({"Product Name":w, "Product Spec":v, "Old Prices":X, "New Prices":y, "Numbers of Reviews":z})
#print(df5)
# convering it to excel
df5.to_excel("jumia_iphones_details.xlsx")
# converting it to a csv file
df5.to_csv("jumia_iphones_details.csv")






        


