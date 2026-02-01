# Docker Compose Readium LCP Server

Minimal Docker Compose setup for running a **Readium LCP server** (EDRLab) alongside a **file encrypter** for the LCP encryption format.

The LCP server handles DRM and license distribution. The encrypter encrypts EPUB/PDF files by making proper Readium LCP POST requests to the server.


## Instructions

Pull repo:

```bash
git clone https://github.com/zachjesus/docker-compose-lcp-server.git
cd docker-compose-lcp-server
```

Run with Docker Compose:

```bash
docker compose up --build
```

## Services

* **lcp-server**

  * Readium LCP Server (EDRLab)
  * Runs on port `8081`
  * Internal Docker URL: `http://lcp-server:8081`

* **asset-encrypter**

  * Watches a directory for assets
  * Encrypts files via the LCP server
  * Outputs encrypted assets

## Volumes

* `assets-data`
* `assets-encrypted`

## Example (Readium LCP API)

Minimal example of encrypting an EPUB using the Readium LCP server.

```bash
mv book.epub /{WATCH_DIR} # defined in the docker compose file update to your choosing
```

```bash
curl -X POST http://lcp-server:8081/encrypt \
  -H "Content-Type: application/json" \
  -d '{
    "content": {
      "location": "/assets/book.epub",
      "type": "application/epub+zip"
    },
    "profile": "basic"
  }'
```

## Notes

* Local/dev only
* Uses test certificates
* LCP server based on:
  [https://github.com/readium/readium-lcp-server](https://github.com/readium/readium-lcp-server)
