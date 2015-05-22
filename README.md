# Hackspace RFID power switch & usage logger

A system for logging usage and only allowing an inducted user to turn on power
to a machine.

Valid inducted users are fetched from a google spreadsheet once per day.

Inducted users use their RFID cards to turn on power to the connected machine.

Usage time is logged to a google spreadsheet for maintenance and billing
purposes.

Power is controlled via 433MHz remote mains power sockets.

# Software

## Design decisions

* Presenting an inducted user's RFID card will turn on or off the SSR.
* If a new user is detected while machine is on, finish previous user's session and start a new one.
* Never switch off/time out the connected machine in case it's a long job.
* Pull validated users once per day and store locally in case internet is unavailable.
* If internet is unavailable discard usage logging. Possible to add a queue later.

## Flowchart

![software](software/software.png)

## Beeper

* a session starts or ends.
* RFID invalid user

## LEDs

Green LED for system status OK

## LCD messages & Menu

Display after inactive for 5 seconds

    --------------------
    scan RFID to start
    goto: ven.nz/1RFvu9s

Unrecognised user.

    --------------------
    unrecognised RFID
    goto: ven.nz/1RFvu9s

user Matt recognised

    --------------------
    user: matt
    press next for tools

down button pressed - shows a tool ready to start

    --------------------
    1: lathe    inducted  
    select to start

down button pressed  -showing Nic is already using machine for 1 hour 21 secs

    --------------------
    2: mill     inducted
    nic         00:01:21

down button pressed - showing router is currently unavailable, with webpage for details

    --------------------
    3: router   offline
    goto: ven.nz/1RFvu9s

down button pressed - showing laser cutter, not inducted and webpage for detils

    --------------------
    4: laser    noinduct
    goto: ven.nz/1RFvu9s

select button pressed - showing saw available for use

    --------------------
    5: saw      inducted
    select to start

select button pressed - showing saw and option to stop

    --------------------
    5: saw      00:00:00
    select to stop

select button pressed - showing option 1 again

    --------------------
    1: lathe    inducted  
    select to start

# Electrical

## Bill of Materials

* [Arduino Yun](http://uk.rs-online.com/web/p/products/7824594/)
* [10W 5V PSU](http://uk.rs-online.com/web/p/products/0327936/)
* [RFID reader](http://uk.rs-online.com/web/p/products/6666625/) [datasheet](http://docs-europe.electrocomponents.com/webdocs/0d16/0900766b80d1684b.pdf)
* [LCD 2x20](http://uk.rs-online.com/web/p/products/7200222/) [datasheet](http://docs-europe.electrocomponents.com/webdocs/0f25/0900766b80f25e5b.pdf)
* [case](http://uk.rs-online.com/web/p/products/3648223/)
* [433MHz ASK radio transmitter](http://uk.rs-online.com/web/p/lower-power-rf-modules/6172072/) [datasheet](http://docs-europe.electrocomponents.com/webdocs/087d/0900766b8087d2df.pdf)
* 2 [switches](http://uk.rs-online.com/web/p/push-button-switches/8207533/) [datasheet](http://docs-europe.electrocomponents.com/webdocs/1388/0900766b8138874b.pdf)
* [beeper](http://uk.rs-online.com/web/p/piezo-buzzer-components/7716910/) [datasheet](http://docs-europe.electrocomponents.com/webdocs/1168/0900766b811685e8.pdf)

## Wiring

![electrical components and wiring](electronics/electrical.png)

## Schematic

## Case design

[generated with openscad file case.scad](case/case.scad)

![case](case/case.png)

All measurements in mm and stated as L x W x D

* Yun + PCB = 70 x 85 x 10
* LED = 12mm diameter
* RFID = 62 x 82 x ?
* LCD = 37 x 116 x ?

Modelling the layout led to a case size of 280 x 120 x 90mm.
But these dimensions didn't yield many cases! So I looked for some common sizes
and found some that were 240 x 120 x 100mm. Setting the openscad case dimensions
to these figures allowed me to move things about to check fit.


# Shortcomings

* Super easy to bypass, but as used in a trusting environment, not seen as a big issue. A locking box could be made out of wood that would contain an extension lead end, the radio power switch and the end of the machine cable.
