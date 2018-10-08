# Chooser

Using ionic-native ( https://ionicframework.com/docs/native/chooser/ ) the promise never resolve nor reject.
I thought it was an Angular/Cordova event conflict ( http://weblogs.thinktecture.com/thomas/2017/02/cordova-vs-zonejs-or-why-is-angulars-document-event-listener-not-in-a-zone.html ) but it didn't work either.

I've forked this project and implemented www/chooser.js with callbacks instead of promise and it works.
( https://github.com/wideLandscape/cordova-plugin-chooser ).
Replace \node_modules\@ionic-native\chooser\index.d.ts with the index.d.ts file in the root folder of the repo.

Tested on real device Huawei Mate 10 lite, EMUI: 5.1, Android: 7.0
If someone could test it on iOS that would be great.

## Overview

File chooser plugin for Cordova.

Install with Cordova CLI:

    $ cordova plugin add cordova-plugin-chooser

Supported Platforms:

- Android

- iOS

## API

    /**
     * Displays native prompt for user to select a file.
     *
     * @param accept Optional MIME type filter (e.g. 'image/gif,video/*').
     *
     * @returns Promise containing selected file's raw binary data,
     * base64-encoded data: URI, MIME type, display name, and original URI.
     *
     * If user cancels, promise will be resolved as undefined.
     * If error occurs, promise will be rejected.
     */
    chooser.getFile(accept?: string) : Promise<undefined|{
    	data: Uint8Array;
    	dataURI: string;
    	mediaType: string;
    	name: string;
    	uri: string;
    }>

## Example Usage

    (async () => {
    	const file = await chooser.getFile();
    	console.log(file ? file.name : 'canceled');
    })();

## Platform-Specific Notes

The following must be added to config.xml to prevent crashing when selecting large files
on Android:

```
<platform name="android">
	<edit-config
		file="app/src/main/AndroidManifest.xml"
		mode="merge"
		target="/manifest/application"
	>
		<application android:largeHeap="true" />
	</edit-config>
</platform>
```
