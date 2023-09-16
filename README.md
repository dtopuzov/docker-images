# docker-images

Example how to build and publish docker images in GitHub registry.

## test-container

The name of the container in this example is `test-container` and it is based on Debian 11 base image.

List of additionally installed software:

- JDK 17
- NodeJS 18
- Chromium (and Chromedriver)
- Firefox (and Geckodriver)
- [Xvfb](https://en.wikipedia.org/wiki/Xvfb) to emulate display in case you need to run tests in non headless mode

## usage

```
jobs:
  build:
    runs-on: ubuntu-latest
    container:
      image: ghcr.io/dtopuzov/test-container:main
      credentials:
         username: ${{ github.actor }}
         password: ${{ github.token }}
      options: --user 1001 --cap-add=SYS_ADMIN --shm-size="4g"

    steps:
    - uses: actions/checkout@v3
```