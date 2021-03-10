# [brutal.io](https://brutal.io) info

In this repo, you will find a lot of my research on the game [brutal.io](https://brutal.io), including packet information and structure.

## Encoding Types

| Name    | Description               |  Length (bytes) | Shorthand |
|---------|---------------------------|-----------------|-----------|
| uint8   | a unsigned 8 bit integer  | 1               | u8        |
| uint16  | a unsigned 16 bit integer | 2               | u16       |
| uint32  | a unsigned 32 bit integer | 4               | u32       |
| float32 | a floating point number   | 4               | f32       |

## Packets

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


# Incoming (clientbound)



## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

Please make sure to update tests as appropriate.

## License
[MIT](https://choosealicense.com/licenses/mit/)
