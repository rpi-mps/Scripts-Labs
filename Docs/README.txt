All your lab folders should have this basic file and folder structure (otherwise they won't compile or get cleaned up properly!):

inc - user headers
src - user .c source
startup - essential file(s) used to turn on the STM32F769I-Discovery
Cleanup.bat - cleanup script for HAL-based projects
Compile.bat - compilation script for HAL-base projects
Upload.bat - upload script
c_files.txt - See below for more details
h_hiles.txt - See below for more details

The files c_files.txt and h_files.txt are to be modified to match the header directories and .c files needed for the project. The files that c_files.txt refers to specifically are in the stm32lib directory in "Backend" ("Backend" is located one folder up from here, and contains files and folders necessary to compile and upload code to the STM32 Discovery board). h_files.txt, by contrast, is the list of include directories you want passed to GCC. Open them up in Notepad, WordPad, Atom, etc. and see for yourself how they look (or just keep reading, which is probably a good idea anyways so you don't get really confused really fast).

For example, the below files are what the STM32 Workbench program uses in an ADC example:

c_files.txt:

stm32lib/Drivers/BSP/Components/otm8009a/otm8009a.c
stm32lib/Drivers/BSP/STM32F769I-Discovery/stm32f769i_discovery.c
stm32lib/Drivers/BSP/STM32F769I-Discovery/stm32f769i_discovery_lcd.c
stm32lib/Drivers/BSP/STM32F769I-Discovery/stm32f769i_discovery_sdram.c
stm32lib/Drivers/CMSIS/Device/ST/STM32F7xx/Source/Templates/system_stm32f7xx.c
stm32lib/Drivers/STM32F7xx_HAL_Driver/Src/stm32f7xx_hal.c
stm32lib/Drivers/STM32F7xx_HAL_Driver/Src/stm32f7xx_hal_adc.c
stm32lib/Drivers/STM32F7xx_HAL_Driver/Src/stm32f7xx_hal_adc_ex.c
stm32lib/Drivers/STM32F7xx_HAL_Driver/Src/stm32f7xx_hal_cortex.c
stm32lib/Drivers/STM32F7xx_HAL_Driver/Src/stm32f7xx_hal_dma.c
stm32lib/Drivers/STM32F7xx_HAL_Driver/Src/stm32f7xx_hal_dma2d.c
stm32lib/Drivers/STM32F7xx_HAL_Driver/Src/stm32f7xx_hal_dsi.c
stm32lib/Drivers/STM32F7xx_HAL_Driver/Src/stm32f7xx_hal_gpio.c
stm32lib/Drivers/STM32F7xx_HAL_Driver/Src/stm32f7xx_hal_i2c.c
stm32lib/Drivers/STM32F7xx_HAL_Driver/Src/stm32f7xx_hal_i2c_ex.c
stm32lib/Drivers/STM32F7xx_HAL_Driver/Src/stm32f7xx_hal_ltdc.c
stm32lib/Drivers/STM32F7xx_HAL_Driver/Src/stm32f7xx_hal_ltdc_ex.c
stm32lib/Drivers/STM32F7xx_HAL_Driver/Src/stm32f7xx_hal_pwr.c
stm32lib/Drivers/STM32F7xx_HAL_Driver/Src/stm32f7xx_hal_pwr_ex.c
stm32lib/Drivers/STM32F7xx_HAL_Driver/Src/stm32f7xx_hal_rcc.c
stm32lib/Drivers/STM32F7xx_HAL_Driver/Src/stm32f7xx_hal_rcc_ex.c
stm32lib/Drivers/STM32F7xx_HAL_Driver/Src/stm32f7xx_hal_sdram.c
stm32lib/Drivers/STM32F7xx_HAL_Driver/Src/stm32f7xx_hal_uart.c
stm32lib/Drivers/STM32F7xx_HAL_Driver/Src/stm32f7xx_ll_fmc.c

h_files.txt:

stm32lib/Drivers/CMSIS/Device/ST/STM32F7xx/Include
stm32lib/Drivers/CMSIS/Include
stm32lib/Drivers/STM32F7xx_HAL_Driver/Inc
stm32lib/Drivers/BSP/Components/Common
stm32lib/Drivers/BSP/STM32F769I-Discovery
stm32lib/Utilities/Log
stm32lib/Utilities/Fonts
stm32lib/Utilities/CPU

It might look a little overwhelming at first, but it's actually not that bad. See, ST Microelectronics packs so much good stuff into their microcontrollers that if you were to just bit bang the whole thing on a register level, you might hit your next birthday before your code's done. To mitigate that problem, STM created a Hardware Abstraction Layer (HAL) that enables various features of their microcontrollers. If you look at the c_files.txt example above, you can see that there is a main "stm32f7xx_hal.c" followed by a bunch of other "stm32f7xx_hal_[thing].c" files. "stm32f7xx_hal_i2c" enables the I2C HAL module, for example. Each HAL module (which you enable/disable as needed in "stm32f7xx_hal_conf.h") contains a bevy of functions like ADC_Init, HAL_ADC_START_DMA, etc. that basically do a lot of the necessary bitmasking for you. And with 10.5 ports, 16 pins per port (except Port K, which has 8, hence the 0.5), and up to 16 alternate functions per pin, it makes a lot of sense to have as streamlined a process as possible to access all the features STM32 put in there. Of course you don't strictly need any of these to program the microcontroller, but as stated before--and as you will find out in the next lab--HAL is really nice when not referring to HAL9000 from 2001: A Space Odyssey.

Another note about the HAL is that this is not unique to STM32, either. If you look at the 5th line in the above c_files.txt example, you'll see a CMSIS folder. This is a HAL that is universal across all ARM devices--meaning anything you can do purely with CMSIS on this MCU will pretty much work on any ARM chip. If you're wondering what BSP is (right above the line with CMSIS), that's an even higher level HAL that STM32 made to abstract some of the HAL stuff further, and contains really basic functions like BSP_LED_Init(LED1), which sets up LD1 (red LED below the blue button) for you so you can blink it with BSP_LED_On(LED1). In general, you won't be using BSP functions yourself in MPS unless explicitly told--or the good Professor is merciful upon your poor soul at the cost of some grade points.

One last thing to address: Why make this build system like this? Why not just provide backend files as needed and forego the need for a system employing c_files.txt and h_files.txt?

Mainly, this system it makes it really easy for build script users to work with STM32 Workbench users. STM32 Workbench has a TON of symbolic links that it uses to keep track of what it actually needs to compile, and what c_files.txt and h_files.txt do is strip away those links to allow users to see and change what goes on under the hood. In other words, this system lets you keep things as simple as possible so that you can spend more time programming and debugging and less time trying to figure out what STM32 Workbench did with your include directories. A perk of this is that build script users can easily jump to STM32 Workbench if they want, and vice-versa.

It also means that you only need one "Backend" folder (have you seen the file size of that thing? GCC alone takes up a few hundred MB), which will then work for all of your labs.

Anyways, enough of all that gab. Time for some programming! Open "How to use.txt" to get started!
-Karl Nasrallah
EE/Physics dual, Class of 2018
