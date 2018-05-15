# Variables
| Symbol | Description |
| :--- | :--- |
| `C` | BOLT contract |
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
| `A` | `account` set |
| `Ai_h` | `account` root hash of stage height `i` |
| `a` | `account` |
| `ai` | `account` of user `i` |
| `ai_a` | Address of `account` `i` |
| `ai_b` | Balance of `account` `i` |
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
| `r_s` | `receipt` signed by server |
| `LSN` | Local sequence number |
| `GSN` | Global sequence number |
| `DSN` | Deposit sequence number |
| `v` | Value of assets |
| `p` | Challenge penalty |
| `v_i` | Limit of the value for `instantWithdrawal` |

# SDK Functions
| Symbol | Description |
| :--- | :--- |
| `makeLightTx` |
| `signLightTx` |
| `makeReceipt` |
| `signReceipt` |
| `verifyLightTx` |
| `verifyReceipt` |
| `type` | Returns `deposit` / `withdrawal` / `instantWithdrawal` / `remittance` |
| `saveReceipt` |
| `sendLightTx` |
| `getRootHash` |
| `audit` |
| `attach` |
| `challenge` |
| `defend` |
| `compensate` |
| `finalize` |

# Contract Functions
| Symbol | Description |
| :--- | :--- |
| `*audit` |
| `*Audit` |
| `*attach` |
| `*Attach` |
| `*challenge` |
| `*Challenge` |
| `*defend` |
| `*Defend` |
| `*compensate` |
| `*Compensate` |
| `*finalize` |
| `*Finalize` |
| `*isSettled` |
| `*isInChallengePeroid` |
