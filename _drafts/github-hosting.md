title=A Quick Guide to Hosting a Static Website on Github
date=2016-04-17
type=post
status=draft
og_description=A quick guide to hosting a static website on Github
~~~~~~

1. Create a repo.
1. Push content to the `gh-pages` branch.
1. Verify `https://<user_name>.github.io/<repo_name>` published correctly.
1. Create CNAME file at root of repo. It contains the site's main URL: `mydomain.com` or `www.mydomain.com` or `subdomain.mydomain.com`.
1. To point naked domain name to this site, create two A records: set the naked domain name as the host name and the IP addresses found in [Github's documentation](https://help.github.com/articles/setting-up-an-apex-domain/#configuring-a-records-with-your-dns-provider).
1. Create subdomain (www or other): new CNAME record with subdomain as host and your root Github Pages URL (`<user_name>.github.io`) as target.