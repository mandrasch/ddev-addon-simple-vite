[![tests](https://github.com/mandrasch/ddev-addon-simple-vite/actions/workflows/tests.yml/badge.svg)](https://github.com/mandrasch/ddev-addon-simple-vite/actions/workflows/tests.yml) ![project is maintained](https://img.shields.io/maintenance/yes/2022.svg)

# ddev-simple-vite (DDEV addon)

Work in progress, simple DDEV addon for Vite 3 / Vite 2.

Install it via:

```bash
ddev get mandrasch/ddev-addon-simple-vite
ddev restart
```

All it does is exposing the Vite port via the `docker-compose.vite-simple.yaml` configuration file, it can be accessed from outside of Docker afterwards (with help of DDEV router). 

## Vite 3 usage

After installing this addon you need to use the following `vite.config.js`-settings for `server:{}`:

```javascript
// ...
export default defineConfig({
  // ...
  server: {
      // respond to all network requests
      host: '0.0.0.0',
      strictPort: true,
      port: 5173
  }
});
```

## Vite 2 usage

You need to use the same `server:{}`-config as above in `vite.config.js`. The default port of Vite 2 is `3000`. Either change it in `vite.config.js` to `5173`:

```javascript
// ...
export default defineConfig({
  // ...
  server: {
      // respond to all network requests
      host: '0.0.0.0',
      strictPort: true,
      port: 5173
  }
});
```
Or change the `docker-compose.vite-simple.yaml` file like this if you want to use `3000` (needs a `ddev restart`):

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

## Resources

- https://ddev.readthedocs.io/en/stable/users/extend/custom-compose-files/#docker-composeyaml-examples

## License and credits

Created via https://github.com/drud/ddev-addon-template, addon is licensed as Apache License 2.0 (Open Source) as well.
