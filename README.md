# PS1 IGR (In Game Reset)

## Why?

Because I like coding, and i wanted to make my own IGR. Now I don't have to get up to reset my playstation. That is whether it has a PSIO or an Xstation installed. I can get back to the main menu (or reset the chosen game) with a simple key combo.
<br>
<br>

## But there's already an IGR

Yeah, have fun with that code. It works, but it requires additional wires.

<br>
<br>

Other IGR

* Command
* Controller Data
* Slave Select
* Clock
* Ground
* Reset Out
* 3.xxV
<br>
<br>

This IGR

* Controller Data
* Clock
* Ground
* Reset Out
* 3.xxV

<br>

I have nothing against it, but from someone who codes pretty often now I found the code super convoluted (like ...extremely). It's the sort of code that would make someone who has never coded, never want to try and learn to code. It attempts to follow the PS1 controller communication protocol to the absolute 'T' which is absolutely not necessary. My code is short and sweet and instead just uses the controller clock line and controller data line with a bit banging algorithm (this took a few hours to get right).

I was able to read some documentation without even taking my own scope readings and decoding them and put this together quite quickly. It started in another repo (as part of PSXTAL project) but I moved it here on it's own. Now it shows up on google search 'PS1 IGR github' results. There was originally another IGR as part of my open source DFO but that was a bit sucky and required holding down the combos for ages.

Original Repo is here as [Part of PSXTAL](https://github.com/L10N37/PSXTAL/tree/main/Stand-Alone-IGR)

<br>

Again, I can't stress enough that there is zero issue with the other IGR. My judgement is coming from a standpoint of wanting to look at the code and decipher it quickly and also, just wanting to have a go at doing it on my own.
<br>
<br>

## Combos

<br>


Short Reset: L2 + R2 + SELECT + START (back to current selected game) <br>
Long Reset:  L2 + R2 + SELECT + CROSS (back to Xstation menu)

<br>
I was basically told quite firmly that my other IGR sucked because of the length of time you had to hold the combo (though, only by one person out of many installs) and that I should use these combos. They are the same as the other IGR.

<br>
<br>


## Cost

These use an LGT8F328P which are like $1 each. The micro ones I'm about to show you on the installs don't have a USB header and are flashed with a very modified USB to Serial Adapter. You could however use something like the atmega pro micro but they cost more. You could use quite a few MCU's but a lot of them are larger.
<br>
<br>

## Flashing

I use Jaycar CAT NO. XC4464. These stupidly come with a switchable logic so you can toggle between 3.3v and 5v, but the power is locked at the 5v it receives via USB. 

It's dumb though, because

* You leave it on 5v logic and try and flash a 3.3v logic you'll fry it
* When is a 3.3v logic device ever going to be powered by 5v? If it's 3.3v powered, the logic level will be 3.3v, if it's 5v ...the logic level will be 5v. Again, you will just fry your 3.3v device by powering it with 5v, though it might survive a little longer with a 3.3v logic level before it lets out the magic smoke or just dies in the ass completely.

So, use a different one. Mine has a 3.3v regulator stuck on with a bypass cap to power devices @ 3.3v ONLY and the logic level switch is removed and hard wired for perma-3.3v logic.

You can get these for a few dollars off ali express as oppose to 25-35 from retail stores.

![alt text](/assets/images/UART.png) <br>
<br>
<br>
Anyway, now that I've finished complaining we can get to the nitty gritty. It's actually simple. Once that UART stuff is sorted out, you just paste the ino code into a blank file on arduino IDE and flash it to your IC. The only complicated part is that i use a specific library for the timing and stuff.   
<br>

That library is [here](https://github.com/L10N37/tehUberChip_Another_PSX_Modchip/tree/main/LGT8F328P%20Library)

<br>

It's the same one I use for [UberNee](https://github.com/L10N37/tehUberChip_Another_PSX_Modchip/tree/main/UberNee) which is my super dandy 100% stealth modchip, similar to PSNEE. I for one don't bother with softmods when you can put in a chip and be done with it for good. No offense intended for people who like to tonyhax it or use unirom.

<br>

Luckily, unlike some UberNee flashes, you won't need to do an SPI flash ... but you *could* ... and that's covered in the ubernee flashing PDF guide in that link. It really isn't necessary for this particular mod though.
<br>
<br>

## Installs


![alt text](assets/images/1.jpg) <br>
My first install where I didn't even consider installation difficulty. I like to superglue the board down and remove the reset button.<br>
<br>
<br>

![alt text](/assets/images/2.jpg) <br>
Here I found a completely flat surface to again, superglue the board down. This is much easier, and it's probably neater too. You won't dislodge those tiny components which any soldering noob (and even I, at times) will very, very likely accidently remove. The difference in most cases is I can re-install them again without doing any damage :P<br>
<br>
<br>

## do you have any other IGR's?

Sure thing 

<br>

[Snes & Super Famicom](https://github.com/L10N37/Atmega-SNES-IGR-)

<br>

I've considered hacking this into a NES one as well as I don't have a $500 everdrive with built in IGR. Same deal though with the SNES, some people have the expensive flash cart with this feature built in. I have a genuine x5 which I'm happy with and it's paired with this. If i was to upgrade, it would likely be to an SD2SNES which is open source and supports some of games using the expansion IC's in the cartridge.

<br>


I also hacked [someone elses code](https://github.com/LogicalUnit/N64_Interface) up into an [N64 IGR](https://github.com/L10N37/Atmega-N64-IGR-), though you won't need this if you use E-Tim's RGB kit as its incorporated. I do have an older easily moddable RGB system with my own [Super-1](https://github.com/L10N37/Super-1-RGB-bypass-for-1-chip-SNES-SFC-and-early-Nintendo-64) RGB bypass board in it, paired with this. I am honestly not 100% happy with this IGR though. I have had phantom start presses on SOME games, and very very sporadically (as in, hardly ever). Though, with an IGR you generally want 100%, the others mentioned give you that.
