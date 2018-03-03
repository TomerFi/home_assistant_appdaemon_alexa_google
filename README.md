# AppDaemon application for Home Assistant, Handles calls from an Alexa Skill and a Google Action
AppDaemon application for Home Assistant, handles calls from either and Alexa Skill or a Google Action.</br>

**Please note:** The functionality of this application is design to suit me, it won't necessarily fit others and you might need to do python code writing and designing to make it work for you.</br>
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
