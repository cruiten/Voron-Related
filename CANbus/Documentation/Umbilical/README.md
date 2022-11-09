# CAN bus umbilical

## Why?
After implementing the Mellow Fly SB2040, I decided to implement an umbilical because I had 3 wires fail in my X drag chain within a span of 4 months...

## The plan
- Move the X endstop to the right X-carriage mount (It already has a spot to mount the switch)
- Move the Y endstop to the A motor mount
- Remove the X and Y drag chains
- Need mounts for both ends of the umbilical cable
- Build the umbilical cable
- Hook it all up and enjoy all the hard work!

### Move the X endstop
- This was very easy to do since the Voron 2.4r2 X-carriage already has a spot to mount the switch.
- Connected the X endstop switch wires to the gpio29 enstop connection on the SB2040

### Move the Y endstop
- This was more involved than I expected
- I used hartk's modified A motor top plate
  - https://github.com/hartk1213/VoronUsers/blob/master/printer_mods/hartk1213/Voron2.4_Y_Endstop_Relocation/STLs/Gantry/AB_Drive_Units/a_drive_frame_upper_with_jst_y_endstop.stl
  - This was very involved and took some time since I had to replace it on a completed printer
  - In hindsight, if I had to do it again, I would probably use one of the following two solutions, so that I would not have to swap the entire top A motor mount:
    - https://github.com/cruiten/Voron-Related/blob/main/CANbus/Umbilical/STLs/a_drive_umbilical_pg7_y_endstop.stl
    - https://github.com/Minsekt/moronvods/blob/main/Rear_Umbilical/Y_Endstop_Relocation/STL/y_endstop.stl
    
### Remove the X and Y drag chains
- Felt good to remove these
- I actually ended up replacing my Z drag chain with one of the removed drag chains since they are "skinnier" than the Z drag chain, and with CAN bus there are far fewer wires...

### Umbilical Mounts
- For the toolhead mount I used this: https://github.com/hartk1213/MISC/blob/main/Voron%20Mods/Voron%202/2.4/CW2_SB2040_CAN_Umbilical/STLs/cw2_umbPG7_skinnyy.stl
  - I needed the "skinny" version, because the original version was interfering with the Z drag chain and the PG7 gland on top of the A motor plate
- I used an M12 Aviation connector and mounted the female socket on the toolhead mount
- 
