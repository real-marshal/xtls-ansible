## nginx workflow

nginx doesn't start if some servers try to listen on 443 but don't have certs for them, thus there are a few ways to configure let's encrypt certs with nginx:

1. change the config to listen on 80 only, start nginx, request the cert, update the config to listen on 443 too, restart nginx. renewals will happen through nginx with a cron/timer unit.
2. same as 1 but instead of changing the config, provide another one, very simple, for the initial cert request.
3. provide snakeoil (fake) cert to start nginx on both 80 and 443, start nginx, request the right cert, restart nginx. renewals happen in the same way as with the methods above.
4. request the cert in standalone mode (separately without relying on nginx), start nginx. renewals with certbot will try to work standalone too, failing to bind if nginx listens on 80 (e.g. for redirects), might be possible to change this?

when i presented these to gemini, it assured me that 2 is the way to go. i mean, sure, whatever.
