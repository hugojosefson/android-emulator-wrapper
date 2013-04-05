# Android Emulator wrapper script

This wrapper for the Android Emulator executable works around an issue where `emulator -snapshot-list` causes a Segmentation Fault. This may for example happen in Jenkins with the Android Emulator plugin, which causes it to never recognize that an avd already has a snapshot, so it keeps recreating a snapshot at every build instead of creating it only once and then reusing it.

## Instructions

Find out where your `emulator` executable is located.

If it's a Jenkins server, look at the build log for complete path.

If it's your own developer machine, you can probably find it like this:

	which $(emulator)

In my case, on osx and having installed the Android SDK via [brew](http://mxcl.github.com/homebrew/), it's `/usr/local/bin/emulator`.

Rename the original `emulator` to `emulator.bak` in the same directory where it was. In my case:

	mv /usr/local/bin/emulator{,.bak} 

Then `chmod` this wrapper into its place:

	chmod +x emulator
	cp emulator /usr/local/bin/

It should now magically work!
