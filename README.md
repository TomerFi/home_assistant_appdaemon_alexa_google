# home_assistant_appdaemon_alexa_google
AppDaemon application for Home Assistant, handles calls from either and Alexa Skill or a Google Action.

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
