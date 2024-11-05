![image](https://github.com/user-attachments/assets/bbbacbec-5512-4da8-aab0-ea04e43dac7e)
![image](https://github.com/user-attachments/assets/2d6b21bc-7533-497e-84b8-64f81a33bf18)
![image](https://github.com/user-attachments/assets/bfe07ad0-ea23-4dd1-952a-ce854cf23cb8)
![image](https://github.com/user-attachments/assets/d6fcc9c9-98c2-43f3-ad6c-fe7162e922af)
from tkinter import *
from tkinter import messagebox as mb
import requests
from datetime import datetime

#building layout

root = Tk()
root.title("WEATHER APP")
root.configure(bg='royal blue1')
root.geometry("700x450")

#function for getting weather
def get_weather():
    global city
    city = city_input.get()
    api_key = 'd273eeb9c62b40837ac7359411231ce4'
    url = f'http://api.openweathermap.org/data/2.5/weather?q={city}&appid={api_key}'
    response = requests.get(url)
    if response.status_code == 200:
        data = response.json()
        temp = data['main'] ['temp']-273.15
        humidity = data['main'] ['humidity']
        pressure = data['main'] ['pressure']
        wind = (data['wind'] ['speed'])*3.6
        epoch_time = data['dt']
        date_time = datetime.fromtimestamp(epoch_time)
        desc = data['weather'][0]['description']
        cloudy = data['clouds']['all']

        timelabel.config(text=str(date_time))
        temp_field.insert(0, '{:.2f}'.format(temp) + " celcius")
        pressure_field.insert(0, str(pressure) + " hPa")
        humid_field.insert(0, str(humidity) + " %")
        wind_field.insert(0, '{:.2f}'.format(wind) + " km/h")
        cloud_field.insert(0, str(cloudy) + " %")
        desc_field.insert(0, str(desc))
    else:
        mb.showerror("Error", "City not Found.Enter a valid city name")
        city_input.delete(0, END)

#function for reset
def reset():
    city_input.delete(0, END)
    temp_field.delete(0, END)
    pressure_field.delete(0, END)
    humid_field.delete(0, END)
    wind_field.delete(0, END)
    cloud_field.delete(0, END)
    desc_field.delete(0, END)
    timelabel.config(text='')


def get_forecast():
    global city
    city = city_input.get()

    # Get IP Location Information
    ip_res = requests.get('https://ipinfo.io/')
    ip_data = ip_res.json()
    citydata = ip_data['city']

    # Pass the City name to the url
    url = f'https://wttr.in/{citydata}?format=%C+%t'

    # Getting the Weather Data of the City
    res = requests.get(url)

    if res.status_code == 200:
        data = res.text.strip()
        # Display the current weather
        mb.showinfo("Current Weather", f"The current weather in {citydata} is: {data}")
    else:
        mb.showerror("Error", "City not Found. Enter a valid city name")
        city_input.delete(0, END)


#adding title
title = Label(root, text='Weather Detection and TimeZone', fg='black', bg='royal blue1', font=("bold", 15))
label1 =Label(root, text='Enter the city name :', font =('bold', 12), bg='royal blue1')
city_input = Entry(root, width= 24 , fg= 'red2' ,font= 12, relief=GROOVE)
timelabel = Label(root, text='', bg='royal blue1', font=('bold' , 14) , fg='yellow')

btn_submit = Button(root, text='Get Weather' , width=10, font=12 ,bg='lime green' , command=get_weather)
btn_forecast = Button(root, text='Weather forecast' , width=14, font=12 ,bg='lime green' , command = get_forecast)
btn_reset = Button(root, text='RESET' , font=12 ,bg='lime green' , command = reset)

label2 = Label(root, text="Temperature :", font=('bold',12), bg='royal blue1')
label3 = Label(root, text="Pressure :", font=('bold',12), bg='royal blue1')
label4 = Label(root, text="Humidity :", font=('bold',12), bg='royal blue1')
label5 = Label(root, text="Wind :", font=('bold',12), bg='royal blue1')
label6 = Label(root, text="Cloudiness :", font=('bold',12), bg='royal blue1')
label7 = Label(root, text="Description :", font=('bold',12), bg='royal blue1')

temp_field = Entry(root, width=24, font=11)
pressure_field = Entry(root, width=24, font=11)
humid_field = Entry(root, width=24, font=11)
wind_field = Entry(root, width=24, font=11)
cloud_field = Entry(root, width=24, font=11)
desc_field = Entry(root, width=24, font=11)

title.grid(row=0, column=1, padx=10, pady=10)
label1.grid(row=1, column=0, padx=10, pady=10)
timelabel.grid(row=1, column=2)
btn_submit.grid(row=2, column=1, pady=5)
btn_forecast.grid(row=2, column=2, pady=5)
label2.grid(row=3, column=0, padx=5, pady=5, sticky='W')
label3.grid(row=4, column=0, padx=5, pady=5, sticky='W')
label4.grid(row=5, column=0, padx=5, pady=5, sticky='W')
label5.grid(row=6, column=0, padx=5, pady=5, sticky='W')
label6.grid(row=7, column=0, padx=5, pady=5, sticky='W')
label7.grid(row=8, column=0, padx=5, pady=5, sticky='W')

city_input.grid(row=1, column=1, padx=5, pady=5)
temp_field.grid(row=3, column=1, padx=5, pady=5)
pressure_field.grid(row=4, column=1, padx=5, pady=5)
humid_field.grid(row=5, column=1, padx=5, pady=5)
wind_field.grid(row=6, column=1, padx=5, pady=5)
cloud_field.grid(row=7, column=1, padx=5, pady=5)
desc_field.grid(row=8, column=1, padx=5, pady=5)
btn_reset.grid(row=9, column=1, padx=5, pady=5)

root.mainloop()


FOR ADVANCE PY
# Python code to display schematic weather details
import requests
#Sending requests to get the IP Location Information
res = requests.get('https://ipinfo.io/')
# Receiving the response in JSON format
data = res.json()
# Extracting the Location of the City from the response
citydata = data['city']
# Prints the Current Location
print(citydata)
# Passing the City name to the url
url = 'https://wttr.in/{}'.format(citydata)
# Getting the Weather Data of the City
res = requests.get(url)
# Printing the results!
print(res.text)
# This code is contributed by PL VISHNUPPRIYAN


