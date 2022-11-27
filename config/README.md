# The Loft Home Assistant Style Guide

### Directory Naming Conventions

- Directory names should be in lower case.
- Directory names should be prefixed with the `the-loft` prefix.


### Automation Yaml Conventions

- Automations should at minimum include an id, alias, description, trigger, action and mode attributes.
- Automation id is composed of a combination of the alias in Pascal Case.
- Automation alias should include the `room`, `device/trigger`, `action` seperated by a `/`. This is useful for identifying automations on per room or per device basis. 
- Automation description should include a human readable description of the automation. Nothing too complicates here just a simple description to understand the automation when iterating over the automation.

#### Sample Yaml Automation

```yaml
######################################################################################################
# @Automation
# @Room: Nursery
# @Alias: Nursery / Vacuum / Finish House / Notification
######################################################################################################
- id: 'NurseryVacuumFinishHouseNotification'
  alias: 'Nursery / Vacuum / Finish House / Notification'
  description: 'Sends a notification upon finishing cleaning and being docked for 15 minutes.'

  #trigger
  trigger:
    #Vacuum From Cleaning to Docked for 15 minutes
    - platform: state
      entity_id: vacuum.rockrobo_vacuum_v1
      from: 'cleaning'
      to: 'docked'
      for: '00:15:00'

  #condition
  condition:
    #The Loft Members Away
    - condition: state
      entity_id: 'group.the_loft_members'
      state: 'not_home'

  #action
  action:
    # Notify Companion Apps
    - service: script.notify_base_app
      data:
        title: 'The Loft Notification'
        message: 'Nunu has finished Cleaning. Nunu has returned back to Dock.'

  mode: single
```