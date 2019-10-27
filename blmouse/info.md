# Wireshark capture info

## Setting up device

### Creating a connection to device

- 69	6.687100	host	controller	HCI_CMD	29	Sent LE Create Connection
`
- sends a connection request
- reads the meta information for a connection
    - Role : master
- reads the remote features
    - Returns list of features wit encryption set to true
    
    Supported LE Features: 0x0000000000000001, LE Encryption
    .... .... .... .... .... .... .... .... .... .... .... .... .... .... .... ...1 = LE Encryption: True
    .... .... .... .... .... .... .... .... .... .... .... .... .... .... .... ..0. = Connection Parameters Request Procedure: False
    .... .... .... .... .... .... .... .... .... .... .... .... .... .... .... .0.. = Extended Reject Indication: False
    .... .... .... .... .... .... .... .... .... .... .... .... .... .... .... 0... = Slave-Initiated Features Exchange: False
    .... .... .... .... .... .... .... .... .... .... .... .... .... .... ...0 .... = Ping: False
    .... .... .... .... .... .... .... .... .... .... .... .... .... .... ..0. .... = Data Packet Length Extension: False
    .... .... .... .... .... .... .... .... .... .... .... .... .... .... .0.. .... = LL Privacy: False
    .... .... .... .... .... .... .... .... .... .... .... .... .... .... 0... .... = Extended Scanner Filter Policies: False
    .... .... .... .... .... .... .... .... .... .... .... .... .... ...0 .... .... = LE 2M PHY: False
    .... .... .... .... .... .... .... .... .... .... .... .... .... ..0. .... .... = Stable Modulation Index - Tx: False
    .... .... .... .... .... .... .... .... .... .... .... .... .... .0.. .... .... = Stable Modulation Index - Rx: False
    .... .... .... .... .... .... .... .... .... .... .... .... .... 0... .... .... = LE Coded PHY: False
    .... .... .... .... .... .... .... .... .... .... .... .... ...0 .... .... .... = LE Extended Advertising: False
    .... .... .... .... .... .... .... .... .... .... .... .... ..0. .... .... .... = LE Periodic Advertising: False
    .... .... .... .... .... .... .... .... .... .... .... .... .0.. .... .... .... = Channel Selection Algorithm #2: False
    .... .... .... .... .... .... .... .... .... .... .... .... 0... .... .... .... = Power Class 1: False
    .... .... .... .... .... .... .... .... .... .... .... ...0 .... .... .... .... = Minimum Number of Used Channels Procedure: False
    0000 0000 0000 0000 0000 0000 0000 0000 0000 0000 0000 000. .... .... .... .... = Reserved: 0x000000000000

#### The five fields in the auth req byte

- Bonding flags
    - 0 - no bonding
    - 1 - yes bonding
    - bonding is yes LTK (A long term key is exchanged for allow device to pair even after a reboot or shutdown **Encryption happens after this phase**)
- MITM flag
    - 0 - MITM protection is not requested
    - 1 - MITM protection is requested
- Secure Connection
    - 1 - Only secure connection mode
    - 0 - No secure connection
- Keypress flag
    - 1 - Passkey entry needs to be used (input digits) / pressing button for set device into pairing mode
    - 0 - no pass inputs

- send pairing request with options
    - AuthReq: 0x2d, Secure Connection Flag, MITM Flag, Bonding Flags: Bonding
        - 001. .... = Reserved: 0x1
        - ...0 .... = Keypress Flag: False
        - .... 1... = Secure Connection Flag: True ( Asks connection to be secure since device supports encryption)
        - .... .1.. = MITM Flag: True ()
        - .... ..01 = Bonding Flags: Bonding (0x1)
    - Max enc key size is 16 (Small key length) Can be bruteforced given that it would be only digits

- pairing reply from mouse
Bluetooth Security Manager Protocol
    Opcode: Pairing Response (0x02)
    IO Capability: No Input, No Output (0x03)
    OOB Data Flags: OOB Auth. Data Not Present (0x00)
    AuthReq: 0x01, Bonding Flags: Bonding
        000. .... = Reserved: 0x0
        ...0 .... = Keypress Flag: False
        .... 0... = Secure Connection Flag: False
        .... .0.. = MITM Flag: False
        .... ..01 = Bonding Flags: Bonding (0x1)
    Max Encryption Key Size: 16
Initiator Key Distribution: 0x04, Signature Key (CSRK)
    0000 .... = Reserved: 0x0
    .... 0... = Link Key: False
    .... .1.. = Signature Key (CSRK): True
    .... ..0. = Id Key (IRK): False
    .... ...0 = Encryption Key (LTK): False
Responder Key Distribution: 0x03, Id Key (IRK), Encryption Key (LTK)
    0000 .... = Reserved: 0x0
    .... 0... = Link Key: False
    .... .0.. = Signature Key (CSRK): False
    .... ..1. = Id Key (IRK): True
    .... ...1 = Encryption Key (LTK): True


Supports only

- Bonding
- Initiator key is only signature and no encryption key
- Responder key
    - Encryption key (LTK)
    - Id Key (IRK)

- Sends a pairing confirm with a opcode 0x03 and value
- Receives a pairing confirm with an opcode 0x03 and different value

- sends pairing random gen value (key - 32 bit) b0b423e088ff9268a8687dfb01040a57
- receives pairing random gen value

- sends LE encryption key value
Bluetooth HCI Command - LE Start Encryption
    Command Opcode: LE Start Encryption (0x2019)
    Parameter Total Length: 28
    Connection Handle: 0x0e01
    Random Number: 0000000000000000
    Encrypted Diversifier: 0x0000
    Long Term Key: 32e98b15b29e952b3546112f1c22b298 **Only random key gen** 32 chars
    [Pending in frame: 93]
    [Command-Pending Delta: 2.126ms]
    [Response in frame: 96]
    [Command-Response Delta: 292.008ms]

- Declares secondary layer of services using Gatt
    - Battery Service
    - HID

- Receive encryption info
Bluetooth Security Manager Protocol
    Opcode: Encryption Information (0x06)
    Long Term Key: 04776702a8c92622dcf41b45ca237abf

- Received identifaction info master id
Bluetooth Security Manager Protocol
    Opcode: Master Identification (0x07)
    Encrypted Diversifier (EDIV): 0x2c80
    Random Value: b35fa38aa2090a43

- Identity information
    - Identity resolving key


## Moving Device

- Device movements are recorded by the handle - 0x00000016 HID
- opcode for handle 0x1b handle value notification
- value - The mouse movement or point value in hex - 18 bytes
    


