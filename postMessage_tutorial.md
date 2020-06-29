# Sending Messages with Python

Sending messages in Slack is a great way to become familiar with [Slack apps](https://api.slack.com/start) and the chat method family. This tutorial involves a basic example using Python, but translates easily into other languages as well. By the end, you should feel comfortable implementing this feature into an app that will surely delight your users.

## What you'll need before getting started

Before you jump into this tutorial, you will need to make sure that you have an existing Slack workspace. You can either [create a new workspace](https://slack.com/help/articles/206845317-Create-a-Slack-workspace), or [sign into an existing one](https://slack.com/help/articles/212681477-Sign-in-to-Slack). It's recommended you create a new workspace for this tutorial because you'll probably be testing your app first, but either is fine. 

You will also need Python 3 installed. You may already have it installed, but to make sure, check your installation by running the following in your terminal: 

```
$ python3 --version
–> Python 3.8.2
```

If Python 3 is not installed, you will recieve this error:

```
–> bash: python3: command not found
```

Once you are sure Python is installed, you will need to create a project directory and change into it by entering the following:

```
$ mkdir PythonMessaging && cd PythonMessaging
```

Finally, you will need to open the project directory, PythonMessaging, in a text editor of your choice. 

## Creating a Slack App

Now that the prerequisites are out of the way, you can create a new Slack app [here](https://api.slack.com/apps?new_granular_bot_app=1). Give the app a name of your choosing, and pick the workspace where you will be sending messages. Click **Create App**

![Creating App](CreatingApp.png)

To give your app permission to utilize certain features, you'll need to add a [scope](https://api.slack.com/scopes). 

1. On the sidebar of your app, click on **OAuth & Permissions**

![OAuth & Permissions](Oauth.png)

2. Scroll down to **Bot Token Scopes**

![Scopes](scopes.png)

3. Click **Add an OAuth Scope**
4. Choose the `chat:write` scope to allow your app to post messages in a channel

![chat:write](chatwrite.png)


Next, you will need to install and authorize your app for your workspace. 

1. Navigate back to **OAuth & Permissions** and scroll to the top of the page
2. Click **Install App to Workspace**

![install app](install.png)

3. When prompted, click **Allow** when requested by your app for permission. 

![allow](allow.png)

Once these steps are completed, you will be given your Bot User OAuth Access Token. Make sure to save it for later and to treat it like a password!

![token](token.png)

## Building your Slack App

We can now begin building our Slack App to be able to send messages. In your project folder, create a file with the name `message_tutorial.py`.

### Requirements

To make debugging a lot easier down the road when your apps become larger, use the logging module in Python. To add the module, import it and add the following code:

```python
import logging
logging.basicConfig(level=logging.DEBUG)
```

You're also going to need the OS module to set your Bot User OAuth Access Token as an environment variable. Add the following code to import the OS module:

```python
import os
```

To store the access token in an environment variable, head to your terminal and enter the following command, replacing the value with your access token:

```
$ export SLACK_BOT_TOKEN = xoxb-...
```

For the next step, you'll need to add two modules from the Python slackclient. While still in the terminal, install the slackclient by using pip or any other preferred Python package installer. 

```
pip3 install slackclient
```
Finally, import both the `WebClient` and `SlackApiError`. Head back to `message_tutorial.py` and add the following code:

```python
from slack import WebClient 
from slack.errors import SlackApiError
```

### Sending a message

You are now ready to send a message! In `message_tutorial.py`, create a variable for the `WebClient` and pass your access token as an argument:

```python
slack_web_client = WebClient(token = os.environ["SLACK_BOT_TOKEN"])
```
Next, use a **try-except** statement for error and exception handling of the message you'll send. Create a variable inside with the name `response` that will use the `chat.postMessage` method from the `WebClient`:

```python
try:
  response = slack_web_client.chat_postMessage
```

For the `chat.postMessage` method, pass arguments for the channel where you will send your message and the text for the message:

```python
try:
  response = slack_web_client.chat_postMessage(
    channel="#your_channel_here", # Replace this string with the name of your own workspace
    text="Hello World!"
  )
```

Add the **except** statement using `SlackApiError`:

```python
except SlackApiError as e:
  assert e.response["Unable to send message"]
```

That's it! Make sure the bot is [added to the specified channel](https://slack.com/help/articles/201980108-Add-people-to-a-channel), and run `message_tutorial.py` by entering the following command in your terminal:

```
$ python3 message_tutorial.py
```



Congratulations, you've sent you're first message in Slack using Python! If you want learn more about the `chat.postMessage` method, check out the documentation [here](https://api.slack.com/methods/chat.postMessage). You can also find information about the chat method family and other method families at https://api.slack.com/web.