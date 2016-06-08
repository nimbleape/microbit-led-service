# microbit-led-service

This example application shows how to enable the micro:bit's Bluetooth LED Service. The micro:bit has Bluetooth Low Energy and a GATT server, [this document](https://lancaster-university.github.io/microbit-docs/resources/bluetooth/microbit-profile-V1.9-Level-2.pdf) contains the information about the GATT server, which services it offers up, their UUIDs and their charcteristic's UUIDs.

This example utilises the [micro:bit runtime](https://lancaster-university.github.io/microbit-docs/) - there are many other ways of programming on the micro:bit.

If you're interested in learning more about GATT and BLE there's a great article from [adafruit](https://learn.adafruit.com/introduction-to-bluetooth-low-energy/gatt) on the topic.

## Configuration

To enable some of the Bluetooth Low Energy features we need, we need to set some configuration variables; we do this using a `config.json` file that the build tool `yotta` understands. This is a much cleaner way of configuring these variables than changing the `MicroBitConfig.h` file found within the microbit-dal dependency.

The key parts we need to set are the `bluetooth open` - by default the micro:bit is not open and you need to pair with it in the more conventional Bluetooth way. This means changing the setting from 0 to 1. We also need to change the GATT table size from the default of 300 to 700 (it may work with a setting of 500, depending on how much you enable in your application)

```js
{
    "microbit-dal":{
        "bluetooth":{
            "enabled": 1,
            "pairing_mode": 0,
            "private_addressing": 0,
            "open": 1,
            "whitelist": 1,
            "advertising_timeout": 0,
            "tx_power": 0,
            "dfu_service": 1,
            "event_service": 1,
            "device_info_service": 1
        },
        "reuse_sd": 1,
        "default_pullmode":"PullDown",
        "gatt_table_size": "0x700",
        "heap_allocator": 1,
        "nested_heap_proportion": 0.75,
        "system_tick_period": 6,
        "system_components": 10,
        "idle_components": 6,
        "use_accel_lsb": 0,
        "min_display_brightness": 1,
        "max_display_brightness": 255,
        "display_scroll_speed": 120,
        "display_scroll_stride": -1,
        "display_print_speed": 400,
        "panic_on_heap_full": 1,
        "debug": 0,
        "heap_debug": 0,
        "stack_size":2048,
        "sram_base":"0x20000008",
        "sram_end":"0x20004000",
        "sd_limit":"0x20002000",
        "gatt_table_start":"0x20001900"
    }
}
```

## Building and deploying your application to the micro:bit

Firstly, install the build environment `yotta`; you can find instructions [here](http://lancaster-university.github.io/microbit-docs/offline-toolchains/)

Once you have your environment, make sure you're targetting the right build target, in this case we need the `bbc-microbit-classic-gcc` target.

```
yt target bbc-microbit-classic-gcc
```

You should now be able to run our build step

```
yt build
```

This should result in some files built inside `./build/bbc-microbit-classic-gcc/source`

You want to copy the `microbit-led-service-combined.hex` file over to the micro:bit - you can drag and drop it over, or copy it from the command line - on OSX you can use this generic command

```
cp build/$(yt --plain target | head -n 1 | cut -f 1 -d' ')/source/$(yt --plain ls | head -n 1 | cut -f 1 -d' ')-combined.hex  /Volumes/"MICROBIT"
```

## Done?

You should see the words "BLE Enabled" scrolling across the screen! If you do, the BLE LED Service is enabled and you can start building applications with it.
