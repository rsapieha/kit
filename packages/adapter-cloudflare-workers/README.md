# adapter-cloudflare-workers

SvelteKit adapter that creates a Cloudflare Workers site using a function for dynamic server rendering.

This is very experimental; the adapter API isn't at all fleshed out, and things will definitely change.

## Usage

Install with `npm i -D @sveltejs/adapter-cloudflare-workers@next`, then add the adapter to your `svelte.config.js`:

```js
import adapter from '@sveltejs/adapter-cloudflare-workers';

export default {
	kit: {
		target: '#svelte',
		adapter: adapter()
	}
};
```

## Basic Configuration

**You will need [Wrangler](https://developers.cloudflare.com/workers/cli-wrangler/install-update) installed on your system**

This adapter expects to find a [wrangler.toml](https://developers.cloudflare.com/workers/platform/sites/configuration) file in the project root. It will determine where to write static assets and the worker based on the `site.bucket` and `site.entry-point` settings.

Generate this file using `wrangler` from your project directory

```sh
wrangler init --site my-site-name
```

Now you should get some details from Cloudflare. You should get your:

1. Account ID
2. And your Zone-ID (Optional)

Get them by visiting your Cloudflare-Dashboard and click on any domain. There, you can scroll down and on the left, you can see your details under **API**.

Then configure your sites build directory and your account-details in the config file:

```toml
account_id = 'YOUR ACCOUNT_ID'
zone_id    = 'YOUR ZONE_ID' # optional, if you don't specify this a workers.dev subdomain will be used.
site = {bucket = "./build", entry-point = "./workers-site"}

type = "javascript"

[build]
# Assume it's already been built. You can make this "npm run build" to ensure a build before publishing
command = ""

[build.upload]
format = "service-worker"
```

It's recommended that you add the `build` and `workers-site` folders (or whichever other folders you specify) to your `.gitignore`.

Now, log in with wrangler:

```sh
wrangler login
```

Build your project and publish it:

```sh
npm run build && wrangler publish
```

**You are done!**

More info on configuring a cloudflare worker site can be found [here](https://developers.cloudflare.com/workers/platform/sites/start-from-existing)

## Changelog

[The Changelog for this package is available on GitHub](https://github.com/sveltejs/kit/blob/master/packages/adapter-cloudflare-workers/CHANGELOG.md).
