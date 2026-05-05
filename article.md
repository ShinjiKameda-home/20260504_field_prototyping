# Project: Off-grid Mobile Device Waterproof Protection for Field Prototype Tests

## Motivation: Engineering with the Seasons in Hayama
Living in Hayama on the Miura Peninsula of the Japanese archipelago, I am constantly reminded of the beauty and power of the seasons.

The incredibly warm winter sunlight that softens the harsh winds, the fierce spring gales that sweep through the coast (reminding me of the "MISTRAL" from a famous anime I watched in my schooldays), the cooling sea breeze of summer carrying the scent of the tide, and the ginkgo trees dancing in the autumn wind, each turning yellow at its own unique pace. I feel a deep sense of gratitude to live in an environment where nature's presence is so profoundly felt.

For me, the wind — whenever and wherever it blows — traces back to my childhood. Every night before bed, I loved listening to the rustling of leaves in the night breeze. It was the sound of a large poplar tree planted by my grandmother to commemorate my birth.

My grandmother was a resilient woman who met my grandfather in Dalian, Northeast China, during WWII. Just after the war (yes just AFTER that), she escaped the overwhelming Soviet too-much offensive and narrowly returned to Japan. In our small residential area in Minami-Kawachi, Osaka Prefecture, that poplar grew unnaturally tall, surpassing our living house by the time I was 17. Because it became a lightning hazard, we eventually had to cut it down.

Though the tree is gone, the "SARI-SARI" sound of its leaves remains etched in my heart — no, I should say — it shaped my heart. Today, as I face the fierce gales of Hayama, I find myself still trying to capture and understand what the sound of the wind means through my engineering.

This project is a humble attempt to harmonize technology with these natural elements. My goal is to ensure that my prototypes can survive and function as quiet observers within the same environment I call my new house.

## Project Scope
This guide focuses on two essential pillars for field deployment, explained simply for region-free beginners using common hand tools and cost-effective materials.

![images/schematic]("Schematic diagram")

## 1. Region-free Off-grid Immortal Power Solutions
To keep the Raspberry Pi Pico 2W connected to the Wi-Fi and allow my HomeServer to send real-time commands—such as making the Haniwa’s eyes flash—we must first understand the power consumption of the Wi-Fi chip (CYW43). While transmitting data naturally consumes significant power, we can save energy by putting the chip into a "doze" state where it only listens for incoming packets.Even in this power-saving mode, the system will continuously consume approximately ~20mA. If we connect it to a mobile battery with an effective capacity of 3,000mAh, the system is calculated to last for ~6days.

``` c++
    cyw43_arch_enable_sta_mode();
    cyw43_wifi_pm(&cyw43_state,
        cyw43_pm_value(CYW43_PM2_POWERSAVE_MODE, 200, 1, 1, 5));
    watchdog_update();
```

If we have even one sunny day during that 6-day period, we can keep the cycle going. Given a solar input of
$$3W / 5V = 0.6A$$
just 5 hours of warm sunlight should be enough to fully recharge the 3,000 mAh battery. With this setup, even if I were to pass away decades from now, this system would continue to draw breath, tirelessly supplying power to the Pico 2W.The Legacy Battery and the Pass-through TestI searched through my storage and found an old friend: a 6,700 mAh mobile battery made by a Japanese manufacturer many years ago.

It’s nearly a decade old and long discontinued, so it’s safer to assume its actual capacity has dropped to about half. Still, by my calculations, it should provide at least 3,000 mAh. Let’s put it to work.The first thing to check was whether this battery supports "pass-through" charging—the ability to provide power to a device while simultaneously being charged itself. 

To test this, I used a DIY bedside lamp I had previously made by stuffing an LED string light into a small wine bottle. I bought this bottle at a hotel during my very first business trip abroad—a memory I still cherish. I connected the battery to a wall outlet via USB micro-B and plugged the LED string into the battery’s USB Type-A port.

The wine bottle emitted a gentle, steady glow. Success. It was time to bring this ancient battery into my latest project.Selecting the Solar PanelTo fully charge a 3,000 mAh battery in 5 hours at a standard USB voltage of 5V, I needed a solar panel with a generating capacity of at least 3W. I decided on a 3.5W panel. It’s a modest setup, but it feels just right for this project.

## 2. Practical Field Waterproof Protection
- Using simple plastic cases, diatomaceous earth rings and my DIY techniques to shield circuitry from rain and salt-laden sea breezes.
- Using a Ethernet cable as 4 twisted pairs for noise-robust signal transferring.

## 3. Result: Haniwa Flashed in my Garden
- With a movie, when I come to the garden haniwa flashes its LEDs.

## Conclusions
This project is dedicated to my grandmother, who passed away last winter. I have been clumsy with my hands since I was a child, and even now as an adult, I always struggle with soldering. This might sound like an excuse, but the truth is, I never knew my father. I was told that he left home for someone else while I was still in my mother’s womb. Some of the hand tools I still own today — water pump pliers, screwdriver sets, box wrenches — were left behind by him, but I was never taught how to use them from him.

My mother wouldn't let me touch a soldering iron, fearing I might start a fire to our house, so I only bought one for myself after becoming an adult. Everything, I, as a young boy, had to learn by doing. I moved forward through countless injuries and by breaking many things. And even now, I am almost too afraid of breaking something precious.

Once, as a university student, while learning nano-plating from a teaching assistant (high school graduate), he grew frustrated with my clumsiness. "How can a college student not do even such an easy way?" he scolded loudly. "The quality of students has gone down. Haven't your father ever taught you anything?" Flushed with a bit of defiance, I shot back, "I’m sorry, but yes, as you said, I don't have a father." After that, he stopped teaching me altogether. I don't think he was a bad person; it was just that, in those days, the world often abandoned children who didn't perform well immediately. That's just how 'adults' were in that era. Of course, I know my retort was poorly childish, too.

But it’s okay, no problem, it doesn’t matter anymore. I hold no grudges against anyone. I had my grandmother, who raised me up until I grew bigger than a house, and I have my mother, who nurtured my ability to put my experiences into words like this. My life has been more than luxurious enough.

Just recently, I had to help my daughter with her junior high school homework involving soldering. After explaining how to use the tool as carefully as I could, (it might have become too-much safely, but) she moved her own hands and succeeded.

In this era of the AI growing up so quickly, how do you think we can survive simply passing on just what we were taught? Sometimes, we must find a way to teach even the new things we were never heard by other adults. But perhaps, I'd like to say, only in such a world, living people can truly discover and express their own diverse talents and unique individuality.

With deepest gratitude to my grandmother who survived from the extremely cold continent and now reading this in heaven.