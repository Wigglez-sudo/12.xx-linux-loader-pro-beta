# 12.xx Pro loader source

Source for `Linux-loader-pro-aio.elf`. PS5 Pro only, firmware 12.00, 12.02, 12.40,
12.60 and 12.70.

```
cd pro-aio
make PS5_PAYLOAD_SDK=/opt/ps5-payload-sdk
```

You need the PS5 payload SDK, plus gcc, binutils and xxd for the three shellcode
blobs. The output lands in `pro-aio/bin/ps5-linux-loader.elf`.

The Pro check is compiled in, so it refuses to run on a Phat or Slim.

`pro-aio/source/` is the loader itself, `pro-aio/include/` holds the firmware
tables, and the three `shellcode_*` directories build the blobs it copies into
the console.

Based on https://github.com/ps5-linux/ps5-linux-loader. The 12.xx Pro tables and
the GPU firmware dump are the reason this fork exists. GPL-3.0.

The built loaders are at https://github.com/Wigglez-sudo/12.xx-linux-loader.
