<div align="center">
  <h1>Moonramp Chains</h1>

  <image src="./docker-logo.png">

  <h2>Production grade docker images for BTC, BCH, XMR, and ETH</h2>

  <div><p>
  	Non-privileged minimalist docker images
  </p></div>

  <div><p>
    <a href="https://github.com/MoonRamp/moonramp-chains/actions/workflows/docker-images.yml">
      <img src="https://github.com/MoonRamp/moonramp-chains/actions/workflows/docker-images.yml/badge.svg"/>
    </a>
    <a href="https://hub.docker.com/repository/docker/moonramp/bitcoin">
      <img src="https://img.shields.io/docker/v/moonramp/bitcoin?label=bitcoin&color=yellow"/>
    </a>
    <a href="https://github.com/moonramp/moonramp-chains/blob/master/LICENSE">
      <img src="https://img.shields.io/github/license/moonramp/moonramp"/>
    </a>
  </p></div>
</div>


## Image Design

* Non-root user (moonramp:moonramp uid:1337 gid:1337)
* Multi-stage build with scratch based final layer for the bare essentials
* Official sha256 verified bins

### Bitcoin (BTC)

The BTC image is based off [Bitcoin Core](https://bitcoin.org/en/download). All bitcoincore bins and sh are included in the image.
