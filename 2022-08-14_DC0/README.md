# Distributed Current
Baby steps towards energy independence.

Note: This is more of a lengthy lab report than a quick how-to.

## Motivation
We live in a world where more and more devices run on direct current.
Since the electricity grid supplies alternating current, most of them use their own external power supplies, converting AC to DC.

Luckily, recent years brought standardization towards USB and USB Power Delivery for everything from flashlights to laptop computers. This consolidated voltage requirements and connectors.
Additionally, LiIon cells are now widely available at low(er) cost,
so building powerful but light-weight energy storage becomes feasible.

With electricity grids becoming increasingly fragile due to extreme weather and geopolitics,
it makes sense to have energy buffers around those DC devices.
Why not have lights, communication equipment and computers running normally even when the grid has problems?
As an added benefit, this creates reusable building blocks for portable and off-grid installations.


## Step 1: Grid AC-DC, 1 Device
Replacing the specific laptop power supply with a general 12V source.

### Hardware
- quality switching power supply
  - 12V car "cigarette lighter" socket, max. 7A output
  - connectors for 13.8V max. 28A output
- cheap USB-C PD car charger
- laptop computer with USB PD input
- cable USB-C to USB-C

### Set-Up
- 12V power supply connected to 230V~ wall socket
- USB PD supply connected via 12V car socket
- Laptop connected via USB-C cable

### Tests
- Run laptop with heavy load: success
- Charge laptop: success

### Findings
- Audible fan noise from power supply.
- This was mainly a function check for power supply and charger.


## Step 1.1: Grid AC-DC, 2 Devices
Charging laptop and smartphone.

### Hardware
- (as in step 1)
- Smartphone
- cable USB-A to USB-C

### Set-Up
- (as in step 1)
- Smartphone connected to USB PD charger via USB-A to USB-C cable.

### Tests
- Charge phone: success
- Charge laptop: fail
- Charge laptop from AC charger (outside this experiment): fail

Laptop had to be rebooted to UEFI configuration to get PD charging working again.

### Findings
- Test probably limited by 7A output of the car socket.
- Low current caused malfunction of USB PD charger, likely due to cheap hardware.
- Laptop charge controller shuts down in unsafe conditions and stays offline until repeated rebooting.


## Step 2: Grid AC-DC-AC-DC, 1 Device
Running AC components from a DC source.

### Hardware
- quality switching power supply
  - 12V car "cigarette lighter" socket, max. 7A output
  - connectors for 13.8V max. 28A output
- inverter, 12V car plug to 150W (300W peak) 230V~ socket
- computer docking station with AC power supply

### Set-Up
- 12V power supply connected to 230V~ wall socket
- inverter via 12V car socket
- docking station power supply connected to inverter

### Tests
- Normal use of docking station (data only, no charging): success

### Findings
- Mainly function and quality check of inverter.
  - Inverter AC power is "clean" enough for docking station power supply, no noticeable sound or smell.


## Step 3: Grid AC-DC-AC, whole computer desk plus music
Running AC components from a DC source.

### Hardware
- quality switching power supply
  - 12V car "cigarette lighter" socket, max. 7A output
  - connectors for 13.8V max. 28A output
- inverter, 12V car plug to 150W (300W peak) 230V~ socket
- computer desk with everyday devices
- adapter cable from DC connectors to car socket
- AC power strip
- AC power meter

### Set-Up
- 12V power supply connected to 230V~ wall socket
- inverter hooked to **28A connectors** via adapter cable
- AC power strip connected to inverter, supplying
  - electric desk (height adjustment motor)
  - docking station
     - laptop computer (fully charged)
  - 27" LCD screen
  - external speaker
  - small home automation controller

### Tests
- Normal use of computer with external screen and music on speaker (no laptop charging): success for ~15min, then failure

Power meter showed around 42W, with spikes to 150W when adjusting the desk height.

### Findings
- Inverter started heating up after a few minutes and shut off output after approx. 15min despite power consumption being well within spec.
- Power meter covered red-green blinking status LED at inverter front and should be attached differently.
- All powered devices worked normally.


## To be continued
as new hardware arrives.
