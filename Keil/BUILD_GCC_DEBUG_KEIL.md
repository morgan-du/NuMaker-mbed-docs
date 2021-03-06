# Build with Mbed CLI GCC_ARM and debug with Keil uVision

This is clone of [Build with Mbed CLI ARM/ARMC6 and debug with Keil uVision](BUILD_ARMCC_DEBUG_KEIL.md), with toolchain changed to GCC.

The *NuMaker-PFM-NUC472* board (*NUMAKER_PFM_NUC472* target) is taken as an example for explanation.

## Hardware setup
1.  Switch the *NuMaker-PFM-NUC472* board to **Debug** mode.
1.  Connect the board to the host computer via USB.

## mbed command line
1. Guide GCC to generate uVision-compatible debug information in the JSON file `mbed-os/tools/profiles/develop.json` by:
    1.  Changing optimization level to **"-O0"**
    1.  Changing debug information level to **"-g"**
    1.  Changing debug information format to **"-gdwarf-2"**
    1.  Add **"-DMBED_DEBUG"** macro.
        Also confirm `idle-thread-stack-size-debug-extra` is defined in `mbed-os/rtos/mbed_lib.json` for debug target. Both `MBED_DEBUG` and `idle-thread-stack-size-debug-extra` must be ready to enable extra idle thread stack size in debug build.

    <pre>
    "GCC_ARM": {
            "common": ["-c", "-Wall", "-Wextra",
                   "-Wno-unused-parameter", "-Wno-missing-field-initializers",
                   "-fmessage-length=0", "-fno-exceptions", "-fno-builtin",
                   "-ffunction-sections", "-fdata-sections", "-funsigned-char",
                   "-MMD", "-fno-delete-null-pointer-checks",
                   "-fomit-frame-pointer", <b>"-O0"</b>, <b>"-g"</b>, <b>"-gdwarf-2"</b>, <b>"-DMBED_DEBUG"</b>],
    </pre>
    
1. Build *your_program* through **Mbed CLI** and you would get *your_program*.elf in the BUILD/NUMAKER_PFM_NUC472/GCC_ARM folder.
    ```
    mbed compile -m NUMAKER_PFM_NUC472 -t GCC_ARM
    ```

## Keil uVision IDE
1. Create a uVision project, e.g. *Debug_mbed* through the menu **Project** > **New Project**. Select *NUC400/NUC472HI8AE* (**TARGET DEPENDENT**) as target.
1. Select *Nuvoton Nu-Link Driver/NUC400* (**TARGET DEPENDENT**) as ICE driver through the menu **Debug**.
1. Copy *your_program*.elf built above to the uVision project folder **Debug_mbed/Objects**.
1. Replace *Debug_mbed* with *your_program*.elf through the menu **Output** > **Name of Executable**.
1. Download *your_program*.elf through the  menu **Flash** > **Erase** and then **Flash** > **Download**.
1. Enter debug session through the menu **Debug** > **Start Debug Session**.
