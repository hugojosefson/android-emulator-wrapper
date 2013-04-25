# Android Emulator wrapper script

This wrapper for the Android Emulator executable works around an issue where `emulator -snapshot-list` causes a
Segmentation Fault.

## Background

This may for example happen when using Jenkins with the Android Emulator plugin.

The Jenkins plugin executes `emulator -snapshot-list ...` to find out whether to start from an existing snapshot or
create one. Then `emulator` segfaults. The Jenkins plugin interprets that to mean there are no snapshots, and it should
boot up the `avd` from scratch and create the first snapshot. This means it keeps re-creating a snapshot at every build,
instead of creating it only once, and then reusing it.

I found an issue filed against the AOSP where one of the comments suggests that some of the different `emulator-*` executables
segfault, and others do not, when supplied the `-snapshot-list` argument:
<https://code.google.com/p/android/issues/detail?id=34233#c57>

## Installation instructions

Find out where your `emulator` executable is located.

If it's a Jenkins server, look at the build log for complete path.

If it's your own developer machine, you can probably find it like this:

	which emulator

In my case, on osx and having installed the Android SDK via [brew](http://mxcl.github.com/homebrew/), it's
`/usr/local/bin/emulator`.

Rename the original `emulator` to `emulator.bak` in the same directory where it was. In my case:

	mv /usr/local/bin/emulator{,.bak} 

Then `chmod` this wrapper, and copy into the original `emulator`'s place:

	chmod +x emulator
	cp emulator /usr/local/bin/

It should now magically work!
