# Power management in Debian with cpufreq

Last modified: 11-12-2013 18:06:59

Linux allows you to save power by selecting a `governor` that automatically
sets the frequency of the CPU so that it is low when the computer is lightly
loaded and high when the load increases, such as when you are crunching
numbers. There are several such governors, but as far as I know, the
`ondemand` governor is the must useful. It gives you full power when you need
it, and immediately drops to the lowest frequency when you no longer need it.

To use the `ondemand` governor, first install the `cpufrequtils' package. Then
create `/etc/default/cpufrequtils` with the following contents.

	GOVERNOR="ondemand"

After a reboot the new governor should be in place. You can verify this by
running `cpufreq-info`.
