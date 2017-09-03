The purpose of this folder is to provide a starting template for the creation of future labs using the build script system.

How to use:
1. Enable the HAL modules desired in inc\stm32f7xx_hal_conf.h by uncommenting them
2. Add the relevant files (the peripheral and its _ex) to c_files.txt
>> The "master list" of these files is in Backend\stm32lib\Drivers\STM32F7xx_HAL_Driver\Src
3. Replace hello.c/hello.h (placeholders with a working configuration) with desired code.

c_files.txt, as it currently stands, is the absolute minimal needed to get things up and running.

Enjoy!
-Karl Nasrallah