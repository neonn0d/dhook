# Discohooks

A simple library to send notifications to Discord via webhooks, designed for use in React applications.

## Installation

Install the library using npm or yarn:

```
npm install discohooks
```

or

```
yarn add discohooks
```

## Usage

Simple message

```
import React, { useState } from 'react';
import DiscordNotification from 'discohooks';

const DiscordTextSender = () => {
  const [isSending, setIsSending] = useState(false);

  const sendTextToDiscord = async () => {
    setIsSending(true);
    try {
      const webhookUrl = 'https://discord.com/api/webhooks/1234567890/abcdefg'; // Replace with your webhook URL
      const discord = new DiscordNotification(webhookUrl);
      const options = {
        username: 'Custom Username',
        avatar_url: 'https://example.com/avatar.png',
      };
      await discord.send('Hello world!', options);
      alert('Message sent successfully');
    } catch (error) {
      alert('Failed to send message: ' + error.message);
    } finally {
      setIsSending(false);
    }
  };

  return (
    <div>
      <button onClick={sendTextToDiscord} disabled={isSending}>
        {isSending ? 'Sending...' : 'Send Text'}
      </button>
    </div>
  );
};

export default DiscordTextSender;
```

Simple API Response

```
import { useState } from 'react';
import DiscordNotification from 'discohooks';

export default function DiscordApiDataSender() {
  const apiUrl = 'https://reqres.in/api/users/1';
  const webhookUrl =
    'https://discord.com/api/webhooks/1234567890/abcdefg'; // Replace with your webhook URL
  const [isSending, setIsSending] = useState(false);

  const sendApiDataToDiscord = async () => {
    setIsSending(true);
    try {
      const discord = new DiscordNotification(webhookUrl);
      const response = await fetch(apiUrl);
      const data = await response.json();
      const content = `User ID: ${data.data.id}\nEmail: ${data.data.email}\nFirst Name: ${data.data.first_name}\nLast Name: ${data.data.last_name}\nAvatar: ${data.data.avatar}`;
      const embed = {
        title: 'User Information',
        color: 0x00ff00,
        fields: [
          {
            name: 'User ID',
            value: data.data.id,
            inline: true,
          },
          {
            name: 'Email',
            value: data.data.email,
            inline: true,
          },
          {
            name: 'First Name',
            value: data.data.first_name,
            inline: true,
          },
          {
            name: 'Last Name',
            value: data.data.last_name,
            inline: true,
          },
          {
            name: 'Avatar',
            value: data.data.avatar,
            inline: true,
          },
        ],
      };
      await discord.send(content, {}, embed);
      alert('Message sent successfully');
    } catch (error) {
      alert('Failed to send message: ' + error.message);
    } finally {
      setIsSending(false);
    }
  };

  return (
    <div>
      <button onClick={sendApiDataToDiscord} disabled={isSending}>
        {isSending ? 'Sending...' : 'Send API Data'}
      </button>
    </div>
  );
}
```

# Usage for Node

Import the library:

```javascript
const axios = require('axios');
const DiscordNotification = require('discohooks');

const webhookUrl = 'https://discord.com/api/webhooks/1234567890/abcdefg'; // Replace with your webhook
const discord = new DiscordNotification(webhookUrl);

const apiUrl = 'https://reqres.in/api/users/1'; // Replace with the actual API URL

async function sendApiDataToDiscord() {
  try {
    const response = await axios.get(apiUrl);
    const user = response.data.data;

    const embed = {
      color: 0x0099ff,
      title: `${user.first_name} ${user.last_name}`,
      thumbnail: {
        url: user.avatar,
      },
      fields: [
        {
          name: 'User ID',
          value: user.id,
          inline: true,
        },
        {
          name: 'Email',
          value: user.email,
          inline: true,
        },
      ],
      timestamp: new Date(),
      footer: {
        text: 'Discord Notification',
      },
    };

    await discord.send(
      null,
      {
        username: 'Custom Username',
        avatar_url: 'https://example.com/avatar.png',
        tts: true,
      },
      embed,
    );

    console.log('Message sent successfully');
  } catch (error) {
    console.error('Failed to send message:', error.message);
  }
}

sendApiDataToDiscord();
```

# Customization

You can customize the appearance of the Discord message by using "embeds." Update the send method in your index.js file to accept an optional embed parameter:

```
async send(content, options = {}, embed = null) {
  ...
}
```

Refer to the [Discord API documentation](https://discord.com/developers/docs/resources/channel#embed-object) for more information about embeds and their properties.

# Support

If you need help or have questions, please open an issue in the GitHub repository or contact neonn0d

# License

This library is released under the MIT License.
