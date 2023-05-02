# Linux Server Documentation

A small collection of simple instructions for setting up and configuring a Linux
server from scratch.

See the published docs at
https://travishorn.github.io/Linux-Server-Documentation/

## Development

To run these docs locally, clone this repository

```bash
git clone https://github.com/travishorn/Linux-Server-Documentation
```

Change into the repo directory

```bash
cd Linux-Server-Documentation
```

Install dependencies

```bash
npm install
```

Run the dev server

```bash
npm run dev
```

The site is served at http://localhost:5000/Linux-Server-Documentation

Modify markdown files in the repository. The site will automatically be
hot-reloaded.

## Deployment

Modify `retype.yml` and set the `url` to the URL where the site will be served.

### Using GitHub Pages

Pushing this repository to GitHub will automatically invoke a GitHub Action
which will create a new branch called `retype`.

Visit your repository on GitHub. Go to **Settings** > **Pages**. Under
**Source**, choose the `retype` branch and click **Save**.

Your site will be live at https://[username].github.io/[repo name] shortly.

### Using other static hosting

Build the site

```bash
npm run build
```

Built files are created in a directory called `.retype`. You can publish those
files to a static web server.

## License

Copyright 2023 Travis Horn, licensed under [CC BY
4.0](http://creativecommons.org/licenses/by/4.0/)
