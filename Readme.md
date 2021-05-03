# LCD2004 Adapter

![Before](/Images/LCD_withAdapter2.jpg)

This is an adapter to connect the DZ2004A-V1.0 LCD (Geeetech Prusa i3 pro B display) to the BIGTREETECH SKR E3 DIP V1.1 main board (LCD port).
This adapter is necessary because:
- The wiring of the LCD connectors on the display and the main board are different. The LCD2004 adapter connects the correct traces to make the display work again.
- The encoder isn't used anymore. The LCD2004 adapter is adding 5 push buttons as replacement for the encoder. The principle is inspired by the Zonestar LCD. 

Limitations:
- The encoder is out of service. This isn't a big deal, because it wasn't used that often because Octoprint is used most of the time to control the printer. So the 5 button keypad seemed to be good enough. Refer to section "Using the encoder again" of this Readme for some links how the encoder can maybe be used again.

## Principle of the 5 button keypad
The LCD2004 adapter is using 5 buttons to control the printers functions. This is inspired by the Zonestar display. 
Depending on which button is pressed, a different voltage is generated at the ADC_KEYPAD pin. This pin is read by an ADC on the main board.
The "UP" button is located on the bottom side of the PCB. This is correct because up means scrolling up which is the button on the bottom.

**Caution:** The SKR E3 DIP main board (and many other 32-bit boards) are working on 3.3V logic. Most pins on the ARM Cortex M3 STM32F103RCT6 are 5V tolerant (you can connect 5V to them) **but the ADC pin isn't (don't connect 5V to it)**! The LCD2004_Adapter uses a 3.3V zener diode to produce the correct voltage.

## PCB
The schematic and PCB was created using EAGLE. It was designed to be manufactured using toner transfer method so all traces are as big as possible.

![Before](/Images/LCD2004_Adapter_PCB_1.jpg)

## Housing
Print the following parts:
- 1x Keys_Housing.stl
- 1x Key_Back.stl and Key_Back_Icon.stl in one part
- 1x Key_Enter.stl and Key_Enter_Icon.stl in one part
- 1x Key_Home.stl and Key_Home_Icon.stl in one part
- 2x Key_Up_Down.stl and Key_Up_Down_Icon.stl in one part

**Caution:** The back, enter and home buttons are each 2 mm shorter because 2mm longer buttons were used here. The up and down buttons have the "correct" length.

The following parts are needed additionally:
- 2x 12mm spacers
- 1x M3 screw 10mm & M3 nut
- 2x wood screws 10mm

## Cabling
A default 10-pin ribbon cable can be used to connect the adapter with the main board. 
The only thing that must be changed here is to extract pin 8 (ADC_KEYPAD) on the main board side of the cable and wire it to the Servo pin (PA1) on the SKR E3 DIP board.

Cable complete                                |  SKR E3 DIP side
:--------------------------------------------:|:-------------------------:
![Cable complete](/Images/Cable_complete.jpg) |  ![Cable SKR E3 DIP side](/Images/Cable_SKR_E3_DIP.jpg)

## Firmware adaptions
Make the following adaptions to the Marlin 2.0.8 firmware:
- <Marlin root>/Marlin/Configuration.h: 
  Uncomment the `#define ZONESTAR_LCD` (in line 2086)
- <Marlin root>/Marlin/src/pins/stm32f1/pins_BTT_SKR_E3_DIP.h:
  Comment the line 190: `#error "CAUTION! ZONESTAR_LCD requires wiring modifications. See 'pins_BTT_SKR_MINI_E3_common.h' for details. Comment out this line to continue."`

## Using the encoder again
To use the encoder again instead of the 5 buttons, here are some inspirations. Because Octoprint is used most of the time, there wasn't spent much time on this.

https://electronics.stackexchange.com/questions/231803/rotary-encoder-to-2-buttons