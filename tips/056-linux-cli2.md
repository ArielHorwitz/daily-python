# Using Python As Linux Scripts - Part 2
> It is highly recommended to read [part 1](/tips/055-linux-cli.md).

Yesterday, we saw how to run a Python script from the terminal like any other binary including taking command line arguments. Today we'll use `subprocess` to call arbitrary shell commands. We will write a little command line program to control PulseAudio's volume in `volume.py`:
```python
#! /usr/bin/python
import argparse
import subprocess

SET_VOLUME = ["pactl", "set-sink-volume", "@DEFAULT_SINK@"]
SET_MUTE = ["pactl", "set-sink-mute", "@DEFAULT_SINK@"]

def main():
    parser = argparse.ArgumentParser(description="Get and set volume. Pass no arguments to get volume.")
    parser.add_argument("-u", "--up", help="increase volume", action="store_true")
    parser.add_argument("-d", "--down", help="decrease volume", action="store_true")
    parser.add_argument("-m", "--mute", help="mute volume", action="store_true")
    parser.add_argument("-t", "--unmute", help="unmute volume", action="store_true")
    parser.add_argument("-i", "--increment", help="use increment", type=float, default=5)
    parser.add_argument("-s", "--set", help="set volume", type=float, required=False, dest="volume")
    args = parser.parse_args()
    if args.volume is not None:
        subprocess.run([*SET_VOLUME, f"{args.volume}%"])
    elif args.up:
        subprocess.run([*SET_VOLUME, f"+{args.increment}%"])
    elif args.down:
        subprocess.run([*SET_VOLUME, f"-{args.increment}%"])
    elif args.mute:
        subprocess.run([*SET_MUTE, "1"])
    elif args.unmute:
        subprocess.run([*SET_MUTE, "0"])
    else:
        command = ["pactl", "get-sink-volume", "@DEFAULT_SINK@"]
        result = subprocess.run(command, capture_output=True)
        volume_summary = result.stdout.decode()
        _, left, _, right, *_ = volume_summary.split("/")
        left = float(left.strip().removesuffix("%"))
        right = float(right.strip().removesuffix("%"))
        volume = round((left + right) / 2)
        print(volume)


if __name__ == "__main__":
    main()
```

This may feel like a lot to take in, but really there's only two parts to the main function: argument parsing and the `subprocess.run()` calls. The `subprocess.run()` is a high-level function to essential run subprocesses, handling file descriptors, pipes, etc. Most importantly it takes a list of arguments.

Let's not forget to set this script's permissions to be executable by anyone on the system:
```console
$ chmod +x /path/to/volume.py
```

And finally, we can run it:
```console
$ /path/to/volume.py
70%
$ /path/to/volume.py -u
$ /path/to/volume.py
75%
$ /path/to/volume.py -d --increment 6
$ /path/to/volume.py
69%
```

You may notice it's quite tedious to pass each call as a list of strings. In fact, in the example above we even had them created at the module scope for convenience. Tomorrow we'll use `shlex` to make this a little more convenient and a lot more safe, stay tuned!

References:
https://docs.python.org/3/library/subprocess.html
