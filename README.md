# AppDaemon application for Home Assistant, Handles calls from Alexa Skill and Google Action</br>![Maintenance](https://img.shields.io/maintenance/no/2019.svg)

**NOT MAINTAINED!**

I'm no longer maintaining this repository!</br>

You can still use the files :point_up:, if you want.</br>
These :point_down: are the instructions.

__________________________________________

AppDaemon application for Home Assistant, handles calls from either and Alexa Skill or a Google Action.</br>
You can see the application in action in the following videos on my [youtube channel](https://www.youtube.com/channel/UCH9z4dabjTo-pRqM3_i5RTg?view_as=subscriber):
- [Multi step interaction with Alexa part 1](https://www.youtube.com/watch?v=g7Zq0ZpjCU0)
- [Multi step interaction with Alexa part 2](https://www.youtube.com/watch?v=1pNSTCa0X_k)
- [Multi step interaction with Google](https://www.youtube.com/watch?v=ucb5h4LkcNk&)

**Please note:** The functionality of this application is design to suit my own needs, it won't necessarily fit others and you might need to do some python code writing and designing to make it work for you.</br>
For example, the application collects data about my Qnap NAS and reads it back to me, you might not have a Qnap NAS yourself or you might not use its designated Home Assistant component, so you don't actually need that part of the application.</br>
Nevertheless, the infrastructure and flow will still be eligible, just edit the intents and the intent handlers to suit your own needs.</br>

## Prerequisites
- [Home Assistant](https://home-assistant.io/). The application has been tested with HassIO version 0.63.2
- [AppDaemon](http://appdaemon.readthedocs.io/en/stable/index.html), Since I'm using HassIO, I actually use the [AppDaemon v2 Hassio Addon](https://github.com/hassio-addons/addon-appdaemon) made by frenck. The application has been tested with version 1.0.0 of the addon.
- Expose your environment securely to the web so that it is accessible for Amazon and Google servers. There are a number of ways to accomplish that. I use the [Caddy Proxy Hassio Addon](https://github.com/bestlibre/hassio-addons/tree/master/caddy_proxy) made by bestlibre to handle my certificates and do a dns based internal routing: Calls from my DuckDns name is redirected to Home Assistant so that I can access it from outside my lan, my No-IP name is redirected to the AppDaemon instance.</br>
I also use the [DuckDns](https://home-assistant.io/components/duckdns/) and the [No-IP](https://home-assistant.io/components/no_ip/) Home Assistant components to update my ip address.</br>
On my router side, I've create a port-forward for the port 443 towards my Home Assistant environment.</br>

## AppDaemon Configuration
- In [appdaemon.yaml](/appdaemon.yaml), fill in your Home Assistant and AppDaemon information such as url address, port and password.</br>
  You probably already did it when you've installed AppDaemon, so there's no actual need to change anything.
- In [apps.yaml](/apps.yaml), configure the following applications:
```yaml
home_control_google:
  module: home_control_google
  class: HomeControl

home_control_alexa:
  module: home_control_alexa
  class: HomeControl
```
- Copy the following py files to your [apps directory](/apps) in AppDaemon:
  - [home_control.py](/apps/home_control.py) is not actually an AppDaemon application, it's a generic script used by both applications.
  - [home_control_alexa.py](/apps/home_control_alexa.py) is the Application for handling the Alexa Skill requests.
  - [home_control_google.py](/apps/home_control_google.py) is the Application for handling the Google Action requests.

## Application Analysis and Configuration
In [home_control.py](/apps/home_control.py) the following variables contains data to be used by the intent handlers:
- *start_phrases*: A list of start phrases randomly incorporated into the first response of the session.
- *end_phrases*: A list of end phrases randomly incorporated into the last response of the session.
- *hava_profile*: A dictionary for holding the entity names to use with HA to update the current location, get current location and issue an iCloud find request, when I ask the application to locate Hava (my wife).
- *tomer_profile*: A dictionary doing the same as the above, holding the information for Tomer (myself).
- *door_sensors*: A list of the door sensors for the application to check when I ask for a sensors report, the application will read back the ones in Open state.
- *system_sensors*: A dictionary holding the entity id's of 5 specific system sensors, based on their states the application will construct a response informing me of the system overall status when I ask for the system information.
- *devices_sensors*: A list of the entity id's for my nmap trackers, based on this list the application will read back to names of the sensors in Offline state when I ask for the devices information.
- *network_profile*: A dictionary holding the entity id's of 6 specific network sensors, based on their states the application will construct a response informing me of the network overall status when I ask for the network information.
- *storage_profile*: A dictionary holding the entity id's of 8 specific qnap nas sensors, based on their states the application will construct a response informing me of the nas overall status when I ask for the storage information.
- *water_heater_switch*: A string containing the entity id of my Switcher V2 boiler for the application to use when I ask it to turn on the boiler for X minutes/seconds.

### Restricted Access
As an added measure of security, I've added a couple of lists to control the users, application and devices allowed to make calls to the application.</br>
In [home_control.py](/apps/home_control.py) scroll down the bottom where you can see the last two functions and update your id's into the lists, you can also leave it empty for no restricted access:</br>
```python
def authorizeAlexaUse(api, application_id, device_id, user_id):
```
The lists are:
- *applications*
- *devices*
- *users*
```python
def authorizeGoogleUse(api, user_id):
```
One list named *users*.</br>

## Create the Alexa Skill
- Create an Alexa custom skill, if you're not familiar with doing so, [this](https://developer.amazon.com/docs/custom-skills/steps-to-build-a-custom-skill.html) guide can help you.
- Point the created custom skill to the url in the [endpoint_https_address](/config_alexa_skill/endpoint_https_address.txt) text file, just make sure to edit the url with your dns name and ad password.
- Update the interaction model based on the [interaction_model](/config_alexa_skill/interaction_model.json) json file.</br>

## Create the Google Action
- Create the project and the action, if you're not familiar doing so, [this](https://developers.google.com/actions/dialogflow/first-app) guide can help you.
- Edit the [agent](/config_google_action/dialogflow_agent/agent.json) json file, scroll down to the bottom and edit the url with your dns name and ad password.
- Zip the entire content of the [dialogflow_agent folder](/config_google_action/dialogflow_agent) and create the Dialogflow agent from it.
