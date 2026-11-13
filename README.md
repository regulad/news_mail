# News Mail

A Nextcloud app that mails each [News](https://github.com/nextcloud/news) user
their unread entries, one mail per feed, then marks those entries read in News.

News fetches new entries unread, so each feed update's new entries go out once:
at the first Nextcloud cron run after News fetches them. Whether an entry is
unread is all that decides; one marked unread again by hand is mailed again at
the next run.

- **Who gets mail:** every News user with an email address. There is no
  setting, for users or for the administrator.
- **Sender:** Nextcloud's own address, `mail_from_address` at `mail_domain`
  in `config.php`, through Nextcloud's mail settings.
- **The mail:** the feed's title as its heading; each entry's title, linking to
  the entry in News, with its author; and a button opening the feed in News. Its
  subject is `Feed: Entry title` for one entry, or `Feed: 3 new entries`.
- **What is skipped:** entries a feed's keyword filter hides, and users who are
  disabled or have no email address.
- **Failures:** an entry is marked read only once its mail has gone out, so a
  run that fails tries again at the next only what was not mailed. Failures are
  logged to Nextcloud's log.

It runs as a background job at every Nextcloud cron run, so Nextcloud's
background jobs must run by cron or webcron, as News's own updater needs too.

## Requirements

- Nextcloud 35
- News, enabled. News Mail does nothing while News is disabled.

## Installing

Not in the App Store. Place this repository in an apps directory as `news_mail`,
the app's ID, then enable it:

```sh
git clone https://github.com/regulad/news_mail.git /var/www/html/custom_apps/news_mail
sudo -u www-data php occ app:enable news_mail
```

Only `appinfo/` and `lib/` are the app; the rest of the repository can be left
out of the copy.

## License

[AGPL-3.0-or-later](LICENSE.md).
