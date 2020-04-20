# TCM-final-project
Jamie Begleiter/Leah Shindler final project

#import requests to get url, json for data, pprint to print data, random to randomly select a room, time for time.sleep
import requests, json, pprint, random, time

#connecting to api
url = 'https://calendarific.com/api/v2/holidays?api_key=a791916a771b14d273f803d7cda93442db2c8cb3&country=US&year=2020'
response = requests.get(url)
response.raise_for_status()

#loading holiday data into text format
holiday = json.loads(response.text)

#using the response gallery, holidaydata pulls just the response (info)
holidaydata = holiday['response']

#makes code user friendly and welcomes then to the musuem
print("Hello! What is your name?")
yourname = input()
print('Hi ' + yourname + '.')

#holidaydata is a dictionary with the key 'holidays', so this for loop leaves us with a list (holiday2) of all the information we need
for key, value in holidaydata.items():
    holiday2 = value

#creates empty list to later append
list = []

#for loop to extract particular data from holiday list
for y in holiday2:
    name = y.get('name')
    type = y.get('type')
    description = y.get('description')
    date = y.get('date')
    date1 = str(date)
    isodate = date1[9:19]
    type = y.get('type')
    list.append(type)

#makes list of holiday types without duplicates
HolidayType = []
for i in list:
    if i not in HolidayType:
        HolidayType.append(i)

#continues to increase user friendliness by asking how they'd like to search for holiday
print("Would you like to search by date or type of holiday?")
yourchoice = input()

if yourchoice.startswith("d" or "D"):
    print("date")
else:
    print("hi")
    
    
 = yourHoliday 
