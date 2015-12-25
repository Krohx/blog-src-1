Title: Slack is Tweak-friendly
Date: 2015-12-25 23:23
Tags: slack, configuration, json, general, backup, communication, planet
Slug: migrating-slack-channel-general
Summary: Slack gives you a platform for communication, but includes in the package, control over your data.




# Slack is Tweak-friendly

> How I migrated the `#general` channel on Slack.

[Slack](http://slack.com) is a communication platform built for teams. While it is free by default, there are features reserved for paid users. However, the free version already comes with a ton of cool features, and is highly usable as is, especially for teams that are relatively small. My team has stuck with this unpaid version of the application for a few months.

Recently, we decided to have our clients join our Slack team, with some envisaged benefits:
 - It would ease the process of keeping the client up-to-date on project progress
 - It would bring our clients to our preferred communication platform, bringing them all in one place
 - We could leverage some of the many tools available for use with Slack to improve our communication with clients. It's worth noting that Slack is very API friendly ;)


In doing this, we were presented with a small challenge. All of our previous conversations which had mostly being in the public, general channel, `#general`, were now open to our newly signed-on client. Some of these conversations obviously included stuff we would rather be more privy about. This meant I had to quickly find a way to _migrate_ -- I chose not to _delete_ -- the messages in the hope that he had not scrolled back through the channel's message log.


This would have been easier if Slack allowed certain modifications to the default channel provided to every team. This channel is also by default named `#general`<sup>__[1]__</sup>. Slack prevents the deletion of this channel.

Also, private channels on Slack have some restrictions:
 - They can't be [_deleted_, but can be _archived_](https://get.slack.help/hc/en-us/articles/213185307-Deleting-a-channel).
 - They can't be changed to _public_ channels, but the reverse is possible.

Nonetheless, the following explains my approach to tackling the problem.

First I used Slack's [export](https://get.slack.help/hc/en-us/articles/201658943-Exporting-your-team-s-Slack-history) feature to backup all of our conversations, including channel information. A caveat here is, _Slack does not include private channels' data with exported data by default_<sup>__[2]__</sup>, but thankfully, we hadn't any private rooms at the time.

Now, Slack exports team data in JSON format, making it very easy to work with. Having downloaded the exported data, I started looking through it for parts that I could tweak.

There were some top-level (__root folder__) files which included metadata about users, teams and channels. Then there were also folders for each channel, in which, conversation logs were kept, grouped by days.

Next, I edited the `channels.json` file in the top-level directory, and changed the name of the channel `#general`, which contained the messages, to some new name. Then I went and renamed the folder named `general`, to this new name.

With this changes in place, I compressed the modified files alongside the unmodified ones into a _zip_ archive, just as they were when they were downloaded from Slack. Slack allows you to [import](https://get.slack.help/hc/en-us/articles/201748703-Importing-message-history) this zipped file into your Slack team, which was what I did next.

During the import, Slack automatically tried to match detected channels (from the import) with existing ones, afterwhich I was presented with a page that showed the mapping suggestions. I simply set it not to map but create new channels instead.

Upon successful completion of the imports, I went on to delete all the messages in the existing `#general` channel. Those messages where now the contents of a newly minted channel, created using the new name I previously gave to it. The only things left were:
 1. I marked the new channel as _private_, and
 2. I welcomed out new client to the team ;) 


<br/>
__ __

#####[1] More on Slack's default channel - `#general` 
 - https://get.slack.help/hc/en-us/articles/201827866-Renaming-the-general-channel
 - https://get.slack.help/hc/en-us/articles/201898998-Setting-default-channels-for-new-users

#####[2] Exporting private data from Slack()
 - https://get.slack.help/hc/en-us/articles/204897248-Understanding-Slack-data-exports