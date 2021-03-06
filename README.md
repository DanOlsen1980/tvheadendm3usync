# TVHeadEndM3USync

This is a small utility (.Net core based) for keeping your M3U IPTV services up to date with your thheadend installation.

Tvheadend has built in support for reading .m3u files and create muxes from them.

The built in feature works just fine if your mux urls does not change.

For example ,if your iptv channel url is :

http://my.site/xxx/yyy/12323

And your iptv provider changes it to :

http://my.site/xxx/zzz/12323

Tvheadend will delete the old mux and create new mux with the new url , which means you'll have to map the new service to the channel or the channel will not work.

What this tool does is altering your local m3u file (the tool work only with local m3u file) and add tvheadend's uuid tag to the m3u file so it can keep it in sync.

For example, lets say i have the following m3u file :

```
#EXTM3U
#EXTINF:-1 ,ch1
http://my.site/xxx/yyy/12323
#EXTINF:-1 ,ch2
http://my.site/xxx/yyy/12325
```
After first run of this utility, the tool will see this is new m3u links and will create network/mux for them and then will save the changes to the m3u file (will keep a backup file) :

```
#EXTM3U
#EXTINF:-1 TVH-UUID="d034dfd4bacb08183ce5a3487d46bd82",ch1
http://my.site/xxx/yyy/12323
#EXTINF:-1 TVH-UUID="8dc02b130c9c4e53dbbb746a43eff255",ch2
http://my.site/xxx/yyy/12325
```
As you can see the tool added TVH-UUID tag to each url, this UUID is generated by tvheadend as mux id.

Now , lets say our iptv provider changes the url for ch1 from 12323 to 98333 we just change our local m3u file to :

```
#EXTM3U
#EXTINF:-1 TVH-UUID="d034dfd4bacb08183ce5a3487d46bd82",ch1
http://my.site/xxx/yyy/98333
#EXTINF:-1 TVH-UUID="8dc02b130c9c4e53dbbb746a43eff255",ch2
http://my.site/xxx/yyy/12325
```

Save the changes and rerun the tool , it will compare the uuid to the mux on tvheadend and detect that there is new url and will update the same mux url to the new one (without deleting so we dont need to remap the channel).

if you want to change the mux name from ch1 , we can do it also :

```
#EXTM3U
#EXTINF:-1 TVH-UUID="d034dfd4bacb08183ce5a3487d46bd82",ch1 HD
http://my.site/xxx/yyy/98333
#EXTINF:-1 TVH-UUID="8dc02b130c9c4e53dbbb746a43eff255",ch2
http://my.site/xxx/yyy/12325
```

After running the tool the mux name will change to "ch1 HD".

If you want to delete a mux , just remove the line from the m3u and re-run the tool , the mux will be deleted from Tvheadend.

If from some reason the TVH-UUID is gone missing or you delete the mux via tvheadend GUI and re-run the tool it will re-add the mux and assign the m3u file with the new uuid value.

To keep things simple , manage your IPTV urls only by editing your local m3u file and sync them using this tool.

The tool requires 3 parameters :

1.Tvheadend url - the url to your web gui with the correct port , should be something like http://tvheadendip:9981/.

2.path to your local m3u file (do remember it will be written with uuid for each url so keep it available, its **not** one time file)

3.network name , supply the name of the network to sync/manage on tvheadend , i would advise using new network for each m3u file.

4.username if needed by your tvheadend setup. (tested only with "both plain and digest" setting on tvheadend) 

5.password if needed by your tvheadend setup.

Run it in the following way :

```
dotnet TVHeadEndM3USync.dll <tvh url> <path to m3u file> <network name> <username> <password>
```

Have fun.
