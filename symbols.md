| Symbol | Description |
| :--- | :--- |
| `I` | Infinitechain contract |
| `Ld` | Deposit log set |
| `Lw` | Withdrawal log set |
| `Lc` | Challenge log set |
| `lg` | Deposit log |
| `lw` | Withdrawal log |
| `lc` | Challenge log |
| `fd` | Flag indicating wether deposited or not |
| `fw` | Flag indicating wether withdrawed or not |
| `fc` | Flag indicating wether challenge succeeded or not |
| `fp` | Flag indicating wether challenge compensated or not |
| `A` | Account set |
| `a` | Account |
| `ai` | Account of user `i` |
| `ai_a` | Address of accoount `i` |
| `ai_b` | Balance of accoount `i` |
| `S` | `stage` set |
| `sn_i` | height of latest `stage` |
| `T` | `lightTransaction` set |
| `Ti` | `lightTransaction` set of stage height `i` |
| `t` | `lightTransaction` |
| `t_c` | `lightTransaction` signed by client |
| `t_s` | `lightTransaction` signed by client and server |
| `R` | `receipt` set |
| `Ri` | `receipt` set of stage height `i` |
| `Ri_h` | `receipt` root hash of stage height `i` |
| `Ri_s` | `receipt` set stored by server |
| `Ri_c` | `receipt` set stored by client |
| `r` | `receipt` |
| `r` | `receipt` signed by server |
| `LSN` | Local sequence number |
| `GSN` | Global sequence number |
| `v` | Value of assets |

| Function | Description |
| :--- | :--- |
| `makeLightTx` |
| `signLightTx` |
| `make` |
| `audit` |
| `Audit` | Broadcast `Audit` event with `Ri_h` |
| `challenge` |
| `Challenge` | Broadcast `Challenge` event with `sn_i` `GSN` `ai_a` |
