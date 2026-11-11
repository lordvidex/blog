[![Netlify Status](https://api.netlify.com/api/v1/badges/f9c9c982-fcea-4248-8ffe-866b38cb6bf7/deploy-status)](https://app.netlify.com/sites/lordvidex/deploys)


# Local development

Use Hugo **0.169.0 extended** (the same version pinned in `netlify.toml`).

```sh
git submodule update --init --recursive
hugo server
```

To preview drafts and future posts, run `hugo server --buildDrafts --buildFuture`.
To generate a production build, run `hugo --gc --minify`. Output goes to `public/`.
Generated resources are ignored by Git.

# Netlify

The root `netlify.toml` pins Hugo and defines production, deploy-preview, and
branch-deploy builds. Netlify checks out the pinned theme submodules automatically.
Commit the PaperModX submodule pointer along with the config and template changes.
Production uses the configured custom domain and excludes drafts and future posts.
Pull request previews include drafts and future posts; branch deploys exclude them.
Both use Netlify's `DEPLOY_PRIME_URL` for canonical links and assets, set a preview
environment, and omit analytics and comments. Preview pages carry `noindex`.

# Theme compatibility

PaperModX is pinned to `846feebb62fcb438901c7af190678218b077e02d`.
Project templates in `layouts/` preserve the custom header, home card, covers,
sharing, and RSS feed, and adapt the theme to current Hugo APIs. The local gist
shortcode preserves existing embeds after Hugo removed its built-in shortcode.
Raw HTML is enabled for existing authored Markdown content.

PaperModX still uses LibSass. Hugo 0.169.0 builds successfully but warns that
LibSass is deprecated. A future theme migration should switch its SCSS to Dart
Sass and install Dart Sass in both local and Netlify environments before upgrading
to a Hugo release that removes LibSass.

When upgrading Hugo, update the Netlify version too, build with drafts enabled,
and check the home page, search, RSS, archives, taxonomy pages, and article pages.
