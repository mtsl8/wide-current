{intel, <b>ati</b>}/mesa/wayland//xwayland]

note: [<b>wayland</b>](https://wayland.freedesktop.org/) is not yet fully supported; systems are in development: <br/>
nvidia prop. drivers & hardware video accel. not supported <br/>
fully functional for basic applications <br/><br/>
complex games and production software come with an array of dependencies, <br/>
some of which are linked to the old ways of doing things; <br/>
whenever available, the updated deps. are chosen. <br/>

~ ~ ~

wayland is capable of faster and smoother graphics than xorg,<br/>
due to the reduction in complexity of the rendering pipeline;<br/>

that does however shift some complexity into userspace,<br/>
and tends to require patching at best or new framework;<br/>
some compat. layers and their proper integration.

different organizations/maintainers have different motivations;<br/>
some are more motivated to support wayland than others.<br/>
some have more work to do to make that happen.

much of the current base software designed for wayland is highly configurable,<br/>
which can be quite nice, especially if you want ease of access and scriptablitiy of keybinds, themes, etc.<br/>
but then of course it also necessitates some degree of learning and understanding to use properly.

with time and use (and effort), more<br/>
configurations, utilities, and documentation will become available

~ ~ ~

xorg is still (for a little while longer) more feature-full in general,<br/>
especially in terms of utilities to assist with simplifying configuration</br>
and in terms of the sheer base of software that supports it.</br>
its more complex pipeline is also - by design - good at (ssh) window/screen forwarding.
