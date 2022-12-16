lsidrivemap
===========


Heavily modified from [louwrentius/lsidrivemap](https://github.com/louwrentius/lsidrivemap) to bring this up to the times. Customization still required of the `print_controller` function as every card, enclosure, case, etc. is different for everyone. Since I have two enclosures on my controller/case, the enclosure IDs (13 and 17) are hard coded to customize the visual layout. 

This fork adds:

- Moved from MegaCLI (deprecated) to Broadcom/LSI's StorCLI64
- Support for enclosures (reported by StorCLI) which are just an additional level beyond controller/port, it's now controller/enclosure/port
- Support for new commands:
    - `model`: shows drive model information
    - `serial`: shows drive serial number
    - `size`: shows drive sizes
- `temp` command now uses `hddtemp` binary as StorCLI does not include support for drive temperatures (at least that I could find)
- `temp` now returns `°C` in result (because, why not)

My case is a [Supermicro 847BE1C4-R1K23LPB](https://www.supermicro.com/en/products/chassis/4U/847/SC847BE1C4-R1K23LPB)

The tool can also show the temperature of the drive.

Example output:

    root@nas:~# lsidrivemap temp

    | 30°C | 30°C |      | 26°C |
    | 33°C | 31°C |      |      |
    | 34°C | 33°C |      |      |
    | 36°C | 35°C |      |      |
    | 36°C | 34°C |      |      |
    | 34°C | 32°C |      |      |

    root@nas:~# lsidrivemap disk

    | sdg | sdm |     | sdn |
    | sdf | sdl |     |     |
    | sde | sdk |     |     |
    | sdd | sdj |     |     |
    | sdc | sdi |     |     |
    | sdb | sdh |     |     |

    root@nas:~# lsidrivemap wwn

    | 5000C500C734781B | 50014EE26113D21D |                  | 5000C500E539033F |
    | 5000C500C739AE9B | 50014EE26138E03B |                  |                  |
    | 5000C500C7255D4A | 5000C500DBB06129 |                  |                  |
    | 5000C500C670EFE5 | 5000C500C885D09C |                  |                  |
    | 5000C500C61FD2A7 | 5000C500C8A5B738 |                  |                  |
    | 5000C500C62977F3 | 5000C500C8A0B9DD |                  |                  |

    root@nas:~# lsidrivemap model

    | ST16000NM001G-2KK103 | WDC WD60EFRX-68MYMN1 |                      | ST16000NE000-2RW103  |
    | ST16000NM001G-2KK103 | WDC WD60EFRX-68MYMN1 |                      |                      |
    | ST16000NM001G-2KK103 | ST16000NM001G-2KK103 |                      |                      |
    | ST16000NM001G-2KK103 | ST16000NM001G-2KK103 |                      |                      |
    | ST16000NM001G-2KK103 | ST16000NM001G-2KK103 |                      |                      |
    | ST16000NM001G-2KK103 | ST16000NM001G-2KK103 |                      |                      |

    root@nas:~# lsidrivemap size

    | 16 TB | 6 TB  |       | 16 TB |
    | 16 TB | 6 TB  |       |       |
    | 16 TB | 16 TB |       |       |
    | 16 TB | 16 TB |       |       |
    | 16 TB | 16 TB |       |       |
    | 16 TB | 16 TB |       |       |


Customization
-------------

The output is based on my 24 bay drive chassis that has
six rows of four drives. You may need to customise the
'print_controller' function to suit your own needs. 

Known Issues
------------
The script reads the WWN serial from the drive and uses
it to find the drive name in /dev/disk/by-id. If the storcli
command does not return a WWN, which happens on older WD drives
for me, no data is returned.

Requirements
------------
- The script requires Python 3x or higher.
- Broadcom/LSI command line utility storCLI64 (google for a download)
- Ensure storCLI64 is in your $PATH
- `hddtemp` utility (install with apt, yum, apk, etc.)


