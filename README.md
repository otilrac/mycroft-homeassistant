# Home Assistant Skill for Mycroft

[![Stories in Ready](https://badge.waffle.io/btotharye/mycroft-homeassistant.svg?label=ready&title=Ready)](http://waffle.io/btotharye/mycroft-homeassistant) 
[![Build Status](https://travis-ci.org/btotharye/mycroft-homeassistant.svg?branch=master)](https://travis-ci.org/btotharye/mycroft-homeassistant)
[![Coverage Status](https://coveralls.io/repos/github/btotharye/mycroft-homeassistant/badge.svg?branch=master)](https://coveralls.io/github/btotharye/mycroft-homeassistant?branch=master)
[![Discord](https://img.shields.io/discord/348442860510642176.svg)](https://discord.gg/Xnn89dB)



based off the original code from https://github.com/BongoEADGC6/mycroft-home-assistant, spun off my own version since they seem to be inactive.

See https://geekedoutsolutions.com/using-home-assistant-with-mycroft-ai-for-home-automation for a general overview of Mycroft AI and Home Assistant

# Updates
Now confirmed working for 18.2.6 beta.  Make sure you have your home assistant settings filled out on home.mycroft.ai.

Testrunner tests are now setup as well to test intents.  Currently have PR https://github.com/MycroftAI/mycroft-skills/pull/363 in place to add skill as official skill for 18.02 branch.


This is a skill to add [Home Assistant](https://home-assistant.io) support to
[Mycroft](https://mycroft.ai). Currently is supports turning on and off several
entity types (`light`, `switch`, `scene`, `groups` and `input_boolean`).

## Support/Help
You can always find me in the mycroft mattermost channel here, https://chat.mycroft.ai

If you want to join my official discord where I will have a chatbot helper and more you can join via https://discord.gg/Xnn89dB

If you like this and want to say thanks check out my [Patreon](https://www.patreon.com/Geekedoutsol) I also include special posts here for my Patrons.

## Donate

If you liked this project, you can donate to support it and new features and also help me justify spending more time on this project.

You can donate directly via my [Blog](https://geekedoutsolutions.com/donations/geeked-out-donation) or any of the other options listed below.

[![paypal](https://www.paypalobjects.com/en_US/i/btn/btn_donateCC_LG.gif)](https://www.paypal.com/cgi-bin/webscr?cmd=_donations&business=brianhh1230%40gmail%2ecom&lc=US&item_name=Geeked%20Out%20Solutions&no_note=0&cn=Add%20special%20instructions%20to%20the%20seller%3a&no_shipping=1&currency_code=USD&bn=PP%2dDonationsBF%3abtn_donateCC_LG%2egif%3aNonHosted)

Or become a [Patreon](https://www.patreon.com/Geekedoutsol) - I offer special posts and tutorials to my Patreons that aren't public.

## Installation
Before installation ensure you have python-dev package installed for your OS.  For debian this would be `apt-get install python-dev` it is required for the levenstein package.


Should be able to install this now via just saying `Hey Mycroft, install skill home assistant` it will then confirm if you want to install it and say yes and you are good to go.

Can also be installed via `msm install https://github.com/btotharye/mycroft-homeassistant.git`


## Demo Home Assistant
You can see a demo of Home Assistant at http://167.99.144.205:8123 using the password "password".  You can also plug these settings into the home.mycroft.ai skill setting mentioned below to test your mycroft instance with the demo instance of home assistant.

## Configuration
This skill utilizes the skillsettings.json file which allows you to configure this skill via home.mycroft.ai after a few minutes of having the skill installed you should see something like below in the https://home.mycroft.ai/#/skill location:

Fill this out with your appropriate home assistant information and hit save.

![Screenshot](screenshot.JPG?raw=true)

## Usage

Say something like "Hey Mycroft, turn on living room lights". Currently available commands
are "turn on" and "turn off". Matching to Home Assistant entity names is done by scanning
the HA API and looking for the closest matching friendly name. The matching is fuzzy (thanks
to the `fuzzywuzzy` module) so it should find the right entity most of the time, even if Mycroft
didn't quite get what you said.  I have further expanded this to also look at groups as well as lights.  This way if you say turn on the office light, it will do the group and not just 1 light, this can easily be modified to your preference by just removing group's from the fuzzy logic in the code.


Example Code:
So in the code in this section you can just remove group, etc to your liking for the lighting.  I will eventually set this up as variables you set in your config file soon.

```
def handle_lighting_intent(self, message):
        entity = message.data["Entity"]
        action = message.data["Action"]
        LOGGER.debug("Entity: %s" % entity)
        LOGGER.debug("Action: %s" % action)
        ha_entity = self.ha.find_entity(entity, ['group','light', 'switch', 'scene', 'input_boolean'])
        if ha_entity is None:
            #self.speak("Sorry, I can't find the Home Assistant entity %s" % entity)
            self.speak_dialog('homeassistant.device.unknown', data={"dev_name": ha_entity['dev_name']})
            return
        ha_data = {'entity_id': ha_entity['id']}
```


## Supported Phrases/Entities
Currently the phrases are:
* Hey Mycroft, turn on office (turn on the group office)
* Hey Mycroft, turn on office light (to turn on the light named office)
* Hey Mycroft, activate Bedtime (Bedtime is an automation)
* Hey Mycroft, turn on Movietime (Movietime is a scene)
* Hey Mycroft, status of thermostat (For sensors in homeassistant)
* Hey Mycroft, locate/where brian (Brian is a device tracker object)
* Hey Mycroft, what is the current living room temp
* Hey Mycroft, what is the current season



## TODO
 * Script intents processing
 * New intent for opening/closing cover entities
 * New intent for locking/unlocking lock entities (with added security?)
 * New intent for thermostat values, raising, etc.
 * New intent to handle multimedia/kodi

## In Development
* Climate and Weather intents

## Contributing

All contributions welcome:

 * Fork
 * Write code
 * Submit merge request

## Licence

See [`LICENCE`](https://gitlab.com/robconnolly/mycroft-home-assistant/blob/master/LICENSE).
