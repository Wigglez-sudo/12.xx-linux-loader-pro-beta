# PS5 Pro 12.xx Linux Loader

Boot Linux on a PS5 **Pro** on firmware 12.xx.

## Files

| File | What it is |
|---|---|
| `Linux-loader-pro-aio.elf` | One file for every supported firmware. Works out the table it needs. **Use this.** |
| `ps5-linux-loader-1200-pro.elf` | Locked to 12.00 |
| `ps5-linux-loader-1202-pro.elf` | Locked to 12.02 |
| `ps5-linux-loader-1220-pro.elf` | Locked to 12.20 |
| `ps5-linux-loader-1240-pro.elf` | Locked to 12.40 |
| `ps5-linux-loader-1260-pro.elf` | Locked to 12.60 |
| `ps5-linux-loader-1270-pro.elf` | Locked to 12.70 |

The single-firmware builds are the same code as the AIO, restricted to one firmware. Use one only to rule out the AIO picking the wrong table.

Every build checks the console and refuses anything that isn't a Pro.

## Firmware support

| Firmware | PS5 Pro |
|---|---|
| 12.00 | ✅ confirmed working — Bazzite |
| 12.02 | ✅ confirmed working — Bazzite |
| 12.20 | ✅ confirmed working — Bazzite |
| 12.40 | ✅ confirmed working — Bazzite |
| 12.60 | ✅ confirmed working — Bazzite |
| 12.70 | ✅ confirmed working — Bazzite |

## Build

Source is in `pro-aio/`. Needs the PS5 payload SDK at `/opt/ps5-payload-sdk` (or set `PS5_PAYLOAD_SDK`).

```
cd pro-aio
make clean && make                                             # Linux-loader-pro-aio.elf
make clean && make EXTRA_CFLAGS="-DFW_1220_ONLY -DPRO_ONLY_GATE"  # the 12.20 build
make clean && make EXTRA_CFLAGS="-DFW_1200_ONLY -DPRO_ONLY_GATE"  # the 12.00 build, and so on
```

`shellcode_hv`, `shellcode_kernel` and `shellcode_ap` need a clean between builds, otherwise the previous blobs get relinked.

## What it does on a Pro

- Reads the Pro's hypervisor at `0x8E0000000` rather than the base carveout.
- Dumps the console's GPU firmware — eight `viola_*.bin` files — to `lib/firmware/amdgpu/` on the USB boot partition.
- Images built for the Pro pick those files up at boot. On Bazzite Deck, the first boot installs them; start Linux with the loader once more to use them.
- Writes `ps5lin_probe.log` to the boot partition, adding to it each run.

Firmware isn't modified and nothing is flashed — a reboot puts everything back. It does patch the running system in memory while it runs.

## Thanks

Pro dumps: solidpsycho (12.00) · boykopovar (12.02) · mclariechanrie (12.20) · Anon (12.40) · Marlon (12.60) · H1Vltg3 (12.70)

Built on https://github.com/ps5-linux/ps5-linux-loader, including the Pro GPU firmware work from their PR #84.
