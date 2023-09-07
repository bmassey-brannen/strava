# strava
**google apps script used to pull Strava data into google sheet
**

[Link to Google Sheet]([url](https://docs.google.com/spreadsheets/d/1v72srsmcXz53l7tl6S5SM58R46OXxgX9wQQoez2ZHRc/edit?usp=sharing)https://docs.google.com/spreadsheets/d/1v72srsmcXz53l7tl6S5SM58R46OXxgX9wQQoez2ZHRc/edit?usp=sharing) 

[Link to Dashboard
]([url](https://lookerstudio.google.com/u/0/reporting/29998d2b-7d6e-4f42-b4e7-cfa489bff4a9/page/p_24y1t5ln9c))

**Setting up the Strava API**
1. go to https://www.strava.com/settings/api
2. enter your website information and name the app. I have Data importer as the category
3. run the function doGet(e) script to create your apps script web app. use the url from that page in step 4
4. enter the website https://script.google.com/macros/s/<UNIQUE CODE FROM Do(e) Web App)
5. callback domain = script.google.com
6. run authorizeWithStrava() and click authorize

**Google Sheets Formulas I've added:**
  1. 100 Day Journey? =IF(A2<>"",IF(AK2<>"No","Yes","No"),"")
     I ran a 5k everyday for 100 days, I wanted a field to act as a filter for those runs. based on "Day"
     
  3. Day =IF(A2<>"",IFERROR(index('Data Validation'!$B$2:$B$103,match($H2,'Data         
    Validation'!$A$2:$A$103,0)),"No"),"")
    My Data Validation page has a list of all the runs in the 100 day journey, I index (m,m) those into this       data
     
  5. Date =IF(A2<>"", DATEVALUE(LEFT(J2,10)), "")
    The start date field from strava is too long, I just wanted short date here
    
     
  7. Mileage =IF(H2=8890934382,3.1,IF(B2<>"",B2/1609.34,""))
     Convert meters to miles 
     
  9. Moving Time
      =IF(A2<>"",
      IF(H2=9364758484, "29m 50s",
          IF(QUOTIENT(C2, 3600) > 0, QUOTIENT(C2, 3600) & "h ", "") & 
          QUOTIENT(MOD(C2, 3600), 60) & "m " & 
          MOD(C2, 60) & "s"), 
      "")
  
     I wanted a text field to show xH xM xS to include in the tables
  
  
  
  12. Year =IF(A2<>"", YEAR(DATEVALUE(LEFT(J2,10))), "")
    I wanted a roll up field for year because looker studio wasn't doing what I wanted it to 
  
  
  13. moving_#
    =IF(A2<>"",
      IF(H2=9364758484, "00:29:50",
          TEXT(QUOTIENT(C2, 3600), "00") & ":" & 
          TEXT(QUOTIENT(MOD(C2, 3600), 60), "00") & ":" & 
          TEXT(MOD(C2, 60), "00")), 
    "")
  
  Similar to Moving Time, but it is 00:00:00 format



**Trouble Shooting**
I've included some logs that helped me along the way to trouble shoot. you can remove these in the code. 

Additionally, I have separated out the start lat / longitude so that I could build a map. in the code it separate the array into two columns. In my google sheet I concatenated these into lat,long so that I have one field in looker studio. you could play with this so that the output is in one column, but it provides the same results. 


I think that is it, I set up a trigger on fetchStravaData() to run every six hours so the data updates automatically
