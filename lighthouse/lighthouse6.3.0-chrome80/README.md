# Docker image lighthouse

## Info

Inspired from [justinribeiro](https://github.com/justinribeiro/dockerfiles/tree/master/lighthouse)
More article about
- [lighthouse docker by femtopixel](https://github.com/femtopixel/docker-google-lighthouse)
- [readme lighthouse](https://github.com/GoogleChrome/lighthouse/blob/master/docs/readme.md#using-programmatically)
- [using google lighthouse to audit](https://blog.flexiple.com/using-google-lighthouse-to-audit-your-web-application/)
- [lighthouse launch](https://geekflare.com/google-lighthouse/)

## Container lighthouse

Build container
```
docker build -t lighthouse:latest .
```

Run lighthouse
```
docker run -it --rm -v ~/lighthouse:/home/chrome/reports \ 
    --cap-add=SYS_ADMIN \ 
    lighthouse:test lighthouse:latest https://google.fr \ 
    --chrome-flags="--headless" \ 
    --enable-error-reporting
```

Show versions
```
docker run -it lighthouse:latest google-chrome --version
docker run -it lighthouse:latest lighthouse --version
```

## Options chrome flags

```
  --headless
  --disable-gpu
  --remote-debugging-address=0.0.0.0
  --remote-debugging-port=9222
  --logLevel=debug
  --dump-dom
  --no-sandbox
```

## Issues

### PROTOCOL_TIMEOUT

- [src](https://github.com/GoogleChrome/lighthouse/issues/6512)

#### Problem

lighthouse_worker_module.js:1453:229 is just the line that creates this error, literally everyone running into this in devtools will have this as their stack trace. This is the correct place to track PROTOCOL_TIMEOUT.

As stated in the issue description this error happens when Chrome stops responding to Lighthouse commands. Obviously, this also means we can't get any extra information from Chrome about why this happened because Chrome wouldn't respond*. In very rare circumstances that are actually reproducible (#11124) there is something Lighthouse can do about it, but 99% of the URLs shared here do not reproduce in a clean Chrome environment, meaning it's specific to the device and there's not much we can do to investigate.

We've been thinking about ways to recover from this situation, but it will pretty much always involve some loud annoying error like this until whatever causes Chrome to stop responding is fixed.

#### Solution

You can downgrade the version of google-chrome compatible with the package lighthouse
