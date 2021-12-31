{intel, <b>amd</b>}/mesa/wayland _(/xwayland)_

note: [<b>wayland</b>](https://wayland.freedesktop.org/) is not yet fully supported; systems are in development: <br/>
nvidia proprietary drivers & hardware video accelleration are not supported <br/>
wayland is fully functional for basic applications and hardware that supports [mesa](https://mesa3d.org/) drivers<br/>

complex games, utilities and production software often come with an array of dependencies, <br/>
some of which are linked to old or different ways of doing things; <br/>

~ ~ ~

wayland is capable of faster and smoother graphics than xorg,<br/>
due to the reduction in complexity of the rendering pipeline;<br/>

that does however shift some complexity into userspace,<br/>
and tends to require patching at best or new framework;<br/>
some compatibility layers and their proper integration into the stack.

different organizations/maintainers have different priorities;<br/>
some are more motivated to support new technologies than others,<br/>
and some also have more work to do in order to make that happen.

~ ~ ~

much of the current software base designed specifically for wayland is highly configurable,<br/>
which can be quite nice, especially if you want ease of access and scriptablitiy (of keybinds, layouts, themes, etc.)<br/>
but then of course it also requires some patience and learning to use and understand it well.

with time, more general use, and with some effort, a wider variety of <br/>
configurations, utilities, and documentation will become available.

~ ~ ~

__xorg__ is still (for a little while longer) more feature-full in general,<br/>
especially in terms of utilities to assist with simplifying configuration for the end user</br>
and in terms of the sheer base of software that supports it.</br>
its more complex pipeline is also by design good at (ssh) window/screen forwarding.

three major factors in transitioning from the old to the new :
* the time & energy & motivation it takes for:
  * wayland/core developers to fully extend the frameworks to handle modern needs
  * app developers/maintainers to fully integrate with compatible technologies, such as __xwayland__
  * graphics hardware developers/manufactureres to include driver support and/or ABI's for new / open source technologies

note: the number of people and organizations involved in this proces is large,<br/>
~ so naturally it is complex and will take time.
