# xpc_set_event_stream_handler

Consume an `com.apple.iokit.matching` event, then run the executable specified in the first parameter.

This is useful when creating `launchd` LaunchAgents that are triggered by IO events (e.g., run a script when keyboard/mouse attached). Failing to consume the `com.apple.iokit.matching` event will result in the executable being called [repeatedly](https://stackoverflow.com/questions/13987671/launchd-plist-runs-every-10-seconds-instead-of-just-once).

This isn't really documented anywhere other than the `man` page for `xpc_set_event_stream_handler`.

## Example PList

	<?xml version="1.0" encoding="UTF-8"?>
	<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
	<plist version="1.0">
	<dict>
		<key>Label</key>
		<string>com.example.KeyboardAttach</string>
		<key>ProgramArguments</key>
		<array>
			<string>/usr/local/bin/xpc_set_event_stream_handler</string>
			<string>/usr/local/bin/KeyboardAttachScript.sh</string>
		</array>
		<key>LaunchEvents</key>
		<dict>
			<key>com.apple.iokit.matching</key>
			<dict>
				<key>com.example.KeyboardAttach.Event</key>
				<dict>
					<key>idVendor</key>
					<integer>1234</integer>
					<key>idProduct</key>
					<integer>56789</integer>
					<key>IOProviderClass</key>
					<string>IOUSBDevice</string>
					<key>IOMatchLaunchStream</key>
					<true/>
				</dict>
			</dict>
		</dict>
	</dict>
	</plist>
