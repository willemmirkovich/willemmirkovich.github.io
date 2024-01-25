# Willem Mirkovich Personal Website

My personal site!

## Requirements

Need `hugo` package:
```sh
brew install hugo
```

On fresh clone of this repo, also need to get `PaperMod` submodule locally:
```sh
git submodule update --init --recursive
```

## Development

Run local development server
```bash
hugo server 
```

Run local server with drafts
```bash
hugo server -D
```

## Instructions

### New Post

```bash
hugo new content/posts/[fill].md
```

## Deploy

1. `hugo --minify`

2. `git add/commit/push` 
