# TCM-final-project
Jamie Begleiter/Leah Shindler final project

#import requests to get url, json for data, pprint to print data, random to randomly select a room, time for time.sleep
import requests, json, pprint, random, time
import datetime as dt
from datetime import datetime



#connecting to api
url = 'https://calendarific.com/api/v2/holidays?api_key=a791916a771b14d273f803d7cda93442db2c8cb3&country=US&year=2020&type=national&type=religious'
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
    list.append(type)

#print(holiday2)


#makes list of holiday types without duplicates
HType = []
for i in list:
    if i not in HType:
        HType.append(i)


#continues to increase user friendliness by asking how they'd like to search for holiday
print("Would you like to search by date or type of holiday?")
yourchoice = input()

def holidayoutput():
    if yourchoice.startswith("d" or "D"):
        TodaysDate1 = dt.datetime.today()
   # print(TodaysDate1)

        datetime_object_list = []

        for x in holiday2:
            date1 = x.get('date')
            date2 = date1.get('iso')
            date3 = str(date2)
        #print(date2)R

            try:
                datetime_object = dt.datetime.strptime(date3, '%Y-%m-%d')
                datetime_object_list.append(datetime_object)
            except ValueError:
                pass

        #print(datetime_object_list)
        closest = min(datetime_object_list, key=lambda d: abs(d - TodaysDate1))
        #print(closest)

        for z in holiday2:
            name = z.get('name')
            d1 = z.get('date')
            d2 = d1.get('iso')
            d3 = str(d2)
            datetime_object1 = dt.datetime.strptime(d3, '%Y-%m-%d')
            description = z.get('description')

            if closest == datetime_object1:
                your_dated_holiday = name
                    #print(d2)

                print("The next upcoming holiday is " + your_dated_holiday + ". This holiday is on " + d2 + ".")
                yourHoliday = your_dated_holiday


    else:
        print("Here is a list of the types of holidays. Please select a type of holiday:")
        print(HType)
        yourHolidayType = input()
        yourHolidayType1 = yourHolidayType.lower()

        holidaytypelist = []

        for y in holiday2:
            type2 = y.get('type')
            type3 = str(type2)
            type4 = type3.lower()
            type5 = type4.strip('[]')
            name1 = y.get('name')

            if yourHolidayType1 in type5:
                holidaytypelist.append(name1)
                #holidaytypelist1 = str(holidaytypelist)

        print("The " + yourHolidayType + " holidays in your country are: " + str(holidaytypelist))

        time.sleep(5)
        randomholiday = random.choice(holidaytypelist)

        print("2020 seems like an exciting year! Based on my super holiday generator, I reccomend you celebrate " + randomholiday + "!")

        yourHoliday = randomholiday

holidayoutput()

 # start of recipe API

import requests, json, pprint # for the API data

url = f'https://api.spoonacular.com/recipes/random?number=1&tags={yourHoliday}&apiKey=6f873121665c40adb0c1fa22d3b87c09'
response = requests.get(url)
response.raise_for_status()  # check for errors

# load json data
recipeData = json.loads(response.text)

celebrating = 'yes'
cook = 'no'
answer = 'yes'

print('Are you going to be celebrating this holiday?')
celebrating = input()
if celebrating == 'yes':
    print('Are you going to cook for the meal?')
    cook = input()
    if cook == 'no':
        print('Me neither.')
    else:
        print('Would you like a recipe?')  # ask user if they want to receive a recipe
        answer = input()
        if answer == 'yes':
            print('Here is a link to your recipe:')
            pprint.pprint(recipeData['recipes'][0]['sourceUrl'])

            print('This is a summary of the recipe:')
            pprint.pprint(recipeData['recipes'][0]['summary'])

            print('These are the instructions for the recipe:')
            pprint.pprint(recipeData['recipes'][0]['instructions'])

        else:
            print('Okay, enjoy your holiday.')
else:
    print('Okay, have a nice day.')

# Write out the translated message to the output file:

outputFilename = 'recipe_file.txt'
outputFileObj = open(outputFilename, 'w')

outputFileObj.write('Here is a link to your recipe: ')
outputFileObj.write(str(recipeData['recipes'][0]['sourceUrl']))
outputFileObj.close()
