# Events

Players can trigger various events at each other's colonies by selecting a settlement on the world map and paying a fee determined by the server owner.

## Configuration

The server owner can set the silver cost for all events and add references to modded ones through the "Events" folder of the server.

To modify an existing event, just open the event file with the name of the event you want to modify, there you'll have access to toggling it and modifying its values.

To add new events, you'll have to investigate around the mod that adds the event you want to use, and then create a new file using the same structure of the other events, and introduce all the necessary details (event name, defName, availability and price) into it. If everything is alright it should show as a loaded event on the server.

```C#
{
  "Name": "Event Name",
  "DefName": "Event DefName",
  "Cost": 500,
  "IsEnabled": true
}
```

## How to Use

1. **Select Settlement**: From the world map, select the settlement you wish to target.
2. **Trigger Event**: Click the `Event` button and choose the event you wish to invoke.

## Important Notes

- **Cooldown Period**: If the server has the setting enabled, there is a 1-hour cooldown before the same player can be targeted again. This can be adjusted in the `ServerConfig.json` file inside the `Core` folder.
- **Silver Requirement**: The player triggering the event must carry enough silver to cover the event fee.
- **Target Must Be Online**: The target player must be online for the event to be triggered.

## Troubleshooting

For additional troubleshooting assistance, please join our [Discord Server](https://discord.gg/yUF2ec8Vt8).
