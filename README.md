CSVideoReverse
==============

A simple Objective-C class for creating a reversed (silent) version of a video file. Reversal occurs in its own thread, input frames are read in passes to reduce memory usage, and a delegate can be called upon completion or error.


Usage Example
-------------

``` objective-c
- (void)reverseLocalInputFile {
	NSString *inputPath = [[NSBundle mainBundle] pathForResource:@"input" ofType:@"mov"];

	// create a path for our reversed output video
	NSString *documentsPath = [NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES) objectAtIndex:0];
	outputPath = [documentsPath stringByAppendingFormat:@"/reversed.mov"];

	// get instance of our reverse video class
	CSVideoReverse *reverser = [[CSVideoReverse alloc] init];
	reverser.delegate = self;

	// if custom reader settings are desired
	reverser.readerOutputSettings = [NSDictionary dictionaryWithObjectsAndKeys:[NSNumber numberWithInt:kCVPixelFormatType_420YpCbCr8BiPlanarVideoRange], kCVPixelBufferPixelFormatTypeKey, nil];

	// now start the reversal process
	[reverser reverseVideoAtPath:inputPath outputPath:outputPath];
}

#pragma mark CSVideoReverseDelegate Methods
- (void)didFinishReverse:(bool)success withError:(NSError *)error {
	if (!success) {
		NSLog(@"%s error: %@", __FUNCTION__, error.localizedDescription);
	}
	else {
		// show the reversed video located at outputPath
	}
}
```

Licenses
--------

All source code is licensed under the [MIT-License](https://github.com/chrissung/CSVideoReverse/blob/master/LICENSE).

