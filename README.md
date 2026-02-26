This project's objective is to remove the Amazon video content from my FireTV. I use only a few apps like Jellyfin and Tubi. Using firetv-amazon-content.txt will block most(all?) Amazon content from showing.

Both files are recommended. firetv-blocklist.txt only contains blocks for software updated, and a few trackers. It is safe to use if you still want Amazon video to work.

Addition actions to reduce possible tracking include redirecting a couple url requests that should be only captive portal checks. The following instructions are for setting up a redirection of those queries on Openwrt:

First, create a new file to handle the ok message return:

vim /www/cgi-bin/error.cgi
 ====error.cgi====
#!/bin/sh
if [ "$REQUEST_URI" == "/generate_204" ]; then
  echo "Status: 204 No Content"
  echo ""
  exit
fi

echo "Status: 404 Not Found"
echo "Content-Type: text/html"
echo ""
cat /path/to/my/error404.html
===end error.cgi====

chmod 755 /www/cgi-bin/error.cgi

vim /etc/config/uhttpd
====uhttpd====
	option error_page  /cgi-bin/error.cgi
	option cgi_prefix  /cgi-bin
	list   interpreter ".cgi=/bin/ash"
====end uhttpd====

done
