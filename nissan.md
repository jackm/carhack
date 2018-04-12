# Nissan Sentra 2010 CAN bus data

## Inputs to reverse engineer

A preliminary list of human vehicle interactions to sniff the CAN bus for.

* Accelorator pedal
* Brake pedal
* Steering wheel angle
* Vehicle lights controls
* Windshield wiper controls
* Turn signals
* Cruise control on/off
* Horn
* Climate control fan on/off
* Rear window defrost on/off
* Radio on/off
* Various radio buttons
* Power windows
* Door locks
* Power side mirrors

## Active CAN IDs

Using `cansniffer -c can0`, `canbusload -tcbr can0@500000`

### Key inserted, ACC off, engine off

CAN bus load 7%

When key inserted or removed; messages only last a few seconds then stop
358
35D
625

When key moved to first position; messages sent periodically and constantly
35D
60D
625

When car is off and a control is activated (that is allowed to be when the car is off, eg door open, hazard & head lights, etc.) these messages are sent in addition to the usual ones associated with said control:
35D byte 2: 00=doors closed, 03=driver door opened
625 6 bytes total byte 4: 0E=doors closed, 0D=any door opened

### ACC on, engine off

CAN bus load 30%

Following always active and changing
174 byte 5: 0C 04 05 09 01 0D
176 7 bytes total byte 7: 01 0D 09 05 06
180
182
280
284
285
551 byte 1: 31 32
560 3 bytes total
6E2 3 bytes total byte 3: 78 7A, 7B 79

#### Accel pedal

160
 last 4 bytes are position
 first two bytes 3D64 when pedal not pressed, rises to 41D4 when fully pressed
 third byte always A1

#### Brake pedal

354 byte 7: 06 when not pressed, 16 when pressed
35D byte 5: 00 when not pressed, 10 when pressed

#### Vehicle lights

Three positions, possible states are off / on / fog lights / high beams

358 byte 2: 00=off, 80=on or fog lights, 
60D byte 1: 00=off, 04=on, 06=fog lights
60D byte 2: 06=hibeams off, 0E=hibeams on
625 6 bytes total byte 2: 00=off, 40=on, 60=fog lights, 10=hibeams

When car is off:
5C5 byte 1: 80=off, 40=any head light is on/active

#### Hazard lights

Message changes each time hazard lights are flashed on and off

60D byte 2: 06=off / blink off, 66=blink on

#### Wipers

Wiper control stick

35D byte 3: 00=rest, C0=oneshot "MIST", 40=intermitten "INT", C0=slow const "LO" , E0=fast const "HI"
354 appears during intermitten
    byte 5: 48=rest, 4A=intermitten active, 
625 byte 1: 32=rest, 34=intermitten active, 

#### Parking brake

5C5: byte 1: 40=off, 44=park brake engaged

#### Turn signals

60D byte 2: 06=off tick, 26=left turn tick, 46=right turn tick

#### Cruise control

551 byte 6: 00=off, 50=cruise control sys on

#### Horn

nothing

#### Climate control fan

358 byte 2: 00=off, 40=fan on (any speed)
35D byte 1: C0=off, C1=fan on (any speed)

#### Rear defrost

35D byte 1: C0=off, C6=on

#### Radio

nothing

#### Power windows

nothing

#### Door locks

nothing

#### Power side mirrors

nothing

#### Door ajar

60D byte 1: 00=doors closed, 08=driver door, 10=passenger door, 20=driver side rear door, 40=passenger side rear door

#### Seat belt

280 byte 1: 01=driver seat belt clicked in, 03=seat belt not clicked in

#### Cabin lights

nothing

###3 Gas cap release

nothing, likely just mechanical

#### Trunk release

nothing, likely just mechanical


### ACC on, engine on

CAN bus load 30%

Following always active and changing:
160
174
176
180
182
1F9
280
284
285
551
560
6E2

## CAN ID summary

ID | Desc | Interval
0x001 | signal 1 | 10 ms
0x002 | signal 2 | 100 ms
