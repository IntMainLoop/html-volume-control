# html-volume-control
audio volume ui-element demonstrating complex gain curves (linear, log, s-curve, inverse-sigmoid)

<img width="638" height="680" alt="image" src="https://github.com/user-attachments/assets/55ffc587-19bd-4dbd-bbda-0d7ecdeb169c" />  

Human hearing is logarithmic, so most of the time, you don't want to control the volume of an audio system using a linear ui control (such as the commonly encountered percent-delineated volume control). Unfortunately, it is extremely easy to map a dBFS volume level to a linear ui control, so lots of developers have done so to get their code working quickly, and then other people have learned from their improper examples. This demo allows developers to easily comapare different gain algorithms for their use case, hopefully inspiring more ergonomic control implementations. One of the hallmarks of a linear control assigned to dB units, is that the control often becomes extremely sensitive to the slider's position and users typically notice that '80% of the control's travel is useless.' Make no mistake, the traditional (log) function is pretty much ideal for use as a volume control, but it can also be equally problematic when trying to operate a high-power audio system at a low level. Users correctly notice that 'only the first 20% of the control's travel can be used', making an obvious solution illusive. The S-Curve was developed to make transparent fades, which it does very well, but nothing has really been optimized for the extraordinarily common purpose of selecting a playback volume and to accommodate variations in recorded level via web-designed ui controls. By inverting the S-curve's very steep sloping mid-band to create a gentile, gradually sloping mid-band, the 'Inverse-Sigmoid' control is intended to be optimized to the user's nominal playback level using the 'Pivot dB' and 'Pivot %' parameters. While likely sub-optimal for creating smooth fades, the function is shown to be capable of being optimized for various use cases, and it seems particularly well-suited to radial and slider ui controls.  
  
The following does not apply to 'endless' radial encoders, or to incremental 'paddle' style controls. It only applies to slider and radial controls with a defined start and end position:

Available Gain Functions:
  
* Linear:  
Pretty much only for mic preamps and test equipment. It also makes sense to use linear when the manufacturer uses 0-100 in their API (assuming that they are correctly applying the log conversion in their firmware).  
  
* Logarithmic:  
THE standard volume control. From radios and guitar amps to televisions and mixing boards, this is the way EEs have always done it. But there are still some serious UX problems for users who have powerful amplifiers and want to operate at low volume.
  
* S-Curve:  
Usually only found in 'fancy' mixing boards or advanced audio editing software, this is essentially two separate curves glued together. This allows for elegant fade-ins and fade-outs on high-end pro audio products.
  
* POW-n:
In progress... Coming soon.

* Inverted-Sigmoid:  
This is the inversion of each section of the aforementioned S-Curve. This is my own unique idea (AFAIK), intended to provide a better user experience while manipulating a typical web-ui slider control associated with audio volume. By defining the user's typical listening level, the majority of the slider's travel can be used for fine-tuning while the full range of the control target is accessible (exponentially) at the extreme ends of the slider.

  
IMO, this makes it much easier to casually slide my thumb across my cellphone to adjust volume without making any massive, unintentional, and sudden volume changes — and critically, it does this without losing access to the full range of the control target, as many other solutions typically require.  

----

Here are some of the values that I've found useful/interesting:
  
[90.31 dB] - "(16-bit) DR" :  
Curve: Inv-Sigmoid  
dB(min): -90.31 dBFS  
dB(max): -0.00 dBFS  
Pivot(%): ???  
Pivot(dB): -12 to -16 dBFS  (i.e., unity / ~85 dBSPL(a) )  
Notes:  More Dynamic Range than required for most music.  
  
[138.47 dB] - "(24-bit) DR" :  
Curve: Inv-Sigmoid  
dB(min): -138.47 dBFS  
dB(max): -0.00 dBFS  
Pivot(%): ???  
Pivot(dB): -14 to -21 dBFS  (i.e., unity / ~85 dBSPL(a) )  
Notes:  Way more Dynamic Range than required for most music.  
    
[Alt-Slope #1] - "60 dB Slider Control" :  
Curve: Inv-Sigmoid  
dB(min): -60 dBFS  
dB(max): -8.00 dBFS  
Pivot(%): 50%  
Pivot(dB): -20 dBFS  
Notes:  Makes slider controls easier to use without as much over/under shoot as the log and linear functions.  
Notes:  Places -40 dBFS @ 9:00, -20 dBFS @ 12:00, -14 dBFS @ 3:00  
    
[Alt-Slope #2] - "80 dB Rotary Control" :  
Curve: Inv-Sigmoid  
dB(min): -80 dBFS  
dB(max): -0.00 dBFS  
Pivot(%): 90%  
Pivot(dB): -6 dBFS  
Notes:  This distribution feels highly intuitive on the rotary control  
Notes:  Places -40 dBFS @ 9:00, -20 dBFS @ 12:00, -10 dBFS @ 3:00  
  
[Alt-Slope #3] - "Virtual-Mixing-Board Vertical Fader" :  
Curve: Inv-Sigmoid  
dB(min): -80 dBFS  
dB(max): -0.00 dBFS  
Pivot(%): 75%  
Pivot(dB): -18 dBFS  
Notes:  Places the unity position (e.g., -18 dBFS) at 75% travel and -27 dBFS @ 50% travel. While not creating any positive gain, perhaps this might be ideal for a virtual mixing board's fader?    
Notes: Using the rotary dial places -40 dBFS @ 10:00, and -18 dBFS @ 2:00 (i.e., not ideal for the rotary control)  
  
[Alt-Slope #4] - "Peak-Level Precision Rotary Control" :  
Curve: Inv-Sigmoid  
dB(min): -80 dBFS  
dB(max): -0.00 dBFS  
Pivot(%): 90%  
Pivot(dB): -1 dBFS  
Notes:  Places a vertical fader's unity position (e.g., -18 dBFS) at 50% travel.  
Notes:  Using the rotary dial places -40 dBFS @ 10:00, and -6 dBFS @ 2:00.  
Notes:  The final 10% of travel becomes highly-precise (perhaps this might be useful for audio mastering, or calibration controls?)  

["Alt-Slope #5"] - "'Half-S'/'F-curve' for 50% @ -17 dBFS (i.e., +21 dBu max)":  
Curve: Inv-Sigmoid  
dB(min): -57 dBFS  
dB(max): -0.0 dBFS  
Pivot(%): 100%  
Pivot(dB): -0.00 dBFS  
Notes: linear-ish travel across the majority of the control with a log transition to the maximum level

["Alt-Slope #6"] - "'Half-S'/'F-curve' for 50% @ -20 dBFS (i.e., +24 dBu max)":  
Curve: Inv-Sigmoid  
dB(min): -67 dBFS  
dB(max): -0.0 dBFS  
Pivot(%): 100%  
Pivot(dB): -0.00 dBFS  
Notes: linear-ish travel across the majority of the control with a log transition to the maximum level  
  
[My Yamaha AVR] :  
Curve: Inv-Sigmoid  
dB(min): -80 dBFS  
dB(max): -14 dBFS  
Pivot(%): 50%  
Pivot(dB): -40 dBFS  
Notes: The nominal system volume is now controlled using the majority of the control's travel.  
  
[My laptop] :  
Curve: Inv-Sigmoid  
dB(min): -80 dBFS  
dB(max): -8 dBFS  
Pivot(%): 50%  
Pivot(dB): -24 dBFS  
Notes:  
  
Note that:    
'Theoretical Bit-Dynamic-Range' = 20 * log10( 1 / (2^(n-1)) )    
24-bit audio = -138.47 dBFS (min)  
16-bit audio =  -90.31 dBFS (min)  
...so it doesn't make any sense to exceed these values.  
