# html-volume-control
audio volume ui-element demonstrating complex gain curves (linear, log, s-curve, inverse-sigmoid)

<img width="633" height="814" alt="image" src="https://github.com/user-attachments/assets/89ad019f-0503-4e9f-aa9a-5dadc8b1dd34" />

Human hearing is logarithmic, so most of the time, you don't want to control volume with a linear control (like percent). It's just easy to map volume to a linear UI control, so lots of developers do it to make their code work quickly, and then other people learn from their improper examples.

Linear:
Pretty much only for mic preamps and test equipment. It also makes sense to use linear when the manufacturer uses 0-100 in their API (assuming that they are correctly applying the log conversion in their firmware).

Logarithmic:
The standard volume control. From guitar amps to basic mixing boards, this is the way EEs have always done it. But there are still some serious UX problems for users who have powerful amplifiers and want to operate at low volume.

S-Curve:
Usually only found in fancy mixing boards or advanced audio editing software, this is essentially two separate curves glued together. This allows for elegant fade-outs on high-end pro audio products.

Inverted-Sigmoid:
This is the inversion of each section of the aforementioned S-Curve. This is my own unique idea (AFAIK), intended to provide a better user experience while manipulating a typical web UI slider control associated with audio volume. By defining the user's typical listening level, the majority of the UI control element's travel can be used for fine-tuning while the full range of the control target is accessed (exponentially) at the extreme positions of the slider. 

For example, I personally own a Yamaha AVR with a minimum volume of -80 dB.  In the associated HTML demo, I define the -80 dB minimum level and a -14 dB max level, set the split's mid-point to 50%, and define a typical listening level of -40 dB.  The UI slider control now provides high-precision dBFS values centered around the nominal listening volume using the majority of the control's travel area, without limiting the full range of the control (unless desired).  IMO, this makes it much easier to casually slide a thumb across a touchscreen to adjust volume without making any massive, unintentional, and sudden volume changes—and critically, it does this without losing access to the full range of the control target, as many other solutions typically require.
