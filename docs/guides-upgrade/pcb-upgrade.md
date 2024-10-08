# PCB UPGRADE GUIDE

> [!Warning|style:callout]
> Please understand that modifying your coffee machine involves working with potentially lethal mains level voltage. Do not undertake this project if this makes you uncomfortable. Understanding & utilizing safe electrical practices is critical to your safety and safely completing this project. Only start working on your machine while it's completely disconnected from the mains power socket. By agreeing to follow the below guide, you agree that the authors cannot be deemed responsible for any of the damage you induce to your house appliances, yourself, your cat, your friend, or your gold fish. It will be entirely your fault! 

These instructions are for experienced makers only! In many cases buying a new PCB is a better option due to the upgrade difficulty and other pcb design improvements that come with a newer revision.  

# PA15 Pulldown Rework

> [!Warning|style:callout]
> The following modifications pull down the pin on affected PCBs. They are risky and recommended only for experienced solderers.  
> If your board is affected and you do not feel comfortable with the modifications then firmware updates should be flashed with machine power off.

The STM32 PA15 pin, used to control SSR1, has an internal pull-up resistor enabled by default that will activate the SSR. This pull-up can be disabled on the STM32U585, however, the STM32F411 pull-up can only be turned off when firmware is running. As a result, the boiler heaters will be turned on if an STM32F411 board without a pull-down is flashed with machine power on. 

With Gaggiuino Gen 2 the heating effects are minimal when flashing because VSCode automatically connects, flashes, restarts, and disconnects. However, when flashing Gaggiuino Gen 3 firmware with STM32CubeProgrammer the user must manually connect, start flashing, acknowledge flash results, and then disconnect. This slows the process and adds significant risk that the user will forget to disconnect in time, increasing the possibility that the system will overheat and trip the boiler's thermal fuse. 

## Testing

Not all PCBs are affected by this issue. To test if yours is affected:

1. Open your machine so the SSR LED is visible and connect a WeAct Mini Debugger / ST-Link as for flashing a firmware update.  

2. Open STM32CubeProgrammer and click **Connect**. Note whether the SSR light turns on, then click **Disconnect**.  

    <img width="500" alt="image" src="https://github.com/user-attachments/assets/87fed100-683d-49a5-80ce-ef1d31f21da0">

* If the SSR light light turns **on** when STM32CubeProgrammer is connected then your system is affected. Complete the PA15 pulldown upgrade or flash with machine power off. 
* If the SSR light light turns **off** when STM32CubeProgrammer is connected then your system is *not* affected and you can flash with machine power on.

## Bill Of Materials

<!-- tabs:start -->
<!-- tab:PCBv3.1 -->
* 4.7k&Omega; 0603 SMD resistor
<!-- tab:PCBv3 -->
* 4.7k&Omega; through-hole resistor
<!-- tab:PCBv2 -->
* 4.7k&Omega; through-hole resistor
<!-- tabs:end -->

## Instructions
<!-- tabs:start -->
<!-- tab:PCBv3.1 -->
There are two different versions of PCBv3.1, differentiated as PCBv3.1a and PCBv3.1b below.

PCBv3.1a has R6 and R10 near Q12, and a pull-down can be added by soldering the resistor across them as shown.  

<img width="500" alt="image" src="https://github.com/user-attachments/assets/45bb8c1e-cad4-4f58-843d-2a9301404324">  

PCBv3.1b has R30 and R10 near Q12, and the pull-down value can be fixed by replacing R30 as shown.  

<img width="500" alt="image" src="https://github.com/user-attachments/assets/5a5550c4-a9c3-46fe-a05c-ea8d31c02189">  

<!-- tab:PCBv3 -->
There are two different versions of PCBv3, differentiated as PCBv3a and PCBv3b below.

PCBv3a can have a pull-down added by soldering the resistor on the backside of the PCB between PA15 and the ground pin of J5.  
PCBv3b has R6 and R7 pull-downs, so a pull-down change shouldn't be necessary.  

<img width="500" alt="image" src="https://github.com/user-attachments/assets/8284a0e3-2bcf-451d-af7e-9672731f7cff">  

<!-- tab:PCBv2 -->
There are two different versions of PCBv2, differentiated as PCBv2a and PCBv2b below.

A pull-down can be added to PCBv2a by soldering the resistor on the backside of the PCB between PA15 and the ground pin of J27. Note that the PCB is shown from the front.  
PCBv2b has 3.3k&Omega; R36 and R37 pull-downs, so a pull-down change shouldn't be necessary.  

<img width="500" alt="image" src="https://github.com/user-attachments/assets/43b74fc9-3d55-4644-93b6-4063abbb1929">  

<!-- tabs:end -->

# STM32 FU Upgrade

> [!Warning|style:callout]
> This is documented because it's a possible upgrade path, but it's also an order of magnitude more difficult than anything else in this project and should not be attempted unless you have experience reworking SMDs. 

For our purposes, the STM32U585CIU6 is pin-compatible with the STM32F411CEU6, so the BlackPill F411 can be replaced with the U585. 

## Bill Of Materials

* STM32U585CIU6

## Instructions

1. Remove the STM32F411CEU6 from the BlackPill
2. Solder the STM32U585CIU6 in its place

This document will not cover SMD rework processes. If further instruction is needed you may want to reconsider the attempt (see warning above). 

This reworked BlackPill was part of initial STM32U585CIU6 feasibility testing.

<img width="500" alt="image" src="https://github.com/user-attachments/assets/42bf2e27-7189-43bf-9df9-760a537558a4">