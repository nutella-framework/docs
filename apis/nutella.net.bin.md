# nutella.net.bin (STUB)
This is the binary files counterpart of `nutella.net`. It implements methods that are similar to the ones in `nutella.net` but allow binary files (i.e. images, videos, audio and any arbitrary file) to be transferred between components.

1. `nutella.net.bin.subscribe(channel, callback(file_url, metadata)`
1. `nutella.net.bin.subscribe(filter, callback(file_url, channel, metadata))`
1. `nutella.net.bin.unsubscribe(channel/filter)`
1. `nutella.net.bin.publish(channel, binary_file, metadata)`

The first thing to notice is the absence of requests. The reason for this is that we couldn't find a suitable use case for them so we decided to leave them out.

The second thing to notice is that publish and subscribe are not "symmetrical" anymore. Publish takes a `binary_file` as an input while the callback in subscribe takes a `file_url`. This might change in the future since it was heavily inspired by the web but, native applications, might need a way of downloading the file in addition to uploading it. The other challenge the still needs to be solved is how to indicate what type of resource each URL corresponds to but that might be the place where metadata becomes helpful. 