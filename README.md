# Active Cleanup - Automation
TL/DR - Unorganized AD accounts need some TLC: so I created all these files to organize them to my specifications (I'll leave out what exactly those are).

TLSR (Too long, still read):

I noticed a lot of accounts within our AD that were not following a good naming convention, blank descriptions, some accounts weren't part of the exact groups we needed them to be in. The main logic script (TODO name) currently relies on a CSV file exported from AD (future functionality will get accounts striaght from AD) to iterate through the list, get the necessary info and then store said information into an object for each account. 

The second script will then use info gathered from the main logic script (TODO name) to then create another array, storing anomalies. These "anomalies" can be anything you want them to be (granted you'll have to script that portion yourself since you're likely not going to need the exact checks that I have set for mine, plus I won't even tell you what those check are... security... ya' know?)

In the main logic script I will indicate where you need to change / add the logic to check for what you want. I repeat, you WILL NOT be able to just copy and paste this code into your own environment and expect it to run perfectly. You'll see a lot of my own psuedo code in there. Make sure you alter it to your own specifications. 

This is a work in progress, so don't be too harsh on my code. :) 
