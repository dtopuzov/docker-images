# docker-images

Example how to build and publish docker images in GitHub registry.

## test-container

The name of the container in this example is `test-container` and it is based on Debian 11 base image.

List of additionally installed software:

- JDK 11
- NodeJS 18
- Chromium (and Chromedriver)
- Firefox (and Geckodriver)
- [Xvfb](https://en.wikipedia.org/wiki/Xvfb) to emulate display in case you need to run tests in non headless mode
