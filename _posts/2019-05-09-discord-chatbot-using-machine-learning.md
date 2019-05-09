---
layout: post
title: 'Getting Started with Discord ChatBot using Machine Learning'
categories: blog
tags: ['discord', 'machine-learning', 'chatbot']
excerpt: 'The post is all about getting started with the Discord Chatbox and add little bit of Machine Learning to it'
date: May 09, 2019
author: self
---

The post is all about getting started with the Discord Chatbot and add a little bit of Machine Learning to it. Getting Started with anything is the hardest part of being a developer because you need to choose which technology to use, what is the process to follow, are there any open-source solution for your problem, etc.

I am very impressed with the Discord UI and how easy it to develop a Bot in any language. When I started using Discord, I have seen a variety of Discord Bots be it Tatsumaki, Minnie, etc. Since I am proficient in .NET, I have created a simple Discord bot that says pong when the user types pong. At first, you need to install DSharpPlus which is a .Net wrapper for connecting to Discord API. You need to create a bot in [discord applications page](https://discordapp.com/developers/applications) to get the Bot Token. 

The below code shows how easy is to develop a simple bot in Discord.

```
using System.Threading.Tasks;
using DSharpPlus;

namespace MyFirstBot
{
    class Program
    {
        static DiscordClient discord;

        static void Main(string[] args)
        {
            MainAsync(args).ConfigureAwait(false).GetAwaiter().GetResult();
        }

        static async Task MainAsync(string[] args)
        {
            discord = new DiscordClient(new DiscordConfiguration
            {
                Token = "Bot Token",
                TokenType = TokenType.Bot
            });

            discord.MessageCreated += async e =>
            {
                if (e.Message.Content.ToLower().StartsWith("ping"))
                    await e.Message.RespondAsync("pong!");
            };

            await discord.ConnectAsync();
            await Task.Delay(-1);
        }
    }
}
```

Next is to add Natural Language Processing (NLP) to understand the human language to get started with ChatBot. The importance of NLP in a ChatBot is that you have to know the context of the message of the user. The NLP is a field of Machine Learning which will understand, analyze, manipulate and then generate human language thus helps to get the context of the message to reply. 

I searched for an NLP solution for .NET and found out [Stanford NLP](https://github.com/sergey-tihon/Stanford.NLP.NET), but the problem was that it was not supported in .Net Core. Then I decided to use Node.js to code my Discord Bot so that I can use [natural npm package](https://www.npmjs.com/package/natural) for Natural Language Processing. As per the package, the definition of "natural" is 

>"Natural" is a general natural language facility for nodejs. Tokenizing, stemming, classification, phonetics, tf-idf, WordNet, string similarity, and some inflections are currently supported.

Writing Discord bot in Node.js is as simple as .Net, the code to write "pong" when user types "ping" in Node.js is shown below :

```
const Discordie = require('discordie');
const Events = Discordie.Events;
const client = new Discordie();

const token = process.env.DISCORD_API_TOKEN;
client.connect({
    token: token
});

client.Dispatcher.on(Events.GATEWAY_READY, e => {
    console.log('Connected as: ' + client.User.username);
});

client.Dispatcher.on(Events.MESSAGE_CREATE, e => {
    if (e.message.author.bot) {
        return;
    }
    let content = e.message.content;
    if(content.startsWith('ping')) {
        e.message.channel.sendMessage("pong");
    }
});
```

A simple concept of Machine Learning is to make our machine intelligent, thus its time to add some intelligence to our bot i.e. train out bot. Let's say we take an example of greeting, we have to train our bot to understand every type of greeting so that it can reply back. Now not everyone greets the same way, there is a lot of different ways to greet people like hi, hello, sup, they are just a few examples. Our bot should understand all types of greeting and then greet the user back. 

Thus now we have to train our bot with a set of questions and their probable answer. A sample training data looks like below, saved in data.json:

```
"greet": {
        "questions": [
            "introduce yourself",
            "sup",
            "hi",
            "hello",
            "aka",
            "intro",
            "hey",
            "good morning",
            "good evening"
        ],
        "answer": "Hello, I am aka, ask me anything"
    }
```

Next is to train our data using the training data, for that we will use Classifier of "natural" npm package, as shown below and then save it in the classifier JSON.

```
const fs = require('fs');
const NLP = require('natural');
const classifier = new NLP.LogisticRegressionClassifier(); 

const myData = JSON.parse(fs.readFileSync("./data.json"));

Object.keys(myData).forEach((e, key) => {
    myData[e].questions.forEach((phrase) => {
        classifier.addDocument(phrase.toLowerCase(), e);
    }) 
});
classifier.train();
classifier.save('./classifier.json', (err, classifier) => {
    if (err) {
        console.error(err);
    }
    console.log('Created a Classifier file in ', filePath);
});
```

Thus I have trained my Bot, its time to use the training to talk to the user, as shown below:

```
let message = "hey";
let botAnswer = "Sorry, I'm not sure what you mean";
const guesses = classifier.getClassifications(message.toLowerCase());
const guess = guesses.reduce((x, y) => x && x.value > y.value ? x : y);
if(guess.value > (0.6) && myData[guess.label]) {
    botAnswer = myData[guess.label].answer;
}
```

So the bot will reply with the answer if the guess value is greater than 0.6 otherwise it will reply "Sorry, I'm not sure what you mean". Thus now my bot is ready to reply to the user if the user greets him.

The problem with the bot is that the reply is not natural as it will always give the same answer to all the people who asks the same question. Finding ways to improve the training data, will post once another version is ready.

Check out the first version: https://github.com/gopesh-sharma/LetsChat/releases/tag/v1.0

Thanks for reading.


