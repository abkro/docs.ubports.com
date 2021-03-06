Android apps
========================

`Anbox <https://anbox.io>`_ is a minimal Android container and compatibility layer that allows you to run Android apps on GNU/Linux operating systems such as UBports.

.. note::
    When "host" is used in this document, it refers to another device which you can connect your Ubuntu Touch device to. Your host device must have ``adb`` and ``fastboot`` installed.

.. note::
    You will need to execute commands on your Ubuntu Touch device to install Anbox and APKs. You can do that by using the terminal application, but it is easier to use ``adb shell`` or :doc:`set up ssh </userguide/advanceduse/ssh>` to access your phone from your host.

Supported devices
-----------------

Make sure your device is supported:

- Meizu PRO 5
- Fairphone 2
- OnePlus One
- Nexus 5
- BQ Aquaris M10 HD
- BQ Aquaris M10 FHD

How to install
--------------

.. warning::
    Installing Anbox is only recommended for experienced users.

Install Anbox kernel
^^^^^^^^^^^^^^^^^^^^

Some devices require you to install a custom Linux kernel to use Anbox. These devices are:

- Meizu Pro 5 (codename: ``turbo``, name of the boot partition: ``bootimg``)
- BQ M10 HD (codename: ``cooler``, name of the boot partition: ``boot``)
- BQ M10 FHD (codename: ``frieza``, name of the boot partition: ``boot``)

If your device is not in this list, the Anbox kernel was automatically installed when you installed Ubuntu Touch. Please skip to :ref:`run-anbox-installer`.

**You will need to repeat these steps after each Ubuntu Touch update**. Not doing so can put your phone into an unstable state. Only update your Ubuntu Touch device when you have a computer with you to re-flash the modified kernel image.

#. Be sure to have a `backup <https://askubuntu.com/questions/602850/how-do-i-backup-my-ubuntu-phone>`_ of your device.
#. Open a terminal on your host and set some device specific variables by running ``export CODENAME="turbo" && export PARTITIONNAME="bootimg"``, but replace the part between the quotes respectively with the codename and name of the boot partition for your device. See the above list.
#. Activate developer mode on your device.
#. Connect the device to your host and run the following commands from your host (same terminal you ran the ``export`` command in)::

    wget http://cdimage.ubports.com/anbox-images/anbox-boot-$CODENAME.img
    adb shell # connect from your host computer to your UT device
    sudo reboot -f bootloader # 'adb shell' will exit after this command, the prompt will be back on your host
    sudo fastboot flash $PARTITIONNAME anbox-boot-$CODENAME.img
    sudo fastboot reboot
    rm anbox-boot-$CODENAME.img
    exit

.. _run-anbox-installer:

Run the Anbox installer
^^^^^^^^^^^^^^^^^^^^^^^

Once your device has the Anbox kernel installed, you can use the Anbox Tool to install the Anbox container.

#. Run ``adb shell`` from your host computer to get a shell on your Ubuntu Touch device.
#. Run the following command on your Ubuntu Touch device: ``anbox-tool install``.
#. Follow the on-screen instructions.

Now you're done! You might have to refresh the app drawer (pull down from the center of the screen and release) for the new Android apps to show up.

.. note::
    You now have an ADB server running on your Ubuntu Touch device. This guide asks you to run some ``adb`` commands, sometimes on your computer, other times on the device itself. Carefully check which device you are on!

You can check that ADB server is correctly running locally on your phone by opening the terminal app and enter ``adb devices``. You should see something like::

    phablet@ubuntu-phablet:~$ adb devices
    List of devices attached
    emulator-5558	device

How to install new APKs
-----------------------

- Copy the APK to ``/home/phablet/Downloads``, then run the following from your host::

    adb shell # connect from your host computer to your UT device
    adb install /home/phablet/Downloads/my-app.apk # This is the adb of your device, not your host
    exit

- Done! You might have to refresh the apps scope (pull down from the center of the screen and release) for the new Android apps to show up.

Keep your apps up to date
-------------------------

- To keep your apps up to date you can use of F-Droid or ApkTrack. If you want to install any of the above apps you can find them here:

- F-Droid: https://f-droid.org/
- ApkTrack: https://f-droid.org/packages/fr.kwiatkowski.ApkTrack/

How to uninstall apps
---------------------

- To uninstall apps, run ``adb uninstall [APP_ID]`` from your Ubuntu Touch device::

    adb shell # connect from your host computer to your UT device
    sudo mount -o rw,remount /
    adb uninstall [APP_ID] # This is the adb of your device, not your host
    exit

- Done! You might have to refresh the apps scope (pull down from the center of the screen and release) for the new Android apps to show up.

Access Android storage
-----------------------

Android storage is located at ``/home/phablet/anbox-data/data/media/0``.

- Links to android libraries can be made in their respective XDG counterparts::

    ln -s ~/anbox-data/data/media/0/Documents ~/Documents/android
    ln -s ~/anbox-data/data/media/0/Pictures ~/Pictures/android
    ln -s ~/anbox-data/data/media/0/Music ~/Music/android
    ln -s ~/anbox-data/data/media/0/Movies ~/Videos/android


Troubleshooting
---------------

- When you want to install an APK, but get the error ``Invalid APK file`` that error could also mean "file not found"

  - Check that you typed the filename correctly
  - If the APK does not reside in the current folder where you execute adb, you have to specify the full path, e.g. ``/home/phablet/Downloads/my-app.apk`` instead of just ``my-app.apk``


Reporting bugs
--------------

Please :doc:`report any bugs </contribute/bugreporting>` you come across. Bugs with Ubuntu Touch 16.04 are reported in `the normal Ubuntu Touch tracker <https://github.com/ubports/ubuntu-touch/issues>`_ and issues with Anbox are reported on `our downstream fork <https://github.com/ubports/anbox/issues>`_. Thank you!
