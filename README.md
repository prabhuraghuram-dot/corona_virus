# Coronavirus App

This is a set of dashboards for analyzing the Corona Virus using Splunk. 

Contributors to this app: Ryan O'Connor, Miranda Luna, Caleb Dyck, Anthony Barbato, Giovanni Mola

<em>The Splunk Corona Virus dashboard provided in this GitHub repo is an informational tool provided by Splunk without charge to all those who are working to understand and combat COVID-19.  The dashboard is intended for informational purposes only and relies entirely on data provided by various third-parties including, inter alia, Johns Hopkins University and any information entered by the user. [https://github.com/CSSEGISandData/COVID-19](https://github.com/CSSEGISandData/COVID-19). This dashboard is not for commercial use and is intended and should be used to provide background and context on the evolving COVID-19 situation.  Splunk disclaims any and all representations and warranties with respect to the dashboard, including accuracy, fitness for use and merchantability.</em>

## Installing the App

This app should be installed directly into $SPLUNK_HOME/etc/apps. You simply clone the app directly into that directory and it will be self-contained. Please see the install instructions below. 

**You must use the git clone method for this app to work properly. See [Cloning this App](#cloning-this-app)**

### App Requirements

* This app currently is supported on Linux only. 

* For the Confirmed Cases/Locations Overlay dashboard to load optimally, please ensure you have the [Maps+ App installed from Splunkbase](https://splunkbase.splunk.com/app/3124/). 

* Please also ensure you are installing using the git clone method below. 

### Cloning this App

This package depends on a submodule from here: https://github.com/CSSEGISandData/COVID-19 which is the main source of data for the Coronavirus. As a result, when you run git [clone](https://help.github.com/en/github/creating-cloning-and-archiving-repositories/cloning-a-repository), please add the --recurse-submodules parameter after the clone. So for example:

`git clone --recurse-submodules https://github.com/splunk/corona_virus.git`

This will ensure the required submodule is cloned into the correct directory inside of the app. Once you have cloned the app, please restart Splunk. 

## Dashboard Information

1. Coronavirus 
    1. This is a static analysis of the Coronavirus. 
1. covid-19 Patterns & Trends
    1. This is the same dashboard as the one publicly available on https://covid-19.splunkforgood.com 
1. Coronavirus - Timelapse
    1. This is a timelapse of the Coronavirus from the first day it was detected, until the current day. 
1. Confirmed Cases/Locations Overlay
    1. This is a dashboard that can be used to overlay locations of your choosing, with confirmed cases of COVID-19. By default, we are simply using U.S. State Capitals as an example. But you can choose to modify locations.csv to fit your own purposes. 
    
## Lookup Table Updating

There is a scripted input inside of this app that is **enabled by default**. It can be found in the GUI by going to Settings > Data Inputs > Scripts and enabling the input "update_git.sh". 

This scripted input will send it's output by default to `index=main sourcetype=git_update_corona`. You can use this index/sourcetype to find out when the latest update to the Coronavirus git repository took place. 

A search to find out when the last time the JHU Git Repository was updated would look like the following:

```
index=main sourcetype=git_update_corona _raw!="*Already up to date.*" 
| head 1 
| eval time=strftime(_time,"%m/%d/%Y %H:%M:%S") 
| table time
```

An example update would look like this:

```
2020-03-09 20:59:32	Entering 'git/COVID-19'

Updating 382bda4..473681f
Fast-forward
 .../time_series_19-covid-Confirmed.csv             | 541 ++++++++++-----------
 .../time_series_19-covid-Deaths.csv                | 541 ++++++++++-----------
 .../time_series_19-covid-Recovered.csv             | 541 ++++++++++-----------
 3 files changed, 801 insertions(+), 822 deletions(-)
 ```
## Change Record
### 3/19/20
* Merged in the public Splunk Dashboards Beta into the app. So people downloading this app can look at both Simple XML Dashboards, as well as the new dashboards beta dashboard.
* Fixed a couple bugs to make the main Simple XML Dashboard Mobile Friendly
* Turned the scripted input on by default to make setup easier for everyome
