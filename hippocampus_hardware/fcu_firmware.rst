FCU Firmware
############

Since the FCU is connected with the Raspberry Pi via USB, it is possible to flash new firmwares directly from the Raspberry Pi.

In the PX4-Autopilot repository is a script to do this: :file:`Tools/px_uploader.py`.

.. hint:: It is quite handy to have this script in :file:`~/bin` on the Pi. Make sure that this directory is in :code:`PATH`.



#. Copy the firmware you want to flash to the Raspberry Pi (for example with :code:`scp`).
#. Reboot the FCU into bootloader mode via its shell (QGroundControl or debug port)

   .. code:: sh

      screen /dev/fcu_debug 57600

   .. code:: sh

      reboot -b
   
   The FCU should stop flashing slowly in green and start flashing rapidly in some weird color.

#. Flash the firmware

   .. code:: sh

      px_uploader.py --port '/dev/ttyACM*' px4_fmu-v4_default.px4

   .. attention:: Make sure you have single quotes around the port name. Otherwise the shell resolves the wildcard before the python script is executed. Alternatively choose the port directly, probably :code:`/dev/fcu_usb`.

Exiting Bootloader
##################

In some cases the FCU might get stuck in bootloader mode. In this case send the reboot sequence to the bootloader.

Run

.. code:: sh

   python3

and execute the following lines:

.. code:: python

   import serial
   s = serial.Serial('/dev/fcu_usb', 115200)
   s.write(b'\x30' + b'\x20')
   quit()


   
