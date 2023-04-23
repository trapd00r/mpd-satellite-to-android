Using mpd to stream all the music at home to a phone without httpd streaming
-----------------------------------------------------

This is what my setup roughly looks like:

```
nfs server (debbie) -> mpd server (endie) <-> mpd server (mpd on phone) -> mpd client (M.A.F.A)
```

I have a wireguard server setup at home, which means that the playback is never
interupted no matter where I am.

The setup is as follows:

First, I have a nfs server running on one of my servers, which exports the music
directory. There are different rules for different clients (rw, ro etc).

This is mounted on another server which runs mpd in satellite mode. This mpd
never actualy plays any music, so it doesn't need any output defined.

Next, I have a mpd server running on my phone, which is configured to use the
satellite server as relay. Check the configuration files for details.

Finaly, M.A.F.A is configured to use the local mpd server.

---

 The /etc/exports on nfs server:

 ```
/mnt/music8 192.168.1.*(ro,insecure,sync,no_subtree_check,anonuid=1000,anongid=1000)
```

Use the builtin ftp server in solid explorer, or any other means (usbc cable...)
to add the mpd.conf_android file to the Android/data/org.musicpd/files/
directory (named mpd.conf)


The config location was changed as detailed here:

https://github.com/MusicPlayerDaemon/MPD/commit/40bc60d6ae7c722516d357dcb59d98c72bf836bf#diff-b61f200706313226d75a19bd12ab2eb66d744fa5373d0ff66d03e361bcad58d8R39


If your server and phone can handle it, it's highly recommended to have a large number as input_cache, as noted in the conf.
