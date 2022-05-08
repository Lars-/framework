---
icon: IconCloud
---

# Plesk

How to deploy Nuxt to Plesk Obsidian.
Based on [this guide](https://github.com/Lars-/Nuxt3-on-Plesk).

## Prerequisites

To follow this guide you will need:

- A Plesk server with Plesk Obsidian installed
- Node.js installed on the server via the Node.js extension
- A working Nuxt.js project
- A domain added to the server (for this example, I will use example.com)

## Deployment

1. Run the following command in the root of the project: `yarn build`. This will create the `.output` directory.
2. Create a new folder called `.app` in the root of the project and copu the `.output` folder into it.
3. Create `app.json` in the `.app` folder with the following content:

```js
(() => import('./.output/server/index.mjs'))();
```

4. Copy the contents of the `.app` folder to `httpdocs` folder on the server.
5. In the Plesk control panel, go to the Node.js page.
6. Set the Document Root to `/httpdocs/.output/public` and the Application Startup File to `app.js`.
7. In the Hosting & DNS tab, go to Apache & nginx Settings.
8. Disable Proxy mode and add the following to the Additional nginx directives:

```
location ~* \.mjs$ {
   types { }   default_type "text/javascript; charset=utf-8";
}

location /api/ {
  expires 1m;
  add_header Cache-Control "public, no-transform";
}
```

That's it!

## Updating the application
When you make changes to the application, simply execute step 1 to 5 again. And then on the Node.js page, you need to restart the application.
