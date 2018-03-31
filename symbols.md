| Symbol | Description |
| :--- | :--- |
| `I` | Infinitechain contract |
| `S` | `stage` set |
| `s` | `stage` |
| `sl` | The latest `stage` |
| `s_n` | `stage` height |
| `Ld` | Deposit log set |
| `Lw` | Withdrawal log set |
| `Lc` | Challenge log set |
| `lg` | Deposit log |
| `lw` | Withdrawal log |
| `lc` | Challenge log |
| `fw` | Flag indicating wether withdrawed or not |
| `fc` | Flag indicating wether challenge succeeded or not |
| `fp` | Flag indicating wether challenge compensated or not |
| `fn` | Flag indicating wether `stage` is finalized or not |
| `A` | Account set |
| `a` | Account |
| `ai` | Account of user `i` |
| `ai_a` | Address of accoount `i` |
| `ai_b` | Balance of accoount `i` |
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
| `B` | `balance` tree |
| `Bi_h` | `balance` root hash of stage height `i` |
| `r` | `receipt` |
| `r_s` | `receipt` signed by server |
| `LSN` | Local sequence number |
| `GSN` | Global sequence number |
| `v` | Value of assets |
| `p` | Challenge penalty |

| Function | Description |
| :--- | :--- |
| `makeLightTx` |
| `signLightTx` |
| `makeReceipt` |
| `audit` |
| `*Audit` | Broadcast `Audit` event with `Ri_h` |
| `*challenge` |
| `*Challenge` | Broadcast `Challenge` event with `sn_i` `GSN` `ai_a` |
| `*isSettled` |
| `*isInChallengePeroid` |