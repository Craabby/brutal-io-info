# [brutal.io](https://brutal.io) info
---
In this repo, you will find a lot of my research on the game [brutal.io](https://brutal.io), including packet information and structure.

* please note I have not completely finished this and some information might and very well could be wrong. I lost interest in the game a little while ago, but felt I should share my findings for others trying to learn. 

*whatever you have done with this information was done by yourself, I am just sharing my findings*

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
---
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

---

# 0x00
## Description

A single byte that is sent by the client when it is ready to start receiving packets from the server. It is also be used as a ping packet.

## Sample

```00```

---

# 0x01
## Description

A 5 byte packet containing screen dimensions of the client. This packet is sent just before the `0x00` packet. 

## Sample

```01 7E 00 61 00```

## Structure
```u8(header) u16(screen width / 10) u16(screen height / 10)```

---

# 0x03
## Description

This is the spawn packet, sent when a player wants to spawn into the game. The length of this packet is dependent on the name length, which is capped at 15 characters. It is always 3 bytes plus double the names character count. The character list is terminated by a null value.

## Sample

```03 74 00 65 00 73 00 74 00 69 00 6E 00 67 00 31 00 32 00 33 00 00 00```

## Structure
```u8(header) ...u16(characters of name, continues until a null value is read)```

---

# 0x04
## Description

A single byte, sent to the server when the client leaves the game. 

## Sample

```04```

---

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

---

# 0x07
## Description

This packet is sent when the client resizes the window, structured the same as the `0x01` packet

## Sample

```07 7E 00 61 00```

## Structure
```u8(header) u16(screen width / 10) u16(screen height / 10)```

---

# 0x08
## Description

A simple packet that is always 2 bytes. This is sent whenever the client clicks, or unclicks.

## Sample

```08 01```

## Structure
```u8(header) u8(clicking boolean)```

---


# Incoming (clientbound)

| Packet Name | Description                       | Status     |
|-------------|-----------------------------------|------------|
| 0x00        | ping packet                       | complete   |
| 0xa0        | map config                        | complete   |
| 0xa1        | contains our id                   | complete   |
| 0xa4        | events                            | complete   |
| 0xa6        | map                               | complete   |
| 0xb4        | update, creation, deletion packet | structured |
| 0xb5        | leaderboard                       | complete   |

---

# 0x00
## Description

A simple ping packet

## Sample

```00```

## Structure
```u8(header)```

---

# 0xa0
## Description

This packet contains information about the map, similar to ``0xa6``

## Sample

```A0 00 80 54 44 00 80 54 44 02```

## Structure
```u8(header) f32(mapx)? f32(mapy)?```

---

# 0xa1
## Description

Contains the players ID.

## Sample

```A1 91 DF 00 00```

## Structure
```u8(header) u32(id)```

---

# 0xa4
## Description

This packet is sent when either a player is killed by us, or we die by a player. It triggers the notification on screen. Depending on the flag at the second byte, the packet will either contain the name and id of a player we have killed (if it is 1), or the id and name of a player who killed us (if it is 2). Stop reading if the flag is null.

## Sample

```A4 02 2A BB 51 00 55 00 45 00 20 00 54 00 4F 00 4E 00 54 00 4F 00 20 00 45 00 52 00 45 00 53 00 20 00 00 00 00```

## Structure
```u8(header) u8(flag) u16(id) ...[u16(char of name, continues until null value is read)]```

---

# 0xa6
## Description

This packet contains the X, Y, and radius of every player.
## Sample

```A6 45 00 5E 7F F3 68 E3 8B D7 23 6A 80 6A 6B F9 36 53 96 0B 38 16 66 38 24 B7 36 A5 73 2A 31 2B 26 96 5B 26 97 7E 26 CB C3 22 4B A1 22 61 A8 23 BA BB 1A 85 D3 0F B9 8B 0F 1F A1 0F 97 82 08 C4 5C 08 0A B9 08 60 62 08 48 AF 08 DB 45 08 AD 98 08 AB B2 00 B3 FD 00 7E 2C 00 86 26 00 CB 60 00 B7 6B 00 5B C9 00 A8 68 00 0E B7 00 55 BA 00 0A D3 00 41 32 00 49 51 00 2D 5F 00 42 BC 00 1D 48 00 91 80 00 72 95 00 20 D8 00 0D BD 00 45 C9 00 94 4F 00 9E E0 00 6E 81 30 2C 79 00 34 4E 00 A2 5B 00 17 6E 00 7C C9 00 C9 96 00 FC 4E 00 6D 70 00 59 56 00 31 4D 00 1F 41 00 31 88 00 F9 84 00 38 29 00 54 FF 00 48 49 00 70 AE 00 9B 43 00 2B 12 00```

## Structure
```u8(header) u16(count) ...[u8(x) u8(y) u8(r)]```

---

# 0xb4
## Description

This is the update, delete, and creation packet. It contains information about the leader x, y, and id, along with creations, updates, and deletions of entities.

*I lost interest before diving deep into this packet, if I ever decide to come back to this project I will update this part. In the meantime, if anyone would like to make a pull request on this part, feel free to do so and you will be credited.*

---

# 0xb5
## Description

This leaderboard packet. Used to update the in game leaderboard.
## Sample

## Structure
```u8(header) ...[u16(id, stop reading if this is null) f32(score) ...[u16(char of name, terminated by a null value)]]```

---

## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

## License
[MIT](https://choosealicense.com/licenses/mit/)
