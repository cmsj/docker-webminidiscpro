TODO: Write a readme covering:
 * What this is
 * Where the upstreams are
 * How to run it manually with `docker`
 * A sample `docker-compose.yaml`
 * Important notes about ps3-at3tool.exe mounting

# Docker images for Web MiniDisc Pro (and ATRAC-API)

## What is this?

For users of some old MiniDisc hardware, it’s possible to connect them to a computer via USB, to transfer music to/from the discs. The wonderful people in the modern MiniDisc community have figured out Sony’s protocols and created a web application called [Web MiniDisc Pro](https://www.minidisc.wiki/guides/webminidisc/start) that only needs a browser.

While using their hosted service is great, sometimes you might want to self-host it. They do provider a `Dockerfile`, but sometimes you might just want someone else to take care of building the docker images for you and pushing them to a registry.

That is what this repo does. It is not a fork of WMDPro - none of the application’s code lives here.

What this repo does, is simply build and publish Docker images of WMDPro (and atrac-api - see below) automatically when there are updates available.

## Which upstream repos does this watch and build?

[Web MiniDisc Pro](https://github.com/asivery/webminidisc)
[atrac-api](https://github.com/MiniDisc-wiki/atrac-api)

## How does it watch and build those repos?

See the `.github/workflows/` folder - the entire thing is orchestrated via GitHub Actions

## How do I use the images?

### Requirements

 * Docker
 * A copy of `psp_at3tool.exe` (I cannot include that file, it is owned by Sony and not licensed for distribution)

### Plain Docker commands

```bash
docker run -p 8080:8080 ghcr.io/cmsj/webminidiscpro:latest
docker run -p 5000:5000 -v /path/to/psp_at3tool.exe:/psp_at3tool.exe ghcr.io/cmsj/atrac-api:latest
```

### Docker Compose

```yaml
TBD
```

### What is `psp_at3tool.exe` and how do I find it?

This is a windows executable that was apparently part of the Sony PlayStation Portable developer kit. It’s a tool that can be used to encode and decode (codec) the ATRAC3plus audio data needed for MiniDiscs. For our purposes here, it is used by WMDPro to help convert your music files before writing them to a MiniDisc.

I probably can’t tell you exactly where to get a copy, because I don’t know at what point Sony Computer Entertainment Inc (SCEI) would sue me.

That being said, the Internet’s memory is long, and it is not very hard to track down the web’s archive of SCEI’s ATRAC3plus codec tool.
