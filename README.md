# Go LilyPad docker images

These images are for running the golang based lilypad server in docker containers. The 2 components are the [proxy](proxy/) and the [connect](connect/) servers. They are both multi stage build images.

If you don't want to build the images yourself, you can fetch them from docker hub. Visit [skwashd/lilypad-connect](https://hub.docker.com/r/skwashd/lilypad-connect/) and/or [skwashd/lilypad-proxy](https://hub.docker.com/r/skwashd/lilypad-proxy/).
