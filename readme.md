# Vite 3 Bug - HMR First Dev

HMR doesn't work the first time the dev server is started.

Reproduction steps:

1. ```bash
   git clone git@github.com:brillout/vite-3-bug_hmr-first-dev
   cd vite-3-bug_hmr-first-dev/
   pnpm install
   pnpm run dev
   ```

   Same as single line (copy-paste me):

   ```bash
   git clone git@github.com:brillout/vite-3-bug_hmr-first-dev && cd vite-3-bug_hmr-first-dev/ && pnpm install && pnpm run dev
   ```

2. Go to [http://localhost:3000/](http://localhost:3000/). Observe in the terminal the following message:
   ```
   3:06:09 PM [vite] ✨ new dependencies optimized: .pnpm/vite@3.0.0-beta.0/node_modules/vite/dist/client/client.mjs
   3:06:09 PM [vite] ✨ optimized dependencies changed. reloading
   ```

3. Wait for the page to finish loading. Then increment the counter button. The button text is now `Counter 1`.

4. Edit `/pages/index/index.page.jsx`. E.g. change `Welcome` to `Welcome !`. Observe that a) the page is reloaded and the counter is reset to `Counter 0`, and b) the terminal says:
   ```
   3:06:21 PM [vite] page reload pages/index/index.page.jsx
   ```
   In other words: HMR doesn't work.

5. Shut down the server (hit Ctrl-C in the terminal), re-start the server (`pnpm run dev`), and go to [http://localhost:3000/](http://localhost:3000/) again.

6. Again: click on the counter button to get `Counter 1`, edit `/pages/index/index.page.jsx`, and observe how HMR now works: a) the page is not reloaded and the counter state `Counter 1` is preserved, and b) the terminal says:
   ```
   3:05:47 PM [vite] hmr update /pages/index/index.page.jsx
   ```

To re-reproduce the bug remove `node_modules/.vite/` and try again.

Conclusion: HRM is broken when `node_modules/.vite/` has not already been generated.
