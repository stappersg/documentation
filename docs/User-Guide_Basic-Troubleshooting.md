# Hardware troubleshooting guide

If you are experiencing at least one of these problems:

- Board does not boot
- Board freezes, crashes or reboots randomly or when connecting USB devices
- Plugged in USB devices are not detected (not listed in `lsusb` output)
- Error changing the root password at first boot (Authentication token manipulation error)
- Error installing or updating packages due to read-only file system

and you are using a stable Armbian image, then most likely you have one of two common problems - **powering issue** or **SD card issue**

Note that

- **"I know that my power supply is good", "it worked yesterday", "it works with a different device", etc. are not objective reasons to skip powering related diagnostics**
- **"I know that my SD card is good", "it worked yesterday", "it works with a different device", etc. are not objective reasons to skip storage related diagnostics**
- Undervoltage can cause symptoms related to SD card problems such as filesystem corruptions and data loss, so powering has to be checked first

### Powering notes

- Most boards (even ones fitted with PMIC - power management integrated circuit) don't have any measures to react to undervoltage that could prevent instability
- It doesn't matter what voltage your power supply outputs, it matters what voltage will reach the onboard voltage regulators
- Peak power consumption of popular boards can vary from 0.9A at 5V (H3 based Orange Pi PC) to 1.7A at 5V (RK3288 based Tinkerboard), both without any attached peripherials like USB devices
- Due to the Ohm's Law voltage drop due to cable and connector resistance will be proportional to the electric current, so most of the time problems will be experienced at current spikes caused by CPU load or peripherials like spinning up HDDs

#### Power supply

- Cheap phone chargers may not provide the current listed on their label, especially for long time periods
- Some cheap phone chargers don't have proper feedback based stabilization, so output voltage may change depending on load
- Power supplies will degrade over time (especially when working 24/7)
- Some problems like degraded output filtering capacitors can't be diagnosed even with a multimeter because of the non-linear voltage form

#### Cable

- The longer and thinner is the cable - the higher its resistance - the more voltage will drop under the load
- Even thick looking cable can have thin wires inside, so don't trust the outside cable diameter

#### Connector

- MicroUSB connector is rated for the maximum current of 1.8A, but even this number cannot be guaranteed. Trying to pass larger current (even momentarily) may result in a voltage dropping below USB specifications
- Most of the boards can also be powered through GPIO pins. This can be used to bypass the microUSB connector and thus to improve stability

### SD card notes

- SD card is a complex storage device with an embedded controller that processes read, erase and write operations, wear leveling, error detection and corruption, but it doesn't provide any diagnostic protocols like S.M.A.R.T.
- SD cards will degrade over time and in the end may fail in different ways - become completely or partially read-only or cause a silent data corruption

#### SD card brand

- Based on current prices and performance tests done by Armbian users Samsung Evo, Samsung Evo Plus and Sandisk Ultra cards are recommended
- Other good alternatives may be added to this page in the future

#### SD card size and speed class

- SD card speed class and size doesn't influence the reliability directly, but larger size means larger amount of lifetime data written, even if you are using 10-20% of the cards space

#### Writing images to the SD card

- If you wrote an image to the card it doesn't mean that it was written successfully without any errors
- While it's possible to verify images after write using some tools, currently Etcher is the only popular and cross-platform tool that does mandatory verify on write
- More lightweight alternatives may be added to this page in the future
- "Check for bad blocks" function available in some tools is mostly useless when dealing with SD cards
- Note that Etcher verifies only 1-2GB that is occupied by the initial unresized image, it doesn't verify the whole card
