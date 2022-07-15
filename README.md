[![tests](https://github.com/mandrasch/ddev-simple-vite/actions/workflows/tests.yml/badge.svg)](https://github.com/mandrasch/ddev-simple-vite/actions/workflows/tests.yml) ![project is maintained](https://img.shields.io/maintenance/yes/2022.svg)

Work in progress, simple DDEV addon for vite 3 / vite 2. For usage with vite 2, see below. 

Install it via:

```bash
ddev get mandrasch/ddev-simple-vite
```

All it does is exposing the vite port via the `docker-compose.vite-simple.yaml` file, it can be accessed from outside of Docker afterwards. 


# vite 3 usage

After installing this addon you need to use the following `vite.config.js`:

```javascript
 server: {
    // respond to all network requests
    host: '0.0.0.0',
    strictPort: true,
    port: 5173
  },
```

# vite 2 usage

The default port of vite 2 is `3000`, either change it in `vite.config.js` to `5173` - or change the `docker-compose.vite-simple.yaml` file like this (needs a `ddev restart`):

```yaml
#ddev-generated
# Expose vites port to DDEV router / docker for outside access
version: '3.6'
services:
  web:
    expose:
      - '3000'
    environment:
      - HTTP_EXPOSE=${DDEV_ROUTER_HTTP_PORT}:80,${DDEV_MAILHOG_PORT}:8025,3001:3000
      - HTTPS_EXPOSE=${DDEV_ROUTER_HTTPS_PORT}:80,${DDEV_MAILHOG_HTTPS_PORT}:8025,3000:3000
```

## Example repositories

- https://github.com/mandrasch/vite-php-setup-ddev-test

## License and credits

Created via https://github.com/drud/ddev-addon-template, addon is licensed as Apache License 2.0 (Open Source) as well.

