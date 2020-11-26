import requests, json
import random as rand
from pandas import *
from tkinter import messagebox as msgbox
import matplotlib.pyplot as plt
from matplotlib.backends.backend_tkagg import FigureCanvasTkAgg
import tkinter as tk
from tkinter import *
import mplcursors

def status():
    Status_Now = requests.get('https://covid19api.com/')
    if Status_Now.status_code != 200:
        msgbox.showinfo('Oh No','Server is not responding right now. Please try it later!')
        pencere.quit()

def searching():
    listbox.select_clear(0, 248)
    found = 0
    for i in range(0, 248):
        if search.get() == listbox.get(i):
            listbox.select_set(i)
            listbox.see(i)
            found = 1
    if found != 1:
        msgbox.showerror(title='Not Found', message='Country could not found. Please type it exactly same!')

def random():
    listbox.select_clear(0, 185)
    discover = rand.randint(0, 185)
    listbox.select_set(discover)
    listbox.see(discover)

def draw_graph():
    global target_type
    for i in range(listbox.size()-1,-1,-1):
        if listbox.select_includes(i):
            target_country2 = ulke_slug[i]
    for i in range (listbox_Type.size()-1,-1,-1):
        if listbox_Type.select_includes(i):
            target_type = listbox_Type.get(i)

    if target_country2 == 'china':
        test = requests.get('https://api.covid19api.com/total/country/china')
    else:
        test = requests.get('https://api.covid19api.com/total/dayone/country/%s' % (target_country2))
    country_info = []
    country_info = test.json()
    country_Confirmed = []
    country_Recovered = []
    country_Deaths = []
    country_Active = []

    if target_type == 'Confirmed':
        day = 1
        days = []
        for i in country_info:
            country_Confirmed.append(i['Confirmed'])
            day +=1
        for i in range(1, day):
            days.append(i)
        title = 'Confirmed'
        color ='red'
        kind = 'line'
        result(country_Confirmed, days, title, color, kind)


    if target_type == 'Deaths':
        day = 1
        days = []
        for i in country_info:
            country_Deaths.append(i['Deaths'])
            day += 1
        for i in range(1, day):
            days.append(i)
        title = 'Deaths'
        color = 'black'
        kind = 'line'
        result(country_Deaths, days, title, color, kind)


    if target_type == 'Recovered':
        day = 1
        days = []
        for i in country_info:
            country_Recovered.append(i['Recovered'])
            day += 1
        for i in range(1, day):
            days.append(i)
        title = 'Recovered'
        color = 'green'
        kind = 'line'
        result(country_Recovered, days, title, color,kind)

    if target_type == 'Active':
        day = 1
        days = []
        for i in country_info:
            country_Active.append(i['Active'])
            day += 1
        for i in range(1, day):
            days.append(i)
        title = 'Active'
        kind = 'line'
        color = 'orange'
        result(country_Active, days, title, color, kind)

    country_daily_confirmed = []
    country_daily_deaths = []
    country_daily_recovered = []

    if target_type == 'Daily Confirmed':
        day = 1
        days = []
        dc = []
        for i in country_info:
            dc.append(i['Confirmed'])
            day += 1
        for i in range(0, (day)-2):
            if (dc[i+1]-dc[i]) <= 0:
                country_daily_confirmed.append(0)
            else:
                country_daily_confirmed.append(dc[i+1]-dc[i])
            days.append(i+1)
        title = 'Daily Confirmed'
        color = 'red'
        kind = 'bar'
        result(country_daily_confirmed, days, title, color,kind)

    if target_type == 'Daily Deaths':
        day = 1
        days = []
        dd = []
        for i in country_info:
            dd.append(i['Deaths'])
            day += 1
        for i in range(0, (day) - 2):
            if (dd[i + 1] - dd[i]) <= 0:
                country_daily_deaths.append(0)
            elif (dd[i + 1] - dd[i]) > 0:
                country_daily_deaths.append(dd[i + 1] - dd[i])
            days.append(i + 1)
        title = 'Daily Deaths'
        color = 'black'
        kind = 'bar'
        result(country_daily_deaths, days, title, color, kind)

    if target_type == 'Daily Recovered':
        day = 1
        days = []
        dr = []
        for i in country_info:
            dr.append(i['Recovered'])
            day += 1
        for i in range(0, (day) - 2):
            if (dr[i + 1] - dr[i]) <= 0:
                country_daily_recovered.append(0)
            elif (dr[i + 1] - dr[i]) > 0:
                country_daily_recovered.append(dr[i + 1] - dr[i])
            days.append(i + 1)
        title = 'Daily Recovered'
        color = 'green'
        kind = 'bar'
        result(country_daily_recovered, days, title, color, kind)

def result(type, days, title, color, kind):
    data1 = {'Days': days,
             title : type
             }

    df1 = DataFrame(data1, columns=['Days', title])
    figure1 = plt.Figure(figsize=(11,5), dpi=145)
    ax1 = figure1.add_subplot(111)
    bar1 = FigureCanvasTkAgg(figure1, pencere)
    bar1.get_tk_widget().grid(row=0, column=3,sticky="nsew")
    df1 = df1[['Days', title]].groupby('Days').sum()
    df1.plot(kind=kind,color=color, legend=True, ax=ax1, fontsize=7)
    ax1.set_facecolor('#a5f1fd')
    ax1.set_title(title)
    cursor = mplcursors.cursor(ax1)
    cursor.connect("add", lambda sel: sel.annotation.set_text(
    ('%s : {} \n Days : {}'%(title)).format(int(sel.target[1]), int(sel.target[0])+1)))

def top_ten(global_data):
    for i in range (listbox_global.size()-1,-1,-1):
        if listbox_global.select_includes(i):
            target_type2 = listbox_global.get(i)

    if target_type2 == listbox_global.get(0):
        listbox_topten.delete(0, listbox_topten.size())
        global_data_confirmed = []
        counter = 0
        for i in global_data:
            global_data_confirmed.append(i['TotalConfirmed'])
            counter +=1
        global_data_confirmed = sorted(global_data_confirmed)
        test = []
        for i in range(counter-1, counter-11,-1):
            test.append(global_data_confirmed[i])
        top10_vaka = []
        sayaç = 0
        for i in range (0,10)  :
            for j in global_data:
                if test[i] == j['TotalConfirmed']:
                    top10_vaka.append(j['Country'])
                    sayaç +=1
            if sayaç == 10:
                break
        top10_vakaa = []
        for i in range(0,10):
            top10_vakaa.append('%s - %s'%(top10_vaka[i], test[i]))
            listbox_topten.insert(END,str(i+1)+'. ' + top10_vakaa[i])
    if target_type2 == listbox_global.get(1):
        listbox_topten.delete(0, listbox_topten.size())
        global_data_deaths = []
        counter = 0
        for i in global_data:
            global_data_deaths.append(i['TotalDeaths'])
            counter +=1
        global_data_deaths = sorted(global_data_deaths)
        test = []
        for i in range(counter-1, counter-11,-1):
            test.append(global_data_deaths[i])
        top10_ölüm = []
        sayaç = 0
        for i in range (0,10)  :
            for j in global_data:
                if test[i] == j['TotalDeaths']:
                    top10_ölüm.append(j['Country'])
                    sayaç +=1
            if sayaç == 10:
                break
        top10_ölümm = []
        for i in range(0,10):
            top10_ölümm.append('%s - %s'%(top10_ölüm[i], test[i]))
            listbox_topten.insert(END,str(i+1)+'. ' + top10_ölümm[i])

    if target_type2 == listbox_global.get(2):
        listbox_topten.delete(0, listbox_topten.size())
        global_data_recovered = []
        counter = 0
        for i in global_data:
            global_data_recovered.append(i['TotalRecovered'])
            counter +=1
        global_data_recovered = sorted(global_data_recovered)
        test = []
        for i in range(counter-1, counter-11,-1):
            test.append(global_data_recovered[i])
        top10_iyi = []
        sayaç = 0
        for i in range (0,10)  :
            for j in global_data:
                if test[i] == j['TotalRecovered']:
                    top10_iyi.append(j['Country'])
                    sayaç +=1
            if sayaç == 10:
                break
        top10_iyii = []
        for i in range(0,10):
            top10_iyii.append('%s - %s'%(top10_iyi[i], test[i]))
            listbox_topten.insert(END,str(i+1)+'. ' + top10_iyii[i])

    if target_type2 == listbox_global.get(3):
        listbox_topten.delete(0, listbox_topten.size())
        global_data_NC = []
        counter = 0
        for i in global_data:
            global_data_NC.append(i['NewConfirmed'])
            counter +=1
        global_data_NC = sorted(global_data_NC)
        test = []
        for i in range(counter-1, counter-11,-1):
            test.append(global_data_NC[i])
        top10_nc = []
        sayaç = 0
        for i in range (0,10)  :
            for j in global_data:
                if test[i] == j['NewConfirmed']:
                    top10_nc.append(j['Country'])
                    sayaç +=1
            if sayaç == 10:
                break
        top10_ncc = []
        for i in range(0,10):
            top10_ncc.append('%s - %s'%(top10_nc[i], test[i]))
            listbox_topten.insert(END,str(i+1)+'. ' + top10_ncc[i])

    if target_type2 == listbox_global.get(4):
        listbox_topten.delete(0, listbox_topten.size())
        global_data_ND = []
        counter = 0
        for i in global_data:
            global_data_ND.append(i['NewDeaths'])
            counter +=1
        global_data_ND = sorted(global_data_ND)
        test = []
        for i in range(counter-1, counter-11,-1):
            test.append(global_data_ND[i])
        top10_nd = []
        sayaç = 0
        for i in range (0,10)  :
            for j in global_data:
                if test[i] == j['NewDeaths']:
                    top10_nd.append(j['Country'])
                    sayaç +=1
            if sayaç == 10:
                break
        top10_ndd = []
        for i in range(0,10):
            top10_ndd.append('%s - %s'%(top10_nd[i], test[i]))
            listbox_topten.insert(END,str(i+1)+'. ' + top10_ndd[i])

    if target_type2 == listbox_global.get(5):
        listbox_topten.delete(0, listbox_topten.size())
        global_data_NR = []
        counter = 0
        for i in global_data:
            global_data_NR.append(i['NewRecovered'])
            counter +=1
        global_data_NR = sorted(global_data_NR)
        test = []
        for i in range(counter-1, counter-11,-1):
            test.append(global_data_NR[i])
        top10_nr = []
        sayaç = 0
        for i in range (0,10)  :
            for j in global_data:
                if test[i] == j['NewRecovered']:
                    top10_nr.append(j['Country'])
                    sayaç +=1
            if sayaç == 10:
                break
        top10_nrr = []
        for i in range(0,10):
            top10_nrr.append('%s - %s'%(top10_nr[i], test[i]))
            listbox_topten.insert(END,str(i+1)+'. ' + top10_nrr[i])

def user_guide():
    msgbox.showinfo(title='Basic Guide', message='Select a country and data type through first two list. \n'
                                                 'and you can show global top10 rank list through last list.\n'
                                                 'When you draw a graph you can easily click on bar or line to see values.\n'
                                                 'Use 1920x1080 resolution and full screen for best experience.'
                                                 '\n#StayAtHome')

status()
pencere = Tk()
pencere.title('Live Covid19')
pencere.iconbitmap(r'icon.ico')
frame1 = Frame(pencere, bg='#1EA4a3')
frame1.grid(row = 0, column = 0,sticky="nsew")
scrollb = Scrollbar(frame1, orient=VERTICAL)
lbl1 = Label(frame1, text='COUNTRIES', font='Calibri 16',bg='#1EA4A3')
lbl1.grid(row= 0, column = 0)


search = Entry(frame1, font = 'Arial 12',width=40,bg='#c3f6fe')
search.grid(row = 1, column = 0)
search_Button = Button(frame1, text='Search', font='Arial 10 ', command = searching, bg='#affde6',width='8')
search_Button.grid(row=1, column=1)
random_Button = Button(frame1, text='Random',font='Arial 10 ', command = random, bg='#affde6', width='8')
random_Button.grid(row = 3, column=1)


listbox = Listbox(frame1,font='Arial 12',relief=SUNKEN,border='4',exportselection=0,
                  selectmode='single',fg='black',width=40, height=15,bg='#c3f6fe')
listbox.grid(row = 3, column = 0)

scrollbar = Scrollbar(pencere, orient="vertical")
scrollbar.config(command=listbox.yview)
scrollbar.place(x=353, y=64, height=290)
listbox.config(yscrollcommand=scrollbar.set)


lbl2 = Label(frame1, text='Select data type for selected country', font='Calibri 12',bg='#1EA4A3')
lbl2.grid(row=4, column=0)
listbox_Type = Listbox(frame1, font='Arial 12', relief=SUNKEN, border='4', exportselection=0,
                       selectmode='single', fg='black', width='40',height='8',bg='#c3f6fe')
listbox_Type.grid(row=5, column=0)
draw_graph = Button(frame1, text='Draw Graph', font='Arial 10',command=draw_graph, bg='#affde6')
draw_graph.grid(row=5, column=1)

listbox_topten = Listbox(frame1, font='Arial 12', relief=SUNKEN, border='4', exportselection=0,
                       selectmode='single', fg='black', width='40',height='10',bg='#c3f6fe')
listbox_topten.grid(row = 8, column=0)

lbl3 = Label(frame1, text='Global Statistics', font = 'Arial 14 ', bg='#1EA4A3', fg='green')
lbl3.grid(row = 6, column=0)

listbox_last = Listbox(pencere, font='Arial 12',width='40',height='40',bg='#c3f6fe')
listbox_last.grid(row=0, column=4)

photo = PhotoImage(file='rules.gif')
lbl4 = Label(listbox_last, image=photo, width=625, height=900)
lbl4.grid(row= 0 , column=0)
photo2 = PhotoImage(fil='banner.png')
lbl5 = Label(pencere, image =photo2, width =1920, height = 120 )
lbl5.place(x=0, y=900)
Cou = requests.get('https://api.covid19api.com/summary')
Countries = Cou.json()
Deneme = Countries['Countries']
ulke = []
ulke_slug = []

#ulke_sayisi = 0
#try:
#    for i in range(0, 200):
#        ülke = Deneme[i]
#                                                      //Ülke sayacı , sonuc ulke_sayisi = 186
#        ulke_sayisi += 1
#except:
#    print('Ülke sayısı = %s'%(ulke_sayisi))

for i in range(0, 186):
    Ulkeler = Deneme[i]
    ulke.append(Ulkeler['Country'])
    ulke_slug.append(Ulkeler['Slug'])

for i in ulke:
    listbox.insert(END, i)

data_type = ['Confirmed', 'Deaths', 'Recovered', 'Active', 'Daily Confirmed', 'Daily Deaths',
             'Daily Recovered']
for i in data_type:
    listbox_Type.insert(END, i)

listbox_global = Listbox(frame1, font='Arial 12', relief=SUNKEN, border='5',
                         selectmode='single', fg='black', width='40', height='6', bg='#c3f6fe')
listbox_global.grid(row=7, column=0)
listbox_topten.bindtags((listbox, pencere, "all"))
global_test = requests.get('https://api.covid19api.com/summary')
global_test = global_test.json()
global_data = global_test['Countries']
global_test = global_test['Global']
NewConfirmed = global_test['NewConfirmed']
TotalConfirmed = global_test['TotalConfirmed']
NewDeaths = global_test['NewDeaths']
TotalDeaths = global_test['TotalDeaths']
NewRecovered= global_test['NewRecovered']
TotalRecovered = global_test['TotalRecovered']
stats = ['Total Confirmed : %s'%(TotalConfirmed),'Total Deaths : %s'%(TotalDeaths),
         'Total Recovered : %s'%(TotalRecovered),('New Confirmed : +%s'% (NewConfirmed)),
          'New Deaths : +%s'%(NewDeaths),'New Recovered : +%s'%(NewRecovered),
         ]
for i in stats:
    listbox_global.insert(END, i)
user_guide()
global_button = Button(frame1, text='Top 10 List', font='Arial 10 ',command=lambda: top_ten(global_data), bg='#affde6',
                       width='8')
global_button.grid(row=7, column=1)


pencere.mainloop()

