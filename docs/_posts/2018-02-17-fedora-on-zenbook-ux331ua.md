---
layout: post
title:  "Fedora 27 on Asus Zeenbook UX331UA"
date:   2018-02-17 Sat 22:00:00 +0200
categories: fedora asus
comments: true
---
I recently bought a Zeenbook UX331UA for running a dual boot Windows 10 and Fedora 27. This blog post will describe how I setup the Fedora system and my impression with it. (Spoiler: It's awesome!)

## Hardware

- CPU i7-8550U Quad core
- RAM 8GB
- SSD 512GB
- Non touch - As I don't use touch, and I the glare from a non-matt screens really irritate me.

## Installation procedure

### Backup of the original system

* Download clonezilla and burn it to a USB boot media.
* Boot into bios and turn off secure boot. I couldn't get clonezilla to boot without doing this. I could probably have turned on secure boot after running clonezilla and install fedora in secure mode, but I didn't bother doing this.
* Run clonezilla and mirror the entire disk to external media. Doing this will allow you to restore the laptop to factory settings, if you screw up, or would like to sell the computer in the future.

### Partitioning

In the past I experinced failure shrinking the NTFS file system with the Fedora installer. Perhaps it is solved now, perhaps not. But for the last few years I have instead succesfully been using the Windows program [EaseUS Partition Master](https://www.easeus.com/partition-manager/epm-free.html) for partitioning. So far without any problems. I shrinked my NTFS partition to 240GB and left the rest unpartitioned for Fedora.

### Fedora installation

Just follow the normal fedora installation instructions. The fedora installer should find the free space on the hard drive and install to it. Installation was totally smooth. Note that I choose to leave secure boot off while installing Fedora.

After installation you need to immediately update. Before doing this the system wouldn't wake up after closing the lid.

## Devices and subsystems checked 

The good news is that basically everything works on this laptop under Linux. Here is a list of checked subsystems:

* Screen - Works without any probleIm. Controllable by `xrandr`.
* 3D - Works fine. glxinfo reports OpenGL version "3.0 Mesa 17.2.4"
* audio - works out of the box
* headphone jack - works
* video camera - works
* Screenbrightness - Controllable through `/sys/class/backlight/intel_backlight/brightness` . (I am still trying to figure out how to make this user writable so that I can control the brightness by hot keys.)
* Keyboard backlighting - Controllable through `/sys/class/leds/asus::kbd_backlight/brightness` . (I have a problem with user permissions here as well)
* Keyboard - Everything works. Fn+Key generates distinct Key-codes that are easily mappable through XKB. mate-panel catches volume control buttons out of the box.
* wifi - works without any problem
* micro sd reader - (TBD - check this)
* bluetooth - scan works, but failed to connect to remote device. Not sure if I made a mistake or there is something not supported.
* suspend on lid close works - Wake up failed after initial installation, but worked after updating Fedora 27.

# Other impressions

* For light editing and browsing the computer is completly silent under Fedora. The underside of the laptop is cool and there is hardly any perceptable temperature increase under the CPU. Heavy calculation (e.g. long compilation) starts up a fan a bit, but it is not too annoying.
* On Windows, on the other hand, the fans run a lot, and make quite a bit of annoying noise. I still haven't figured out what the difference is.
* There was a review that complained of the keyboard. I have no idea what their complaint was. I like the keyboard and I feel that I can touch-type comfortably fast on it. The only annoyance is that there is no insert key, that I use for copy/paste, that the escape key is a bit small, and that to access the Menu key (which I typically map to ISO_Next_Group), you need to press the function key.
* There is no coil noise at all
* Boot is fast. After a complete shutdown from grub to complete boot takes about 13s. (I haven't tried to optimize this, though I'm sure it's possible)
* Batterlife - This is not a significant issue for me, as I'm typically not off grid for long. 

# Summary

This is a fantastic machine for running Fedora on. The combination between its weight, the screen, and its silent operation still awes me. 

{% if page.comments %}
<div id="disqus_thread"></div>
<script>

/**
*  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
*  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/
/*
var disqus_config = function () {
this.page.url = PAGE_URL;  // Replace PAGE_URL with your page's canonical URL variable
this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
};
*/
(function() { // DON'T EDIT BELOW THIS LINE
var d = document, s = d.createElement('script');
s.src = 'https://dovg.disqus.com/embed.js';
s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
                            
{% endif %}
