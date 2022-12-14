Install iRedMail from https://docs.iredmail.org/#install

Release Notes https://docs.iredmail.org/iredmail.releases.html

Setup DNS for A, PTR, MX, SPF, DKIM, DMARC https://docs.iredmail.org/setup.dns.html

Performance and Tuning: https://docs.iredmail.org/performance.tuning.html

Check banned ips: https://docs.iredmail.org/fail2ban.sql.html

```sql
use fail2ban;
select * from banned;
update banned set remove=1 where ...;
delete from banned;
```

Disable service for testing purposes: `systemctl stop fail2ban`

Autodiscover from iRedMail Easy: https://docs.iredmail.org/iredmail-easy.autoconfig.autodiscover.html

not available in iRedMail (Free)

Checkout: https://forum.iredmail.org/topic15055-autodiscover-not-working.html

and the Tool: https://rseichter.github.io/automx2/

Used ports: https://docs.iredmail.org/network.ports.html

Create Alias via SQL: https://docs.iredmail.org/sql.create.mail.alias.html

```sql
INSERT INTO alias (address, domain, active) VALUES ('user@domain.tld', 'domain.tld', 1);
INSERT INTO forwardings (address, forwarding, domain, dest_domain, is_list, active) VALUES ('user@from.tld', 'user@receipt.tld', 'from.tld', 'receipt.tld', 0, 1);
```

Customization Frontend Webmailer: https://docs.iredmail.org/webmail.customization.html

Customize SOGo HTML Frontend: https://www.sogo.nu/support/faq/how-to-customize-the-html.html

