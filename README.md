# Bluetooth research project

The purpose of this project is to research, evaluate and understand the bluetooth technology stack.
It was also to understand at which stage of bluetooth communication the recently published Knob Attack takes place.
[Knob Attack](https://knobattack.com/)

## Directory Structure

## blmouse

This directory contains wireshark capture file of pairing and using bluetooth mouse.
The purpose of this capture was to identify the generation of new session keys and communication to establish new encrypted session for data transfer.

## wcap

This directory contains wireshark captures files of performing scans for advertising bluetooth devices and pairing attempts

## MiBand3

This diretory contains a python3 package on connecting to XiaoMi MiBand 3 using a Linux PC.
The package uses the python bluepy module and uses some other opensource libraries to perform bluetooth pairing and authentication.

