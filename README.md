# [brutal.io](https://brutal.io) info

In this repo, you will find  a lot of my research on the game [brutal.io](https://brutal.io), including packet information and structure.
## Clientbound Packets

| Name| Description |
| ----------- | ----------- |
| b5| Leaderboard Packet|
| b4| Update Packet|

| Name| Structure|
| ----------- | ----------- |
| b5|```u8 header```, ```u16 id```, ```u32 score```, ```u16 list of characters of a name, terminated by a null value```,```repeat until the id is null```|
| b4| ```u8 header```, ```u16 entity id [if this value is null, we break out of the loop]```, ```u8 entity flag``` ```[entity flag == 0: the game then updates the network with "h.updateNetwork(dataView, at, !1, packet)][entity flag == 1: a new entity was created][entity flag == 2: an entity was deleted]```|

## Usage

```python
import foobar

foobar.pluralize('word') # returns 'words'
foobar.pluralize('goose') # returns 'geese'
foobar.singularize('phenomena') # returns 'phenomenon'
```

## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

Please make sure to update tests as appropriate.

## License
[MIT](https://choosealicense.com/licenses/mit/)
