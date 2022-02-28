# kyria keyboard ZMK config

## BUILD

Mostly, this is following the steps from `.github/workflows/build.yml`:

```
docker run --name zmk --rm -v $(pwd)/:/zmk -it zmkfirmware/zmk-build-arm:2.5
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
west build --pristine -s zmk/app -b nice_nano -- -DSHIELD=kyria_left -DZMK_CONFIG="$(pwd)/config" && cp build/zephyr/zmk.uf2 kyria_left_nice_nano.uf2
west build --pristine -s zmk/app -b nice_nano -- -DSHIELD=kyria_right -DZMK_CONFIG="$(pwd)/config" && cp build/zephyr/zmk.uf2 kyria_right_nice_nano.uf2
```
