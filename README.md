# [brutal.io](https://brutal.io) info

In this repo, you will find a lot of my research on the game [brutal.io](https://brutal.io), including packet information and structure.

## Encoding Types

| Name    | Description               |  Length (bytes) | Shorthand |
|---------|---------------------------|-----------------|-----------|
| uint8   | a unsigned 8 bit integer  | 1               | u8        |
| uint16  | a unsigned 16 bit integer | 2               | u16       |
| uint32  | a unsigned 32 bit integer | 4               | u32       |
| float32 | a floating point number   | 4               | f32       |
| float64 | a floating point number   | 8               | f64       |
| ...     | a list of values          | 2+              | ...       |

# Packets

# Outgoing (serverbound)

| Packet Name | Description                                                                              | Status   |
|-------------|------------------------------------------------------------------------------------------|----------|
| 0x00        | a single byte, sent by the client when it is ready, in the game code also used as a ping | complete |
| 0x01        | contains screen dimensions of the client                                                 | complete |
| 0x03        | spawn packet, contains name of the player                                                | complete |
| 0x04        | leave packet                                                                             | complete |
| 0x05        | input packet                                                                             | complete |
| 0x07        | resize packet                                                                            | complete |
| 0x08        | click packet                                                                             | complete |

#

# 0x00
## Description

A single byte that is sent by the client when it is ready to start receiving packets from the server. It is also be used as a ping packet.

## Sample

```00```

#

# 0x01
## Description

A 5 byte packet containing screen dimensions of the client. This packet is sent just before the `0x00` packet. 

## Sample

```01 7E 00 61 00```

## Structure
```u8(header) u16(screen width / 10) u16(screen height / 10)```
#

# 0x03
## Description

This is the spawn packet, sent when a player wants to spawn into the game. The length of this packet is dependent on the name length, which is capped at 15 characters. It is always 3 bytes plus double the names character count. The character list is terminated by a null value.

## Sample

```03 74 00 65 00 73 00 74 00 69 00 6E 00 67 00 31 00 32 00 33 00 00 00```

## Structure
```u8(header) ...u16(characters of name, continues until a null value is read)```
#

# 0x04
## Description

A single byte, sent to the server when the client leaves the game. 

## Sample

```04```

#

# 0x05
## Description

This is the main input packet. Contains the player/mouse angle, along with other flags regarding input.
```
Flags

0 : nothing
1 : throttle
2 : screen is unfocussed
```
If you were to have throttle enabled, and have the screen unfocussed, you would have a flags value of `3`
## Sample

```05 40 63 14 01 0E 28 F0 3F 03```

## Structure
```u8(header) f64(player/mouse angle) u8(flags)```
#

# 0x07
## Description

This packet is sent when the client resizes the window, structured the same as the `0x01` packet

## Sample

```07 7E 00 61 00```

## Structure
```u8(header) u16(screen width / 10) u16(screen height / 10)```
#

# 0x08
## Description

A simple packet that is always 2 bytes. This is sent whenever the client clicks, or unclicks.

## Sample

```08 01```

## Structure
```u8(header) u8(clicking boolean)```
#


# Incoming (clientbound)



## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

## License
[MIT](https://choosealicense.com/licenses/mit/)
