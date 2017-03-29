---
layout: post
title: How derkonzert.de works - part 1
category: javascript,mongodb,redis,docker,graphql
---

This is the first post, on how [derkonzert.de] works. I started a big post several times, and decided to instead do it step by step, as there actually is alot about it.

## Part one, the overall setup

First of all, [derkonzert.de] is a [ReactJs application][react]. By that I mean, that everything is being rendered by React. Even the [node js server][nodejs] uses React to serve every user (logged in or anonymous) the entire page.

After that, the browser takes over and from this point on, everything is being rendered in the client.

All persistent data, like concerts, recommendations, users, etc. is being stored in a [mongo db][mongodb], which is being accessed through [mongoose object modeling][mongoose]. This is especially nice, as mongoose supports javascript promises, so you can use ES7 (or 2017?) `async`/`await` functions very easily:

```js
async function updateConcert({id, user, concertData}) {
    try {
        const concert = await Concert.findById(id).exec();

        // Only update properties, the user is allowed to change.
        allowedPropsToUpdate(user, concertData).forEach(key => {
            concert[key] = concertData[key];
        });

        await concert.save();

        await clearConcertCache(concert);

        return concert;
    } catch(error) {
        handleException(error);
    }
}
```

Additionally, there is a [redis store][redis], used to cache mongodb queries, static html output and user session keys. The last one actually is very important, since all the things are docker containers, and the nodejs app server actually is scaled up to four instances. That said, you can't use in-memory storage for sessions.  

Before your request hits one of those app instances, it is being proxied by nginx and haproxy, which handles the "load balancing".

### Getting data into React

You maybe have heard about [facebooks GraphQl][graphql]. It is a "query language for your API". If you haven't, you should definitely check it out! After having a look at GraphQl, you should check out [RelayJs][relay], which gives you the ability to connect your React components to your GraphQl endpoint.

So, to make it short: my app describes all the available data types in a GraphQl schema, which then is being used to fetch specific data. Within the schema, there are resolver functions for every field on an object type, which gives you the link to your actual DAO:

```
const concertType = new GraphQLObjectType({
    name: 'Concert',
    description: 'A concert',
    fields: () => ({
        author: {
            type: userType,
            description: 'The user, who submitted the concert',
            resolve: ({ authorId }) => getUser(authorId)
        },
        ...
    })
});
```

Now, in terms of a react component, you might have a list item, which shows a link, the bands name and the authors username:

```
class ConcertListItem extends React.Component {
    render() {
        return (
            <li>
                <a href={ this.props.concert.url }>
                    { this.props.concert.band }
                    submitted by { this.props.concert.author.username }
                </a>
            </li>
        );
    }
}
```

While creating this component, you know exactly, what kind of properties you'll need, to actually render it: the `url`, the `band` and the authors `username` of the concert.

With Relay, you are now able to build a wrapper component, that creates the required GraphQl query:

```
Relay.createContainer(ConcertListItem, {
    fragments: {
        concert: () => Relay.QL`
            fragment on Concert {
                band
                url
                author {
                    username
                }
            }
        `
    }
});
```

You're right: this does look a bit weird. I am not going into details, what this all does, but the important part is

```
fragment on Concert {
    band
    url
    author {
        username
    }
}
```

This part will result in a GraphQL request to your endpoint, which then will "fill-out" the blanks and return your desired data:

```
{
    band: 'The bands name',
    url: '/the-concerts-url-slug',
    author: {
        username: 'the authors username'
    }
}
```

This is awesome!  
I won't go more into detail, but what really makes this great, is to have your **UI components being bound to your data**. Composition gets super easy with this.

### Additional standalone services

[derkonzert.de] has two more standalone services:

The scheduler service: this is a small cron job like background application, that handles the propagation of notifications (emails, yo, etc.), clears caches and warms them up again.

The facebook bot services: this is a tiny (and very dumb) service, that handles the facebook messenger bot. I actually am not sure if I want to keep it.

### Deployment Pipeline

To automate deployments, I am running an instance of the [drone CI][drone]. I don't know much about these things, but it works. And it's fast! The entire project is being built and deployed in less than 3 minutes!

That's it. I probably forgot something.. but as an overview, this should do the trick.

[derkonzert.de]: https://derkonzert.de/
[react]: https://facebook.github.io/react/
[nodejs]: https://nodejs.org/en/
[mongodb]: https://www.mongodb.com/
[mongoose]: https://www.mongodb.com/de
[redis]: https://redis.io/
[graphql]: http://graphql.org/
[relay]: https://facebook.github.io/relay/
[drone]: https://github.com/drone/drone