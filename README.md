# Docker images for Web MiniDisc Pro (and ATRAC-API)

## What is this?

For users of some old MiniDisc hardware, it’s possible to connect them to a computer via USB, to transfer music to/from the discs. The wonderful people in the modern MiniDisc community have figured out Sony’s protocols and created a web application called [Web MiniDisc Pro](https://www.minidisc.wiki/guides/webminidisc/start) that only needs a browser.

While using their hosted service is great, sometimes you might want to self-host it. They do provider a `Dockerfile`, but sometimes you might just want someone else to take care of building the docker images for you and pushing them to a registry.

That is what this repo does. It is not a fork of WMDPro - none of the application’s code lives here.

What this repo does, is simply build and publish Docker images of WMDPro (and atrac-api - see below) automatically when there are updates available.

## Which upstream repos does this watch and build?

[Web MiniDisc Pro](https://github.com/asivery/webminidisc)
[atrac-api](https://github.com/MiniDisc-wiki/atrac-api)

## How do I use the images?

### Requirements

* Docker
* A copy of `psp_at3tool.exe` (I cannot include that file, it is not licensed for distribution)

### Plain Docker commands

```bash
docker run -p 8080:8080 ghcr.io/cmsj/webminidiscpro:latest
docker run --hostname atrac-api -p 5000:5000 -v /path/to/your/psp_at3tool.exe:/psp_at3tool.exe:ro ghcr.io/cmsj/atrac-api:latest
```

### Docker Compose

```yaml
version: "3.8"

services:
  wmdpro:
    hostname: wmdpro
    image: ghcr.io/cmsj/webminidiscpro:latest
    ports:
      - "8080:8080"

  atrac-api:
    hostname: atrac-api
    image: ghcr.io/cmsj/atrac-api:latest
    volumes:
      - /tmp/psp_at3tool.exe:/psp_at3tool.exe
```

### How do I configure WMDPro to use the atrac-api?

* Load WMDPro in your browser (on port `8080` if you used the above Docker commands unmodified)
* Click on the three dots at the top right of the page, select `Settings`
* For "`LP/HiMD encoder to use`", select "`Remote ATRAC Encoder`"
* Enter `http://atrac-api:5000` (If you've changed the networking setup of the Docker instances, the hostname `atrac-api` might not work)

### What is `psp_at3tool.exe` and how do I find it?

This is a windows executable that was apparently part of the Sony PlayStation Portable developer kit. It’s a tool that can be used to encode and decode (codec) the ATRAC3plus audio data needed for MiniDiscs. For our purposes here, it is used by WMDPro to help convert your music files before writing them to a MiniDisc.

I can’t tell you exactly where to get a copy, because Sony Computer Entertainment Inc (SCEI) never licensed it for third party distribution.

That being said, the web's memory is long, and it is not very hard to find where to download psp_at3tool.exe.

## Why would I want to run this myself?

Maybe you want to use it offline, maybe you want it to be faster than using the version hosted by the MiniDisc Wiki, maybe you just want to. That's up to you!

## How does this repo watch and build those repos?

See the `.github/workflows/` folder - the entire thing is orchestrated via GitHub Actions
