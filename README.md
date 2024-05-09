## This add-on is not needed anymore / not maintained anymore

You can now just expose ports via `.ddev/config.yaml`, no need to use a docker-compose file anymore:

```yaml
nodejs_version: "18"
# Expose vite port
web_extra_exposed_ports:
  - name: node-vite
    container_port: 5173
    http_port: 5172
    https_port: 5173
```

See for more information: 

- https://ddev.readthedocs.io/en/stable/users/extend/customization-extendibility/#exposing-extra-ports-via-ddev-router
- https://ddev.com/blog/working-with-vite-in-ddev/

<br>
<br>
<br>
<br>
<br>
<br>



<hr>

[![tests](https://github.com/mandrasch/ddev-addon-simple-vite/actions/workflows/tests.yml/badge.svg)](https://github.com/mandrasch/ddev-addon-simple-vite/actions/workflows/tests.yml) ![project is maintained](https://img.shields.io/maintenance/yes/2022.svg)

# ddev-simple-vite (DDEV addon)


🚧 Work in progress 🚧 

Simple DDEV addon for Vite 3 / Vite 2.

Install it via:

```bash
ddev get mandrasch/ddev-addon-simple-vite
ddev restart
```

Adds [`docker-compose.simple-vite.yaml`](https://github.com/mandrasch/ddev-addon-simple-vite/blob/main/docker-compose.simple-vite.yaml) to your `.ddev` folder. This file takes care of exposing vites port through DDEV router (reverse proxy) so that it can be accessed via `https://<your-project-name>.ddev.site:5173`. See [DDEV docs](https://ddev.readthedocs.io/en/stable/users/extend/custom-compose-files/#docker-composeyaml-examples) for more information. 

ℹ️ For a more extended approach see [torenware/ddev-viteserve](https://github.com/torenware/ddev-viteserve).

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

Start vite via `ddev exec npm run dev` as usual (`ddev exec npm install` is needed beforehand of course. :wink:).

Your PHP framework needs to point to the following vite url in local development:

```bash
https://<your-project-name>.ddev.site:5173
```

The HTML output in local development should be like

```
<script type="module" src="https://<your-project-name>.ddev.site:5173/@vite/client"></script>
```

Every PHP framework (or framework plugin) does this slightly different, some even need some custom adjustments or may not be currently capable to do this out of the box. Check your browsers developer tools console / network tab for errors.

Example repositories / projects:

- https://github.com/mandrasch/vite-php-setup-ddev-test

Resources for various PHP frameworks:

- CraftCMS Vite plugin: https://nystudio107.com/docs/vite/#using-ddev
  - (you should switch `ports` with `expose` though)
- Laravel new official vite integration: https://laravel.com/docs/9.x/vite (Currently no way to set a DEV_SERVER_URL as in third party plugin https://laravel-vite.dev/. Not using `server.hmr` was suggested here https://github.com/torenware/ddev-viteserve/issues/2, but laravel vite team suggested otherwise. See: https://github.com/laravel/vite-plugin/issues/83)

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

## Acknowledgements

Thanks to @rfay, @torenware, @tyler36 (and others) for taking the time to discuss this approach in [torenware/ddev-viteserve/issues/2](https://github.com/torenware/ddev-viteserve/issues/2)!

Other approaches

- https://github.com/iammati/vite-ddev
- https://github.com/torenware/ddev-viteserve

## License and credits

Created via https://github.com/drud/ddev-addon-template, addon is licensed as Apache License 2.0 (Open Source) as well.

See [My DDEV lab](https://my-ddev-lab.mandrasch.eu/) for more DDEV-related information.
