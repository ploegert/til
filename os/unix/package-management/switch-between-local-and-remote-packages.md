# Switch between Local & remote packages

If you install a local .deb version of Edge (e.g. official 1143+ .deb, which contains the fix for the CFI issue) and then want to go back to the public edge-dev, this worked for me:\


```
# cleaned up 1143
sudo apt purge microsoft-edge-dev 

# installed 1141
sudo apt install microsoft-edge-dev
```
