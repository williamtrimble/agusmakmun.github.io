---
layout: post                                                    # (require) default post layout
title: "PDGA Website - Team DZ Discs Player Data"               # (require) a string title
date: 2018-10-24                                                # (require) a post date
---
Script download: [[Download]](https://github.com/williamtrimble/williamtrimble.github.io/blob/master/_data/PDGA_DataMine.py)


```python
#########################################
#Beautiful Soup DZ Team PDGA Data Mine  #
#                                       #
#Author: William Trimble                #
#Date: 5/17/2018                        #
#                                       #
#########################################

from bs4 import BeautifulSoup #pulls out pieces of web page
import requests #returns web page
from datetime import date
import time

#web page passed to beautiful soup
#epitome men
LauridsenB = r'https://www.pdga.com/player/85941'
PattonC = r'https://www.pdga.com/player/46797'
PopeD = r'https://www.pdga.com/player/57642'
BrowningT = r'https://www.pdga.com/player/88117'
AffolterS = r'https://www.pdga.com/player/85187'
MartinT = r'https://www.pdga.com/player/88919'
HerringtonD = r'https://www.pdga.com/player/108327'
#epitome women
KephartBH = r'https://www.pdga.com/player/94101'
BowmanEB = r'https://www.pdga.com/player/67567'
KincaidK = r'https://www.pdga.com/player/40388'
#paradigm men
AllenL = r'https://www.pdga.com/player/29193'
BakerN = r'https://www.pdga.com/player/86443'
BeachI = r'https://www.pdga.com/player/92669'
ColemanH = r'https://www.pdga.com/player/21416'
CyganB = r'https://www.pdga.com/player/94782'
DavisJ = r'https://www.pdga.com/player/85822'
HorstT = r'https://www.pdga.com/player/90667'
MendezB = r'https://www.pdga.com/player/79257'
MeyersD = r'https://www.pdga.com/player/60616'
OwenZ = r'https://www.pdga.com/player/21462'
RameyJ = r'https://www.pdga.com/player/58858'
TrimbleW = r'https://www.pdga.com/player/71368'
HensonD = r'https://www.pdga.com/player/102249'
#prototype men
BestJ = r'https://www.pdga.com/player/107182'
ClarkD = r'https://www.pdga.com/player/100970'
DanaR = r'https://www.pdga.com/player/93510'
DonleyE = r'https://www.pdga.com/player/108058'
LariviereT = r'https://www.pdga.com/player/95000'
MorrowG = r'https://www.pdga.com/player/98138'
NguyenD = r'https://www.pdga.com/player/83747'
RahmigJ = r'https://www.pdga.com/player/67746'
RingstaffL = r'https://www.pdga.com/player/72074'
RussellJ = r'https://www.pdga.com/player/101466'
SmithR = r'https://www.pdga.com/player/78847'
ZuelkeE = r'https://www.pdga.com/player/108178'
DonleyJ = r'https://www.pdga.com/player/94009'
FernandezJ = r'https://www.pdga.com/player/81329'
TriaM = r'https://www.pdga.com/player/78916'
ZernickowR = r'https://www.pdga.com/player/111763'
WelshDJ = r'https://www.pdga.com/player/100726'
GreerH = r'https://www.pdga.com/player/70805'
#staff men
FarrisK = r'https://www.pdga.com/player/107655'
AdamsG = r'https://www.pdga.com/player/89521'
StaufferC = r'https://www.pdga.com/player/96362'

#function that does all the work
def pageresults(page):
  headers = {'User-agent':'Mozilla/5.0'}                # use to imitate request from browser so script doesn’t get blocked
  response = requests.get(page,headers=headers)         # all the html
  APs = response.content
  soupAP = BeautifulSoup(APs,'html.parser')             # tell BS that this is html content

  # Find player name
  player = soupAP.find("h1",{"class":"title"})
  # Find recent events
  recentevents = soupAP.find("li",{"class":"recent-events"})
  # Check if recent event is none, if not continue to event page
  if (recentevents != None ):
    eventhtml = recentevents.find_next("a")
    eventpage = "https://www.pdga.com" + eventhtml["href"]
    # Print player name
    print ("###########################")
    print (player.string)
    print ("###########################")
    # Print event name
    print (eventhtml.string)

    # Move soup to event page html
    headers = {'User-agent':'Mozilla/5.0'}              # use to imitate request from browser so script doesn’t get blocked
    response = requests.get(eventpage,headers=headers)  # all the html
    APs = response.content
    soupAP = BeautifulSoup(APs,'html.parser')           # tell BS that this is html content
    # Print date
    date = soupAP.find("li",{"class":"tournament-date"})
    print (date.next_element.next_element + date.next_element.next_element.next_element)
    # Check if player has nickname to set search name
    namenumber = player.string
    nicknameidiots = namenumber.split(" ")
    if len(nicknameidiots) == 3:
      name = namenumber.split(" ")[0] + " " + namenumber.split(" ")[1]
    else:
      name = namenumber.split(" ")[0] + " " + namenumber.split(" ")[1]+ " " + namenumber.split(" ")[2]
            
    result = soupAP.find(string = name)
    # Find and print division
    division = result.find_previous("h3")
    print (division.contents[0].string + division.contents[1].string)
    # Find and print place
    place = result.find_previous("tr")
    print ("Place: " + place.contents[0].string)
    #print ("Place: " + result.previous_element.previous_element.previous_element)
    print ("\n")
  else:
    # Print player name
    print ("###########################")
    print (player.string)
    print ("###########################")
    # Print none
    print (recentevents)
    print ("\n")


  headers = {'User-agent':'Mozilla/5.0'}                # use to imitate request from browser so script doesn’t get blocked
  response = requests.get(page,headers=headers)         # all the html
  APs = response.content
  soupAP = BeautifulSoup(APs,'html.parser')             # tell BS that this is html content

  tournaments = soupAP.find(string = "2018 Tournament Results")
  if (tournaments != None):
    division2 = tournaments.find_next("h4")
    while (division2 != None):
      print ("Division: " + division2.contents[0])
      print ("----------------------------------")
      events2 = division2.find_next("tbody")
      divEvent= events2.find_next("tr")
      while (divEvent != None):
        #print ("Place: " + events2.contents[0].string + " Event: " + events2.contents[2].string + " Tier: " + events2.contents[3].string + " Dates: " + events2.contents[4].string)
        print ("Event: " + divEvent.contents[2].string)
        print ("Tier: " + divEvent.contents[3].string)
        print ("Dates: " + divEvent.contents[4].string)
        print ("Place: " + divEvent.contents[0].string)
        print ("\n")
        divEvent = divEvent.next_sibling
        divEvent = divEvent.next_sibling
      division2 = division2.find_next("h4")

  time.sleep(1)   # wait one second before running another request so you don’t get blocked

#epitome men
print ("=====================")
print ("Epitome Men")
print ("=====================")
print ("")
pageresults(LauridsenB)
pageresults(PattonC)
pageresults(PopeD)
#pageresults(BrowningT)
pageresults(AffolterS)
pageresults(MartinT)
pageresults(HerringtonD)
#epitome women
print ("=====================")
print ("Epitome Women")
print ("=====================")
print ("")
pageresults(KephartBH)
pageresults(BowmanEB)
pageresults(KincaidK)
#paradigm men
print ("=====================")
print ("Paradigm Men")
print ("=====================")
print ("")
pageresults(AllenL)
pageresults(BakerN)
pageresults(BeachI)
pageresults(ColemanH)
pageresults(CyganB)
pageresults(DavisJ)
pageresults(HorstT)
pageresults(MendezB)
pageresults(MeyersD)
pageresults(OwenZ)
pageresults(RameyJ)
pageresults(TrimbleW)
pageresults(HensonD)
#prototype men
print ("=====================")
print ("Prototype Men")
print ("=====================")
print ("")
pageresults(BestJ)
pageresults(ClarkD)
pageresults(DanaR)
pageresults(DonleyE)
pageresults(LariviereT)
pageresults(MorrowG)
pageresults(NguyenD)
pageresults(RahmigJ)
pageresults(RingstaffL)
pageresults(RussellJ)
pageresults(SmithR)
pageresults(ZuelkeE)
pageresults(DonleyJ)
pageresults(FernandezJ)
pageresults(TriaM)
pageresults(ZernickowR)
pageresults(WelshDJ)
pageresults(GreerH)
#staff men
print ("=====================")
print ("Staff Men")
print ("=====================")
print ("")
pageresults(FarrisK)
pageresults(AdamsG)
pageresults(StaufferC)
```
