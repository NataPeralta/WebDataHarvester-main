## Cursor Cloud specific instructions

This repository is a Node.js CLI that scrapes Vea (https://www.vea.com.ar) with Puppeteer and writes products and prices into SQLite. It is not a web server and does not listen on a port. Install and the normal run command are in `README.md` (`npm install`, then `node src/index.js`).

- `node src/index.js` loads `src/scrapers/vea.js` (the file next to `base.js`). The folder `src/scrapers/vea/` is not on that path. `server/db.ts` is unused: it expects `DATABASE_URL` and a missing `@shared/schema` module. The scraper does not read `DATABASE_URL`.
- There is no lint script and no ESLint config. `npm test` is the npm placeholder: it prints `Error: no test specified` and exits 1.
- `node src/index.js` walks nine category URLs with `maxPages` 100000 and waits 2 seconds between product pages. That is a long live crawl. For a short check, construct `VeaScraper` with `maxPages` 1, point `categories` at a single URL such as `https://www.vea.com.ar/almacen`, keep one link from `productLinks`, then call `startScraping()`.
- Puppeteer launches Chrome 133 from `~/.cache/puppeteer`. `src/scrapers/base.js` already passes `--no-sandbox`. If launch fails with `Could not find Chrome`, run `npx puppeteer browsers install chrome`. A later `npm install` does not download the browser again while `node_modules` is already up to date (`node_modules` is committed).
- Persistence is the file `data/products.db`. `initializeDatabase()` creates `retailers`, `products`, and `prices`, and inserts the `VEA` retailer. A scrape updates that committed file; leave those rows out of commits.
