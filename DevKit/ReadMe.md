# DevKit Changelog
This readme is to give info as to the updates that happen to the files in this repository.

2/14/2017 - Sam added updated hardware files to fix the duplicate data issue with the data stream. 

2/14/2017 - Graham added updated devkit software files which takes data directly from the DRAM, processes it, and saves it to the SD card.
The data are saved in binary format and may be converted to text format with my program called bin_to_text on the shared drive.

2/24/2017 - Graham updated the LunaH devkit files, namely LApp and read_data_in which include 3 new case statement cases to allow the GUI to change various run parameters.

3/15/2017 - Graham updated two files in the devkit. Namely, LApp.c, read_data_in.c which allow the GUI that was built to access the functions of the devkit. These functions are options 10 - 13 at the 'Main Menu' and are described in more detail in the "Software versions for Luna H" word document in this repository.

3/29/2017 - GJS - Tweaked the I2C source and header files, cleaned up use of variables, and moved some declarations to LApp.h and LI2C_Interface.h. Implemented a log file system which writes to a log file that is created (or appended to) at the beginning of main. The first 10 bytes of the file are reserved; they are equal to the number of bytes written to the file plus 10. This tells the SD card functions where to write to so we preserve previous log data. This is included in the log file itself so that we may use the same log file even after power cycling the uZ board. The log file system needs testing and validation on other systems. Also added a small function to count determine the number of digits of a small (< 1000000000) (< 10^8) integer. This is useful for determining what bytes to write to the beginning of the log file.

4/14/2017 - GJS - Added a directory log file which holds the names of all files created by the uZ. The filenames are printed to the terminal as one string. Files are added to the directory when the uZ is powered up (and the file didn't already exist) and each time a data run is made (just processed data for now). 
				- Included support for long file names (LFN) when saving to the SD card. This changes the cloning of github repositories slightly, as the user will need to modify the contents of ffconf.h when a clone is made. The file is re-built each time that the standalone_bsp is regenerated.
				- DIR function now runs check properly for testing whether or not to add a file name to the directory.
				
8/4/2017 - GJS - This DevKit has been reframed as an emulator for the final version that will be present on the microZed boards on the LunaH science module. The idea is to emulate the input and output syntax that the S/C will use to communicate and control the microprocessor and, thus, the hardware and software used to take data during the mission. A later commit may contain the instrument control document(ICD) which guides the user through the commands and expected behaviour when using the instrument. No useful data is taken with this update; only dummy functions are present when using data acquisition functions. The goal for this version of the software was to provide Blue Canyon Technologies (BCT) and our collaborators at ASU a sense of what the module will be capable of and what options are available. This information will help inform recovery options available to us and to BCT.

10/16/2017 - GJS - v2.0 - A TEST command has been added to the DevKit to allow testing of new functions. I have used this command to add in proper data acquisition and data printing, as found in older versions of the DevKit. This version is a stepping stone for later versions which will be fully functional. This version is also an intermediate because we are waiting to implement an updated bitstream and upgraded digital board for the microZed. The digital board has had some of its hardware changed, thus the need to adapt the FPGA logic and bitstream. Also included is the current version of the ICD.

10/24/2017 - GJS - v2.1 - The file pointer logic has been largely removed from this version of the devkit. The TEST command has been successful, as has adding code to properly write header and footer information to the data files being created in case 1:DAQ. This code will be replicated to the other case statements and the TEST command will be maintained to continue testing. The next step will be to move the actual data acquisition into the correct case statement. Another important testing feature will be to get the WFs working and saving to the SD card. 

10/30/2017 - GJS - v2.2 - Edited case 0: DAQ in the main loop of the program. Now DAQ properly opens, closes, and adds headers and footers to the data files being created/modified by the program. Most, if not all, places in the code which used the file pointer method for accessing data in the log files or data files has been removed and replaced with much simpler logic. 

12/12/2017 - GJS - v2.3 - Added code to track default parameters and load them into the corresponding GPIOs when after the system boots. 

12/15/2017 - GJS - v2.31 - Edited case 6, which is the transfer file command. Adding packetization scheme into the transfer function so that the bus can receive data files. Not finished implementing yet.

2/9/2018 - GJS - v2.5 - Finished the transfer file command. This command transfers packets of data which are comprised of payload (the data), a header, and a Fletcher checksum. The packets are transferred with the UART from the SD card. A return value of "FFFFFF" is sent if the file name can not be found. for finding files, case is ignored. A simple GUI in visual c++ has been created to read the files from the serial transfer and verify their contents.

4/2/2018 - GJS - v2.6 - Verified the TX and DAQ functions for data product "fake" files of random data. DAQ produces both event-by-event and neutron count files according the ICD and TX transfers the contents of those files in CCSDS packets. 

5/18/2018 - GJS - v2.7 - Minor functionality updates. Tweaked how many bytes TX sends so both checksums are sent. Edited init sequence on PS boot. Updated .gitignore file. Updated microzed flash instructions.

5/18/2018 - GJS - v2.71 - Major change of work flow. This change should simplify the steps needed for collaborators to load a microZed board with processor code and run it. The need to build a project in Xilinx SDK is eliminated. Now, user must use Vivado Lab Tools and the included files (BOOT.mcs, MZ_FSBL.elf) to program the microZed. Then, after a power cycle, the board will boot into standby mode and be ready to receive input. A new set of programming instructions are to be found in this folder.