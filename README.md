# selenium-gost-browser
Implementation of Selenium browsers with support for GOST encryption

Selenium WebDriver is a tool for automating web browser actions. It's primarily used for testing web applications, but it's not limited to that.

There's a limited selection of browsers that are already packaged in Docker images and available for out-of-the-box deployment: Firefox, Chrome, Opera, and Edge.

### Usage

`git clone https://github.com/KubLda/selenium-gost-browser`

`cd selenium-gost-browser`

**NodeChromium or NodeYandex**

`cd NodeChromium`

`docker build -t node-chromium:146 ./NodeChromiumGost/`

`docker compose up -d`
