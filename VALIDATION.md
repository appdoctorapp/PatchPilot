# Validation — 2026-09-20

20/20 executable test groups passed on Windows 11 x64 / .NET 10.0.11. WPF application checks also passed: controls load, asynchronous synthetic pipeline, comparison rows, selected candidate details, persisted selection and restored busy/stop state. Tests use synthetic .ppack data. No Unreal Engine, UnrealPak, real game or Steam integration was executed.

## Measured synthetic experiment results

All transfer values are local compressed-transfer ESTIMATES (bytes). Installed sizes are actual fixture file lengths. Every candidate passed exact extracted-payload and byte-reconstruction verification. The baseline same-content rebuild deliberately shuffles payload order; a separate byte-identical test gives zero estimated transfer and zero modeled disk I/O.

| Scenario | Candidate | First transition estimate | Same-policy estimate | Structure-only estimate | Installed bytes |
|---|---|---:|---:|---:|---:|
| same-content | baseline | 3957189 | — | — | 3957189 |
| same-content | stable-raw | 4186594 | 0 | 4186594 | 7888896 |
| same-content | stable-1m | 4218930 | 0 | 4218930 | 12911044 |
| small-edit | baseline | 3957197 | — | — | 3957197 |
| small-edit | stable-raw | 4186635 | 4186635 | 4186594 | 7892992 |
| small-edit | stable-1m | 4218940 | 363037 | 4218930 | 12911044 |
| add | baseline | 3959237 | — | — | 3959237 |
| add | stable-raw | 4187975 | 933117 | 4186594 | 8022016 |
| add | stable-1m | 4243621 | 363069 | 4218930 | 13631639 |
| delete | baseline | 3627461 | — | — | 3627461 |
| delete | stable-raw | 3837763 | 3837763 | 4186594 | 7231488 |
| delete | stable-1m | 3866201 | 363071 | 4218930 | 11862468 |

In small-edit, stable-1m gives a same-policy estimate of 363,037 bytes versus baseline 3,957,197 bytes (about 90.8% lower), but first-transition estimate increases to 4,218,940 bytes (about 6.6% worse) and installed size grows from 3,957,197 to 12,911,044 bytes. Therefore NO candidate is recommended in any of the four scenarios. This is fixture evidence, not a real-game benefit claim.

Optional local reconstruction ran in roughly 0.010–0.024 seconds on these small fixtures. These single warm/unspecified-cache runs verify byte reconstruction, not Steam install timing or representative game performance. Raw transfer, separate disk reads/writes and precise timing are retained in local JSON/CSV reports.

## Failure and recovery coverage

Corrupt archives, encrypted/invalid Pak footers, unknown/incomplete UnrealPak listing grammar, IoStore input, unsafe relative/device paths, overlapping workspace, missing ownership marker, active-session lock, interruption, cancellation with fresh-session restart, undo with source preservation, worse-candidate rejection, HTML escaping, credential-shaped redaction and a real owned subprocess timeout/kill were tested. Mock listing parsing is not actual UnrealPak qualification.

## Not verified

Actual UE 5.4 Pak creation/extraction, packaged game boot, representative scenes, game loading time, private Steam depot download size, Steam installation time, HDD/SSD performance, large-build scalability, all Windows/DPI configurations and paid-product readiness. No public Steam branch was touched. Supply a licensed UE 5.4 install, an owned project and two complete staged builds to qualify the adapter. Then run repeated private-Steam experiments before claiming real savings.
