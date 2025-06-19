## security todo:

- add password to sudo
- fix cert owner and perms (user nginx user as owner, will require changing nginx and certbot users too + sites perms...)

## on clone:

- add inventory.ini
- duplicate .example files, remove the suffix, modify the files
- create secrets folder, put git repo read-only keys there

## censorship logs

### 18.06.25 - 19.06.25

home internet, not working:

- ECH, sites behind CF (even with TLS 1.2 - so without ECH), sites on OVH. in all cases TLS handshake works, some application data get exchanged, then the data stops coming (a bunch of TCP retransmissions without any answer)

home internet, working:

- shadowsocks (outline implementation) still works as long as the server ip address doesn't come from some providers (like OVH), ssh works even for those (OVH)

mobile, not working:

- same + SS hasn't worked for months, provider doesn't matter

mobile, working:

- xtls (vision) + reality using some providers (not OVH)

On 19.06.25, OVH and CF stopped being blocked using home internet, mobile still the same. Found some interesting bits about possible SNI whitelists, tested faking SNI on mobile - requests went through!

**conclusion**:

for **some providers** (OVH, CF, reportedly hetzner and AWS), **SNI whitelists** were probably deployed on home internet, still are on mobile.

currently, using the following setup, both work at home and on mobile:

- ruvds machine (subnet is not a part of SNI checks) with vision-reality (self-steal) + nginx website, currently trying warp as well to prevent google marking the ip as russian
- ovh machine with vision-reality stealing from stackoverflow.com (whitelisted SNI)

both solutions seem to be quite shaky: the enemy can always start checking SNI for more and more destinations, and the enemy could always start checking if any of the ips resolved from the SNI match destination ip. current possible ideas for these scenarios:

- relying on a cdn that's unlikely to be blocked (seems to be google cdn, perhaps because of android?), this would help using a fake SNI as there's a high chance that some of the whitelisted SNI's would be behind google cdn
- proxying via a russian vps - currently the checks are much more lenient for data center traffic, but not a reliable solution
- reverse proxying via a russian vps - currently no checks happen if a foreign machine makes an outbound connection to a domestic one
