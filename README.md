# html-volume-control
audio volume ui-element demonstrating complex gain curves (linear, log, s-curve, inverse-sigmoid)

<img width="588" height="789" alt="image" src="https://github.com/user-attachments/assets/6d300389-aa3e-4a25-9b82-61f54756fed1" />  

Human hearing is logarithmic, so most of the time, you don't want to control volume with a linear control (like percent). It's just easy to map volume to a linear UI control, so lots of developers do it to make their code work quickly, and then other people learn from their improper examples.
  
The following does not apply to 'endless' radial encoders, or to incremental 'paddle' style controls. It only applies to slider and radial controls with a defined start and end position:
  
* Linear:  
Pretty much only for mic preamps and test equipment. It also makes sense to use linear when the manufacturer uses 0-100 in their API (assuming that they are correctly applying the log conversion in their firmware).  
  
* Logarithmic:  
THE standard volume control. From radios and guitar amps to televisions and mixing boards, this is the way EEs have always done it. But there are still some serious UX problems for users who have powerful amplifiers and want to operate at low volume.
  
* S-Curve:  
Usually only found in fancy mixing boards or advanced audio editing software, this is essentially two separate curves glued together. This allows for elegant fade-outs on high-end pro audio products.
  
* Inverted-Sigmoid:  
This is the inversion of each section of the aforementioned S-Curve. This is my own unique idea (AFAIK), intended to provide a better user experience while manipulating a typical web-ui slider control associated with audio volume. By defining the user's typical listening level, the majority of the slider's travel can be used for fine-tuning while the full range of the control target is accessable (exponentially) at the extreme ends of the slider.  
  
IMO, this makes it much easier to casually slide a thumb across a touchscreen to adjust volume without making any massive, unintentional, and sudden volume changes — and critically, it does this without losing access to the full range of the control target, as many other solutions typically require.  

----

Here are some of the values that I've found useful/interesting:
  
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
  
[Alt-Slope #1] "60 dB" :  
Curve: Inv-Sigmoid  
dB(min): -60 dBFS  
dB(max): -8.00 dBFS  
Pivot(%): 50%  
Pivot(dB): -20 dBFS  
Notes:  Places -40 dBFS @ 9:00, -20 dBFS @ 12:00, -14 dBFS @ 3:00  
  
[Alt-Slope #2] "80 dB" :  
Curve: Inv-Sigmoid  
dB(min): -80 dBFS  
dB(max): -0.00 dBFS  
Pivot(%): 90%  
Pivot(dB): -6 dBFS  
Notes:  Places -40 dBFS @ 9:00, -20 dBFS @ 12:00, -10 dBFS @ 3:00  

[Alt-Slope #3] "90.31 dB (16-bit)" :  
Curve: Inv-Sigmoid  
dB(min): -90.31 dBFS  
dB(max): -0.00 dBFS  
Pivot(%): 50%  
Pivot(dB): -12 dBFS  (unity)  
Notes:  Allows for easier selection of lower dBFS value than (log), places -12 dBFS @ 12:00 
  
[Alt-Slope #4] "138.47 dB (24-bit)" :  
Curve: Inv-Sigmoid  
dB(min): -138.47 dBFS  
dB(max): -0.00 dBFS  
Pivot(%): 50%  
Pivot(dB): -20 dBFS  (unity)  
Notes:  Allows for easier selection of lower dBFS value than (log), places -20 dBFS @ 12:00
  
Note that:    
Theoretical Bit-Dynamic Range = 20 * log10( 1 / (2^(n-1)) )    
24-bit audio = -138.47 dBFS (min)  
16-bit audio =  -90.31 dBFS (min)  
so it doesn't make any sense to exceed these values.  
