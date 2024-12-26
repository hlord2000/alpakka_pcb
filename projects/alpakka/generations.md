# PCB generations
The concept of `Generation` versioning was introduced to mark minor changes that are still relevant for the firmware execution, so the firmware can use different configuration values or execution paths if needed.

This way a single version of the firmware is able to support multiple versions of the PCB if the changes are not big backward compatibility breakers.

These firmware-relevant changes may be different in size and complexity than the actual changes in PCB layout or components, and therefore are tracked separately from the main PCB semantic versioning.

The counter might be restarted if there are other means to determine the hardware changes (eg. different compilation targets).

## Defined pins
| Hardware version | Pin A | Pin B |
| - | - | - |
| v0 | A10 | A11 |
| v1 | A6 | A7 |

## Masking
The firmware can check in execution-time to which generation the PCB belongs via a ternary mask on the defined pins.

| Electrical | Ternary value |
| - | - |
| FLOAT | 0 |
| GND | 1 |
| VCC | 2 |

| Generation number | Pin A | Pin B |
| - | - | - |
| 0 | FLOAT | FLOAT |
| 1 | FLOAT | GND |
| 2 | FLOAT | VCC |
| 3 | GND | FLOAT |
| ... | | |

## Generations
| Generation  | From PCB version | To PCB version | FW-relevant change | Mask |
| - | - | - | - | - |
| v0 gen0  | b0.0.0 | b0.84.4 | | A10=FLOAT A11=FLOAT |
| v0 gen1  | b0.88.0 | b.0.90.2 | 500KΩ touch resistor | A10=FLOAT A11=GND |
| v1 gen0  | b1.0.0 | _ongoing_ | All v1 changes | A6=FLOAT A7=FLOAT |

