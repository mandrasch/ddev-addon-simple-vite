[![tests](https://github.com/mandrasch/ddev-addon-simple-vite/actions/workflows/tests.yml/badge.svg)](https://github.com/mandrasch/ddev-addon-simple-vite/actions/workflows/tests.yml) ![project is maintained](https://img.shields.io/maintenance/yes/2022.svg)

# ddev-simple-vite (DDEV addon)

Work in progress, simple DDEV addon for Vite 3 / Vite 2.

Install it via:

```bash
ddev get mandrasch/ddev-addon-simple-vite
ddev restart
```

## Vite 3

### Vite 3 configuration

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

### PHP frontend configuration

Your PHP framework then needs to point to the following vite url in local development:

```bash
https://<your-project.ddev.site>:5173
```

The HTML output in local development should be like

```
<script type="module" src="https://<your-project.ddev.site>:5173/@vite/client"></script>
```

Every PHP framework (or framework plugin) does this slightly different, some even need some custom adjustments. Check your browsers developer tools console / network tab for errors.

Resources for various PHP frameworks:

- CraftCMS Vite plugin: https://nystudio107.com/docs/vite/#using-ddev
  - (you should switch `ports` with `expose` though)

Example repositories / projects:

- https://github.com/mandrasch/vite-php-setup-ddev-test

## Vite 2

### Vite 2 configuration

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

### PHP frontend configuration

See above, same as Vite 3.

## Technical details / Resources

- All it does is exposing the Vite port via the `docker-compose.vite-simple.yaml` configuration file, it can be accessed from outside of Docker afterwards (with help of DDEV router). 
- https://ddev.readthedocs.io/en/stable/users/extend/custom-compose-files/#docker-composeyaml-examples

## Acknowledgements

Thanks to @rfay, @torenware, @tyler36 (and others) for taking the time to discuss this approach in [torenware/ddev-viteserve/issues/2](https://github.com/torenware/ddev-viteserve/issues/2)!

Other approaches

- https://github.com/iammati/vite-ddev
- https://github.com/torenware/ddev-viteserve

## License and credits

Created via https://github.com/drud/ddev-addon-template, addon is licensed as Apache License 2.0 (Open Source) as well.