# Eldewrito-Server-Linux
Halo Online eldewrito Dedicated server Wine wrapper



# Manual Setup

Required packages - wget, wine, winetricks, p7zip, xorg-server-xvfb, aria2

Ports that need to be forwarded are TCP: 11775 - 11777 and UDP - 11774

***

This will download the Eldewrito 0.6 zip archive

`aria2c -d /some/path/ magnet:?xt=urn:btih:3c80918b35af913ac8724bb75983bc1778c35fce&dn=ElDewrito%200.6.zip&tr=udp%3a%2f%2ftracker.openbittorrent.com%3a80%2fannounce&tr=udp%3a%2f%2ftracker.opentrackr.org%3a1337%2fannounce`

This will download the required files through a torrent. Torrent is being used over direct download due much higher download speed and potential network interruptions will not disturb the download as it is quite large.

Those using a VPS may need to enable p2p traffic in order to download this way

***

After its done, do 

`7z x /path/to/ElDewrito_0.6.zip -o /the/path/you/want/for/your/server/`

This will extract the Eldewrito files, so /the/path/you/want/for/your/server/ will be the place you want your server files to be

***

Now if you are on a machine with no GUI, this step is required for Wine to work

Create a new fake display

`Xvfb :1 &`

Now we can declare this as our display later on

***

Next is the Wine setup

First we will create a new Wine prefix

`WINEPREFIX=~/.Halo wine wineboot.exe`

Once that finishes, we will setup our Wine overrides

`DISPLAY=:1 WINEPREFIX=~/.Halo winetricks wininet winhttp dotnet45 win7`

***

Next, we will run Eldorado.exe to generate the needed files

This is expected to crash, however all we need are the files it generates from a first time run

Should it not crash after a while for whatever reason, manually kill the program with Ctrl + C

`WINEPREFIX=~/.Halo wine /server/path/eldorado.exe`

Once this ends, we will have all the required files

***

Make sure to edit the file dewrito_prefs.cfg and make sure both of these settings are set to 1

`Game.SkipTitleSplash "1"
Game.SkipIntroVideos "1"`

In that same file you will find other server settings you may want to set such as voting

Your other server settings files will be located in /mods/server/

[Read this official guide on how to setup vote settings](https://www.reddit.com/r/HaloOnline/comments/8dxl4y/dedicated_server_with_voting_howto_guide/)

***

Once that is all done, you are now ready for launching the server

 `DISPLAY=:1 WINEPREFIX=~/.Halo WINEDLLOVERRIDES="mscoree=n;rasapi32=n,b" wineconsole path/to/eldorado.exe -launcher -dedicated -window -height 200 -width 200 -minimized &`

This will create a Halo Online server instance in the background, however it will still spit its output to your terminal, so usage of tmux is highly reccomended

Try connecting to your server now

The only way to send commands to the server for moderation is through Rcon. An Rcon guide will be added to this guide soon
