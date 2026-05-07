# Project: Off-grid Mobile Device Waterproof Protection for Field Prototype Tests

## Motivation: Engineering with the Seasons in Hayama
Living in Hayama on the Miura Peninsula of the Japanese archipelago, I am constantly reminded of the beauty and power of the seasons.

The incredibly warm winter sunlight that softens the harsh winds, the fierce spring gales that sweep through the coast (reminding me of the "MISTRAL" from a famous anime I watched in my schooldays), the cooling sea breeze of summer carrying the scent of the tide, and the ginkgo trees dancing in the autumn wind, each turning yellow at its own unique pace. I feel a deep sense of gratitude to live in an environment where nature's presence is so profoundly felt.

For me, the wind — whenever and wherever it blows — traces back to my childhood. Every night before bed, I loved listening to the rustling of leaves in the night breeze. It was the sound by a large poplar tree planted by my grandmother to commemorate my birth. In Japan, the poplar trees of Biei Town in Hokkaido are famous. (https://ja.wikipedia.org/wiki/%E3%83%9D%E3%83%97%E3%83%A9) If you felt something strange, your instincts are correct. As you know, the poplar becomes too big to be in a small residential area in Japan. My mother have never said that, however, I'm thinking she perhaps knew it but deliberately chose it.

Because, my grandmother was a resilient woman who met my grandfather at "Manchuria", a land stretching to the horizon endlessly in Northeast China, during WWII. Just after the war (yes, the war should have ended), she was a rare surviver who escaped the overwhelming Soviet persistent offensive robbing and narrowly returned to Japan. In our small but safe residential area in Southern Osaka Prefecture, that poplar grew unnaturally tall, surpassing our living house by the time I was 17. Because it became a lightning hazard, we eventually had to cut it down.

Though the tree is gone, the "SARI-SARI" sound of its leaves remains etched in my heart — no, I should say — it shaped my heart. Today, as I face the fierce gales of Hayama, I find myself still trying to capture and understand what the sound of the wind means through my engineering.

This project is a humble attempt to harmonize technology with these natural elements. My goal is to ensure that my prototypes can continue to survive even after my dead and function as guardians within the same environment I call my new quiet safe area like "Laputa".

## Project Scope
This guide focuses on two essential pillars for field deployment, explained simply for region-free beginners using common hand tools and cost-effective materials.

![images/schematic]("Schematic diagram")

## 1. Region-free Off-grid Immortal Power Solutions
To keep the Raspberry Pi Pico 2W connected to the Wi-Fi and allow my HomeServer to send real-time commands—such as making the Haniwa’s eyes flash—we must first understand the power consumption of the Wi-Fi chip (CYW43). While transmitting data naturally consumes significant power, we can save energy by putting the chip into a "doze" state where it only listens for incoming packets. Even in this power-saving mode, the system will continuously consume approximately ~20mA. If we connect it to a mobile battery with an effective capacity of 3,000mAh, the system is calculated to last for ~6days.
$$3000[mAh] / 20[mA] /24 [h/d] = 6.25[d]$$
``` c++
    cyw43_arch_enable_sta_mode();
    cyw43_wifi_pm(&cyw43_state,
        cyw43_pm_value(CYW43_PM2_POWERSAVE_MODE, 200, 1, 1, 5));
    watchdog_update();
```
If we have even one sunny day during that 6-day period, we can keep the cycle going. Given a solar input of
$$3[W] / 5[V] * 1000 = 600[mA]$$
just 5 hours of warm sunlight should be enough to fully recharge the 3,000[mAh] battery. With this setup, even if I were to pass away decades from now, this system would continue to draw breath, tirelessly supplying power to the Pico 2W. The Legacy Battery and the Pass-through TestI searched through my storage and found an old friend: a 6,700[mAh] mobile battery made by a Japanese manufacturer a many years ago.

![An old mobile battery got a chance to a new place it should be.](images/20260505_mobile_battery.jpg "An old mobile battery got a chance to a new place it should be.")

It’s nearly a decade old (2018 on sale) and long discontinued, so it’s safer to assume its actual capacity has dropped to about half. Still, it might provide at least 3,000[mAh]. Let’s put it to work. The first thing to check was whether this battery supports "pass-through" charging—the ability to provide power to a device while simultaneously being charged itself. 

To test this, I used a DIY bedside lamp I had previously made by stuffing an LED string light into a small wine bottle. I bought this bottle at a hotel during my very first business trip abroad — a memory I still cherish. I connected the battery to a wall outlet via USB micro-B and plugged the LED string into the battery’s USB Type-A port.

![The pass-through function confirmed with German bottle and Japanese battery.](images/20260505_pass-through_confirmed.jpg "The pass-through function confirmed with German bottle and Japanese battery.")
![An LED string.](images/20260505_LED_string.jpg "An LED string.")
![A small wine bottle.](images/20260505_small_wine_bottle.jpg "A small wine bottle.")
![The battery pass-through tester at my bedside.](images/20260505_pass-through_tester.jpg "The battery pass-through tester at my bedside.")

The wine bottle emitted a gentle, steady glow. Success. It was time to bring this ancient battery into my latest project. Selecting the Solar Panel to charge a 3,000[mAh] battery in 5[hours] at a standard USB voltage of 5[V], I needed a solar panel with a generating capacity of at least 3[W]. I decided on a 3.5[W] panel made for security cameras used outside of many houses. It’s a modest setup, but it feels just right for this project.

![The sunshine harvester can provide 3.5[W].](images/20260507_solar_panel.jpg "The sunshine harvester can provide 3.5[W].")



With that, the selection of power supply components and basic testing are complete. Next up is waterproofing the circuit. My hands are shaking with fear of damaging something important, but I'll give it a try tomorrow morning, good night...

## 1'. Refining Resilience: Introducing the Watchdog
When I woke up, however, the Pico had frozen—likely due to a TCP connection failure caused by something like microwave interference in the kitchen.

To improve resilience, I introduced a Watchdog Timer. It’s a simple yet powerful tool to ensure the system reboots automatically if it hangs.

When implementing it, keep these three points in mind:
 - The 8-Second Rule: On the Raspberry Pi Pico, the maximum timeout is 8 seconds. You must feed the watchdog within this window.
 - Preventing Infinite Boot Loops: If you forget to call update() after a long process, the watchdog will keep "spawn-killing" your Pico before it even starts its game.
 - Code Structure: Don’t forget to #include the necessary headers not just in haniwa_main.cpp, but also in haniwa_connector.cpp and haniwa_monitor.cpp respectively if they handle independent tasks.

Like this:
``` c++
#include "hardware/watchdog.h"
    // In a loop 
    // Long processing time of up to 5 seconds
    int state = cyw43_arch_wifi_connect_timeout_ms(
        WIFI_SSID, WIFI_PASSWORD, CYW43_AUTH_WPA2_AES_PSK, 5000
    );
    // Always feed your guard dog after a long period of work.
    watchdog_update();
```


## 2. Practical Field Waterproof Protection
- Using simple plastic cases, diatomaceous earth rings and my DIY techniques to shield circuitry from rain and salt-laden sea breezes.
- Using a Ethernet cable as 4 twisted pairs for noise-robust signal transferring.

## 3. Result: Haniwa Flashed in my Garden
- With a movie, when I come to the garden haniwa flashes its LEDs.

## Conclusions
This project is dedicated to my grandmother, who passed away last winter.

I have been clumsy with my hands since I was a child. Even now, I still struggle with soldering. It may sound like an excuse, but the truth is, I never knew my father. He left home for someone else, just before I was even born. Among my possessions today are several hand tools he left behind — water pump pliers, screwdriver sets, box wrenches — but he was never there to teach me how to use them.

My mother, fearing I might accidentally start a fire, wouldn't let me touch a soldering iron, so I didn't own one until I became an adult. Everything I know, I had to learn by doing. I moved forward through countless minor injuries and by breaking many valuable things. Even now, I find my hand shaking almost too afraid of breaking something precious.

I remember a moment from my university days while learning nano-plating. The teaching assistant grew frustrated with my clumsiness. "How can a college student fail at something so easy way?" he scolded. "The quality of students has gone down. Didn't your father ever teach you anything?"

It was the same as always; with a forced, awkward smile, I said, "I’m sorry, but I don't have a father, as you know, right?"

After that, he fell silent and stopped teaching me altogether. I never thought simply he was a bad person; it was an era where adults usually abandoned children who didn't perform well. Looking back, my retort was childish, too.

But it’s okay, no problem, I hold no grudges. Even before I was born, I think my father taught me just one thing that hating someone changes nothing. I had my grandmother, who raised me up to be bigger than a house, and I have my mother, who nurtured my ability to put experiences into words like these. My childhood was more than luxurious enough.

Just recently, I helped my daughter with her junior high school homework involving soldering. After explaining the safety and technique as carefully as I could (it might be too-much), she moved her own hands and succeeded. Do you understand what this means?

In this era of rapidly advancing AI, we cannot survive by simply passing on what we were taught. Sometimes, we must find a way to teach the new things we discovered ourselves — the things no adult ever told us.

However, perhaps — and I want to send a message of encouragement to all the highly-educated, white-collar workers who, like myself, may feel on the brink of despair jobs were taken by AI — it is only in such a changing world that we can truly discover and express our unique skills and individual personalities. Let’s continue to play this adventure game called life.

With deepest gratitude to my grandmother, who survived the bitter, dry cold of the continent and is now reading this from heaven.

## Disclaimer
 - Personal Project: I do NOT recommend getting injured. This project is a personal hobby and technical experiment. It is not affiliated with, sponsored by, or endorsed by any of the organizations mentioned.
 - Accuracy & Responsibility: While I strive for technical and historical accuracy, I assume no responsibility for any issues or damages arising from the use of the information in this article. Everything is done at your own risk. 
 - Battery Safety: This project involves the use of aged lithium-ion batteries. Please be aware that old batteries may pose risks of swelling or fire; handle them with extreme caution and monitor their condition regularly.
 - Handle with Care: Always use drills and utility knives properly to avoid injury. Most importantly, handle your soldering iron with extreme caution to prevent fires or burns.