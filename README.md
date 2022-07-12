# kyria keyboard ZMK config

## BUILD

Mostly, this is following the steps from `.github/workflows/build.yml`:

```
docker pull zmkfirmware/zmk-build-arm:stable
docker run --name zmk --rm -v $(pwd)/:/zmk -it zmkfirmware/zmk-build-arm:stable
```

Then in the container:

```
cd /zmk
```

And execute the steps from the github workflow:

```
west init -l config
west update
west zephyr-export

rm -rf build

west build --pristine -s zmk/app -b nice_nano -- -DSHIELD=kyria_left -DZMK_CONFIG="$(pwd)/config" && cp build/zephyr/zmk.uf2 kyria_left_nice_nano.uf2 && west build --pristine -s zmk/app -b nice_nano -- -DSHIELD=kyria_right -DZMK_CONFIG="$(pwd)/config" && cp build/zephyr/zmk.uf2 kyria_right_nice_nano.uf2

west build --pristine -s zmk/app -b nice_nano -- -DSHIELD=cradio_left -DZMK_CONFIG="$(pwd)/config" && cp build/zephyr/zmk.uf2 cradio_left_nice_nano.uf2 && west build --pristine -s zmk/app -b nice_nano -- -DSHIELD=cradio_right -DZMK_CONFIG="$(pwd)/config" && cp build/zephyr/zmk.uf2 cradio_right_nice_nano.uf2
```

If the two halves get unpaired, build a settings_reset image, and flash it to
both, then flash again the keyboard firmwares.
See https://zmk.dev/docs/troubleshooting#split-keyboard-halves-unable-to-pair

```
west build --pristine -s zmk/app -b nice_nano -- -DSHIELD=settings_reset -DZMK_CONFIG="$(pwd)/config" && cp build/zephyr/zmk.uf2 settings_reset.uf2
```
