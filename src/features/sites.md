## Creating Sites

With this mod, players can create customizable sites on the world map to enhance their gameplay in RimWorld. Sites are structures that provide valuable resources over time.

### To create a site:

1. Move a caravan to a free space on the map.
2. Use the "Site" button to start the creation process.
3. Pick a site.

### To configure a site:
1. Move a caravan to a site.
2. Use the "Configure" button to start the configuration process.
3. Pick a site and reward.

Sites configurations affect all your rewards for that type of sites, not just one. Players can however have different configs in between them.

### Site Ranks:

Sites are owned by the player that creates them and are independant of factions. They will however benefit every member of the owner's current faction.

### Site Types and Resources:

Sites can be configured to give any item to the players. We'll go over every field in the configuration file (SiteValues.json)
1. TimeIntervalMinute: The amount of minutes between rewards.
2. DefName: Defname of the site. Do not touch except if you know what you are doing.
3. OverrideName: Leave empty for default. Change if you want to change the name of the site to better reflect its rewards.
4. OverrideDescription: Leave empty for default. Change if you want to change the description of the site to better reflect its rewards.
5. DefNameCost: Defname of the item required to purchase this site. A defname is the true name of the item, like showcased in the devmode menu "spawn thing"
6. Cost: Amount of the defname listed above required to purchase the site.
7. Rewards: A reward the client can pick. Clients can only pick one.
8. RewardDef: Defname of the item given to the player
9. RewardAmount: Amount of the reward given to the player.

## Troubleshooting
1. Make sure your defnames are correct.
2. Make sure every reward has an amount.
3. Make sure that the DefNameCost and Cost are the same lenght
   
For additional troubleshooting assistance, please join our [Discord](https://discord.gg/yUF2ec8Vt8) server.
