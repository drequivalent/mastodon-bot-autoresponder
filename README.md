# mastodon group bot

This is a bot that implements group functionality in Mastodon.

This bot was based on the code developed for [mastodon.mit.edu](https://mastodon.mit.edu), where people sometimes message @mastodon, the official announcement account, but the maintainers are too lazy to actually monitor it.

The bot is written in Python 3 and uses [Mastodon.py](https://github.com/halcy/Mastodon.py) and [beautifulsoup4](https://www.crummy.com/software/BeautifulSoup/).

The bot can do the following:

- respond to every toot sent to it by a non-follower with a predefined message mentioning the admins
    - regardless of the visibility setting of the response, the response is always sent as a DM. because of how DMs work, if the predefined message includes other peple's usernames, they'll also see the DM!
- if it receives a DM from a non-follower, it can forward the text of that DM to the admins
- it can boost toots by followers that mention it

The bot will not respond retroactively, i.e., the first time you run it, it will not respond to all the messages your account has received in the past.

# Configuration

The bot is configured in a JSON file that looks like this:

```
{
    "base_url": "https://mastodon.mit.edu",
    "client_id": "0dd...65d",
    "client_secret": "a7e...6b7",
    "access_token": "9af...d33",

    "message": "Hello, I'm a group bot. Follow me and then write sonething, and I will boost it. The admins of the group are:",
    "message_welcome" : "Welcome our new member:",
    "admins": ["drequivalent"],

    "state_file": "/home/mastodon/autoresponder/state"
}
```

All keys are mandatory. The first group contains information about connecting to the API and authenticating to it. The second group contains the autoresponder message and the usernames of the admins. The last group contains the path to the state file, which contains informations that lets the bot remember which messages it's already replied to (this cannot be empty, but the file doesn't have to exist the first time you run the bot).

# Installation

This should really be packaged as a proper Python package, but I haven't done that. If you want to run this bot:

```
# 1. clone this repo
git clone git@github.com:sipb/mastodon-bot-autoresponder.git

# 2. set up a virtual environment for Python and activate it
virtualenv -p python3 env
source env/bin/activate

# 3. install the dependencies
pip install Mastodon.py==1.0.7
pip install beautifulsoup4==4.6.0

# 4. use tokentool to register the bot as an app on your server,
# then authenticate to it (don't worry, it's not hard, there's a nice
# interactive text interface)
python tokentool.py

# 5. create a config file and edit it appropriately
cp sample_config.json config.json
nano config.json

# 6. run the bot!
python autoresponder.py -c config.json
```
