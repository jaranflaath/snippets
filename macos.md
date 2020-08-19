# Useful MacOS commandline tools

## Adding spacers to Dock

```
defaults write com.apple.dock persistent-apps -array-add '{"tile-type"="spacer-tile";}
killall Dock
```
