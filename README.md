This project's objective is to remove the Amazon video content from my FireTV. I use only a few apps like Jellyfin and Tubi. Using firetv-amazon-content.txt will block most(all?) Amazon content from showing.<br/><br/>
Both files are recommended. firetv-blocklist.txt only contains blocks for software updated, and a few trackers. It is safe to use if you still want Amazon video to work.<br/><br/>
Addition actions to reduce possible tracking include redirecting a couple url requests that should be only captive portal checks. The following instructions are for setting up a redirection of those queries on Openwrt:<br/><br/>
First, create a new file to handle the ok message return:<br/><br/>
vim /www/cgi-bin/error.cgi<br/>
 ====error.cgi====<br/>
#!/bin/sh<br/>
if [ "$REQUEST_URI" == "/generate_204" ]; then<br/>
  echo "Status: 204 No Content"<br/>
  echo ""<br/>
  exit<br/>
fi<br/><br/>
echo "Status: 404 Not Found"<br/>
echo "Content-Type: text/html"<br/>
echo ""<br/>
cat /path/to/my/error404.html<br/>
===end error.cgi====<br/><br/>
chmod 755 /www/cgi-bin/error.cgi<br/><br/>
vim /etc/config/uhttpd<br/>
====uhttpd====<br/>
	option error_page  /cgi-bin/error.cgi<br/>
	option cgi_prefix  /cgi-bin<br/>
	list   interpreter ".cgi=/bin/ash"<br/>
====end uhttpd====<br/><br/>
done
