## Installation
```
brew update
brew install mongodb
```
## Set-up
#### Create data directory where data can be saved
```
sudo mkdir -p /data/db
```
### Take ownership of the directory
```
whoami // get $USER
sudo chown $USER /data/db
```
### Start mongod (has to be done everytime you restart your laptop)
`mongod`
