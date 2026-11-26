# Muhammad Umair — Hugo Blog

A personal Hugo blog for **Muhammad Umair**, a Computer Science student at FAST NUCES. The site includes nine original articles inspired by the subject areas shown in the supplied reference screenshot. The wording and examples are newly written rather than copied from the reference author's articles. No personal photograph is included.

## Repository and live address

Your GitHub username is `nuvionix`, so the GitHub Pages repository for a user site must be named **`nuvionix.github.io`**. After deployment, the expected address is: `https://nuvionix.github.io/`. The reference address `muhammad-anfaas.github.io` belongs to a different GitHub account and cannot be used as your own default Pages address.

## Publish

1. Create a new public GitHub repository named `nuvionix.github.io` under your `nuvionix` account.
2. Extract this ZIP file. Open a terminal in the extracted `umair-hugo-portfolio` folder.
3. Run the following commands:

   ```bash
   git init
   git branch -M main
   git add .
   git commit -m "Add my Hugo portfolio and blog"
   git remote add origin https://github.com/nuvionix/nuvionix.github.io.git
   git push -u origin main
   ```

4. On GitHub, open **Settings → Pages** and select **GitHub Actions** as the build and deployment source.
5. Open the **Actions** tab and wait for the “Deploy Hugo site to GitHub Pages” workflow to finish successfully.
6. Visit `https://nuvionix.github.io/`. The first deployment may take a few minutes.

## Local preview

Install Hugo (extended version recommended), then run:

```bash
hugo server
```

Open the local address printed by Hugo. To create the production output locally, run `hugo --minify`; generated files appear in `public/`.

## Add another post

Create a new page bundle under `content/posts/your-post-name/index.md` and add Hugo front matter at the top. Each article has its own folder.

## Profile links

- GitHub: https://github.com/nuvionix
- LinkedIn: https://www.linkedin.com/in/muhammad-umair-285414430/
- Email: p250627@pwr.nu.edu.pk

Review the LinkedIn URL in `hugo.toml` if you want to change it.
