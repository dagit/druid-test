This is a simple demo to test using winit (for accelerated rendering) along side druid for normal UI elements. These two libraries cannot coexist noramlly in one program as they both need to execute a message handling loop on the process's original thread. Mostly this in an issue for macOS and probably works fine on windows. I tested linux and it wasn't an issue there.

To work around this limitation:

1. `main()` immediately performs a double `fork()` (double forks make it so you don't accidentally get zombie proceses)
2. The parent process runs winit
3. The child process runs druid
4. The parent and child exchange ipc channels for sending and receiving messages

The demo is based on copying the multiwindow example from the druid sources and the basic triangle example from the glium sources.

TODO: Other things to add to the demo

    * [ ] Right-click context menu on the winit side to hide/show the druid window
    * [ ] Move the message sending/receiving into the event loops in a non-blocking way
    * [ ] When the winit window is closed send a `Quit` message to the druid side and exit
    * [ ] Figure out why the macOS version is having an issue with cleanly exiting. On closing the windows there is a panic that's probably something I'm doing wrong.
    * [X] Test on linux
    * [X] Test on macOS
    * [ ] Test on windows
