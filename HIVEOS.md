# Cutlass Miner on HiveOS, Minerstat, RaveOS & MMPOS

Cutlass Miner ships a **custom-miner package** for auto-mining operating
systems. One tarball works on HiveOS, Minerstat, RaveOS and MMPOS — the
`h-*` integration files are the HiveOS convention that the others also read.

## What's in the package

`cutlassminer_custom-<ver>.tar.gz` contains:

| file | purpose |
|---|---|
| `pearl-miner` | the closed-source miner (ELF, x86-64, glibc 2.39+) |
| `runtime/` | native kernels (sm_86 / sm_89 / sm_120) + proof verifier |
| `h-manifest.conf` | miner metadata (name, version, algo, entrypoints) |
| `h-config.sh` | writes `config.json` from the OS's wallet/pool/worker env vars |
| `h-run.sh` | launches the miner with the right flags |
| `h-stats.sh` | reports hashrate / accepted / rejected / uptime to the dashboard |
| `config.json` | default config (edit only if not using an auto-mining OS) |

## HiveOS

1. **Add a wallet** — HiveOS web UI → *Wallet* → *Add wallet*:
   - Coin: `Pearl` (or `Ravencoin` for KawPow)
   - Address: your `prl1...` (or `R...`) address
2. **Add the custom miner** — HiveOS web UI → *Flight Sheet* → *New flight
   sheet* → *Miner* → *Custom Miner*:
   - Paste the release URL:
     `https://github.com/liqdmetal/cutlass-miner-release/releases/download/v1.4.5/cutlassminer_custom-1.4.5.tar.gz`
   - HiveOS downloads, extracts and installs it automatically.
3. **Set the pool** — in the flight sheet, set the pool URL:
   - Pearl: `stratum+tcp://prl-us.kryptex.network:7048`
   - Ravencoin: `stratum+tcp://rvn.kryptex.network:7055`
4. **Optional tuning** — extra config fields in the flight sheet:
   - `CUSTOM_GRID` (default `32768`, max `32768`)
   - `CUSTOM_BUDGET_MIB` (default `1024`)
   - `CUSTOM_REGION` (default `8192`)
   - `CUSTOM_DEVICE` (default `cuda:0`)
5. **Apply the flight sheet** to the rig. The dashboard shows hashrate,
   accepted/rejected shares and uptime from `h-stats.sh`.

> HiveOS runs `h-config.sh` on every start, so wallet/pool/worker changes in
> the web UI apply on the next miner restart — no SSH needed.

## Minerstat

1. *Minerstat OS* → *Mining* → *Add miner* → *Custom miner*.
2. Name: `cutlassminer`, version `1.4.5`.
3. Upload or point at the same `cutlassminer_custom-1.4.5.tar.gz`.
4. Set the wallet and pool in the miner's config fields (they map to the
   `WALLET` / `POOL` env vars `h-config.sh` reads).
5. Start the worker. Minerstat reads the same `h-*` files.

## RaveOS

1. *RaveOS* → *Rigs* → *Miner* → *Custom*.
2. Upload `cutlassminer_custom-1.4.5.tar.gz` (or fetch it from the release
   URL).
3. Set coin `PEARL`, wallet address, and pool
   `stratum+tcp://prl-us.kryptex.network:7048`.
4. Apply. RaveOS executes `h-run.sh` and parses `h-stats.sh` output.

## MMPOS

1. *MMPOS* → *Miner* → *Custom miner* → *Add*.
2. Name `cutlassminer`, version `1.4.5`, upload the tarball.
3. Configure wallet + pool in the miner settings.
4. Deploy to the rig.

## Requirements on the rig

- Linux x86-64, **glibc 2.39 or newer** (HiveOS 0.6-222+, Minerstat OS,
  RaveOS and MMPOS current images all qualify).
- NVIDIA **RTX 30-, 40- or 50-series** GPU (sm_86 / sm_89 / sm_120).
- NVIDIA driver **580 or newer**.
- The miner refuses any other card with an explicit message — no mismatched
  kernels.

## Fee

A fixed **2% dev fee** is baked into the binary (paid on accepted shares).
It is not configurable and not present in `config.json`.

## Troubleshooting

| symptom | fix |
|---|---|
| `WALLET env var is required` | set the wallet in the OS's wallet page, not just the miner config |
| miner starts then exits | check the rig's miner log for the refusal message (wrong GPU arch or driver < 580) |
| dashboard shows 0 hashrate | `h-stats.sh` reads the pool API; give it a few minutes, and confirm the wallet is correct |
| `glibc` errors on old images | update the OS image (needs glibc 2.39+) |
