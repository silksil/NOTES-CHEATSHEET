A webhook is when a server communicates to another server after a certain event happens.

### Production & Development
- Production: When setting up a webhook in production, the data can simply be send to production URL.
- Development: Communicating it to a localhost is meaningless for a sending serving. 

### Creating domain
In order to make sure that a other server can communicate to a local production server, we can create a domain ourselves and tell that domain to communicate the incoming request to our local server (e.g. localhost: 5000). ***Localtunnel*** is a dependancy that allows you to do this.

First, install localtunnel:
```npm install localtunnel```

Then,include the following script in the package.json of your server-side:
`"webhook": "./my_webhook.sh"`

Next, add the file `my_webhook.sh` to the server root directory with the following input. Make sure to add your own url and to include the correct port
```js
function localtunnel {
  lt -s YOUR_RANDOM_URL --port 5000
}
until localtunnel; do
echo "localtunnel server crashed"
sleep 2
done
```
Lastly, give the script execution permission by running `chmod +x sendgrid_webhook.sh` in your server directory.

