# Merry

Generate a shareable stats card or animated GIF for any GitHub repository — stars, forks, days active, and a grid of stargazer avatars, styled with a laurel wreath design. Built for social sharing and README embedding.

## What it does

Paste a repository name and a GitHub token, and get back:

- A static PNG card showing repo stats and stargazer avatars
- An animated GIF version with a counting-up reveal effect
- Light and dark theme variants

## How it works

The project is a monorepo with two independent pieces that communicate over HTTP:

- **`server/`** — a Go backend that accepts a repository name and GitHub token, fetches repo and stargazer data from the GitHub API, renders the card using a headless Chrome instance (`chromedp`) against an HTML/CSS template, and returns the finished image or GIF.
- **`client/`** — a lightweight website where a user enters their repository name and token, previews the result, and downloads the generated file.

No database is used. Each request is self-contained: the token is used once, in memory, to render one result, and is never stored.

## Tech stack

| Layer | Choice |
|---|---|
| Backend | Go |
| Rendering | chromedp (headless Chrome) |
| GIF encoding | Go standard library (`image/gif`) |
| Frontend | Astro |
| Data source | GitHub REST API |

## Project structure

```
merry/
├── server/
│   ├── main.go
│   ├── handlers/
│   │   ├── generate_image.go
│   │   └── generate_gif.go
│   ├── render/
│   │   └── template.go
│   └── assets/
│       └── index.html
├── client/
│   └── (Astro project)
└── README.md
```

## Getting your GitHub token

The card requires a classic personal access token with the `public_repo` scope, since it needs to read repository stargazer data. Generate one under GitHub Settings → Developer settings → Personal access tokens → Tokens (classic).

The token is sent directly to the backend in a request body, used once to fetch the requested data, and discarded. It is never logged, cached, or written to disk.

## Status

This project is in active early development. The current focus is local, on-demand image and GIF generation. A future phase will add a live, embeddable version for README badges, using an encrypted-token pattern similar to tools like Star History.

## License

MIT