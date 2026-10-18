<p align="center">
  <img src="branding/cutlass-miner-lockup-1024.png" alt="Cutlass Miner" width="512">
</p>

# Pearl Miner — RTX 50-series

High-intensity Pearl (pearlhash) GPU miner for NVIDIA RTX 50-series (sm_120).
Ships the BM=128 all-four-optimized kernel, pinned to the max-intensity config
(grid 32768, 1024 MiB budget). Single self-contained executable — no Python or
CUDA toolkit install required.

## Requirements
- Windows 10/11 (x64)
- NVIDIA **RTX 50-series** GPU (sm_120). Will not run on 30/40-series.
- Current NVIDIA driver

## Setup
1. Open `config.json`.
2. Replace `PASTE_YOUR_PRL_WALLET_HERE` with your Pearl wallet address
   (`prl1...`).
3. (Optional) set `worker` to a label for the pool dashboard.
4. Save.

## Run
Double-click **`mine.cmd`** (or run it from a terminal). It starts the miner at
grid 32768 and streams pool status to the console. Stop with Ctrl+C.

## Config notes
- `--pearl-grid 32768` is the **maximum** supported grid (4096/8192/16384/
  32768). Larger = more sustained intensity (higher wattage, more shares).
- `--pearl-budget-mib 1024` sets the VRAM budget for the search workspace.
- `batch_seconds` = how often the pool batch is flushed.

## Fee
A fixed **2% dev fee** is baked into `pearl-miner.exe` (paid to the developer
wallet on accepted shares). It is not configurable from `config.json`.

## Warranty
Provided as-is. Mining consumes significant power and generates heat; monitor
your GPU. No guarantee of profitability — returns depend on pool difficulty,
Pearl price, and your electricity cost.

## Branding
Logo and brand assets live in [`branding/`](branding/) — see
[`branding/BRAND.md`](branding/BRAND.md) for sizes, usage, and rules.
Strictly monochrome (black `#000000` / white `#ffffff`), no exceptions.
