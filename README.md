# GitHub Pages Host for AI Edge Gallery Skill

This repository hosts a custom Notion-powered skill for the Google AI Edge Gallery app at the Pages root.

## Skill URL

After GitHub Pages is enabled, use the Pages root URL in AI Edge Gallery:

```text
https://YOUR-GITHUB-USERNAME.github.io/REPO-NAME/
```

Do not point the Gallery app at the repository URL on github.com. Use the published Pages URL above.

## What this skill does

- Reads unfinished tasks from your Notion task database
- Filters rows where `Status` is not `Done`
- Uses `Date` as the due-date column
- Uses `Priority` with `High`, `Medium`, and `Low`
- Returns task pages and page content so Gemma can summarize due work and suggest 3 quick wins
- Keeps your database ID out of the public repo if you pass it through the app secret

## Secret setup

Recommended setup:

```text
secret_xxx
```

Then paste the `data_source_id` or `database_id` directly in the chat prompt.

For compatibility, the app secret can also be:

```json
{
  "token": "secret_xxx",
  "database_id": "your-private-database-id"
}
```

That second option keeps the database ID out of the public repository and out of the README, but the simpler token-only setup is more reliable in the Gallery app.

## Local structure

- `SKILL.md`: the skill definition file AI Edge Gallery looks for
- `index.html`: small landing page for humans visiting the site
- `scripts/index.html`: the hidden JS runner loaded by AI Edge Gallery
- `.nojekyll`: disables Jekyll processing so `SKILL.md` is served directly

## Publish steps

1. Create an empty GitHub repository.
2. Push this folder to the repository.
3. In GitHub, open `Settings` -> `Pages`.
4. Under `Build and deployment`, choose `Deploy from a branch`.
5. Select the `main` branch and the `/ (root)` folder.
6. Save and wait for Pages to publish.
7. Open the published site and confirm that `.../SKILL.md` loads.
8. In AI Edge Gallery, add the skill using the Pages root URL.

## Token handling

Do not commit your Notion token or database ID into this repository.

Enter them inside AI Edge Gallery when the skill asks for its secret. That keeps them out of git history and out of the hosted files.
