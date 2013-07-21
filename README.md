twirk
=====

Pinboards accessible only in certain geographic locations.
by Nick Merrill

## Motivation
In my old lab building, I hardly knew anyone outside my research group, but I'm sure that everyone in that building had a joke to make about the slow-ass elevators. I'm also sure we would've appreciated a heads-up during our randomly scheduled fire alarm tests. 

Twirk's pinboards aim to create a culture around a shared, physical space.   
They can inform newcomers to a space, help regular visitors connect and communicate, and strengthen bonds among people who interact at that space often. 

Twirk's pinboards might even draw strangers to a place - by encouraging "pilgramage" to certain locations at which media can be consumed, Twirk could encourage face-to-face collisions between people with common interests. 

## Spec
Pinboards should have the media-flexibility of Tumblr and the group-thinked moderation/promotion of Reddit.

Two channels must be able to overlap in the same geographic space. For example, an amusement park might have a public pinboard for visitors, a private pinboard for the employees, and another secret pinboard that the employees' boss doesn't know about.

## Design
A user is ID'd by a global username.

Any user can create a channel. This user can invite others to moderate the channel with her. 

The moderators of a channel draw a rectangle on a map. The rectangle represents the space in which this channel (i.e., this channel's pinboard) can be accessed.

Moderators can turn a channel on or off, delete comments on a channel & delete posts on a channel. They can also delete a channel, permanently deleting all the content on it from our servers.

To make a private channel, moderators must add users to the channel.

When a user opens up the app, she sees the channels she's in.

Private channels to which she is not added are invisible to her.

If no channels are available, she is prompted to make a channel.


## Use cases
> \#southhall has a channel covering all of my department's building. #dronelab has a channel covering a room of that building, which is password-protected and/or invisible to non-members. #pirates has a private channel covering the greater part of Berkeley's campus, #southhall included.

*We support this use case.*

> \#sony wants the channel associated with their LA office to have the same content as the one in their NY office.

*We do not support this use case. Decidedly defeats the purpose of this project.*

> \#collablab their channel to include their physical lab, but also Food for Thought, the go-to sandwich place a ways down the street. They want to separate communications meant for the lab and communications meant for the lunch break. They want people to leave tips and sandwich recs, and they want to make sure these tips don't get buried under more work-crucal posts. 

> Maybe a new post in Food For Thought shows up in light gray when a user's reading the Collablab pinboard.  

*Our current design does not support this use case. It may not be a bad idea to let each channel have multiple pinboards. Perhaps users could pay for this functionality.*

> The channel #kogitruck3 moves with its truck. Just like the stickers on the truck, the pinboard is the same whether the the truck's on Sawtelle or Ventura.

*Our current design does not support this use case, but this gestures toward an excellent tool for promotion. We might sell a special GPS-enabled dongle to people with this need.*


## Design questions
+ Is joining a channel like stumbling for wifi networks? (You get a list of channels with pinboards in your location). Or should it be the responsibility of people at the location to connect a user to a channel (signage, word-of-mouth).

+ If joining a channel is like picking a wifi network, should some channels be invisible (cf. hidden SSIDs)?

+ Can a channel's pinboard be arbitrarily large or small? (Consider some end-user implications of large pinboards: easy to spam large groups of people).

+ Can pinboards be moved? (Consider selling a GPS dongle to a food truck. The #kogi channel has a pinboard in the radius around the dongle).

## Engineering questions
+ Is it feasible to let users auto-discover (stumble) onto pinboards the way people stumble onto wifi networks?

*What does this mean, exactly?*

+ What the absolute simplest way for us to describe rectangles in space? What's the absolute simplest way for us to see if coordinates lie within a rectangular region?

*Probably origin and size i.e., (lat, long) and (width, height). Hit testing would be as simple as is x <= lat + width && y <= long + height?*

+ What's the absolute simplest implementation of a pinboard? What's the least functionality it can have while still being a 100% satisfactory prototype, ready to find funding?

*A collection of post objects, each of which has a poster, content, timestamp, some comments, and maybe some kind of score (upvotes - downvotes?).*

## Implementation
Every channel has boundaries expressed as coordinates, preferences, content, etc.

However, a separate database also has the midpoints for these channels. When the app (A) is opened, it sends us its coordinates. We send A a list of channels within [max_channel_width/2]. This is to make it easy to prune the list of possible channels down to only a few candidates when someone opens the app — we don't want to check if a coordinate is inside a polygon more than we have to.

*The computational complexity of this is exactly equivalent to just calculating whether the user's location is within each channel's bounds. This doesn't really save any work.*

The pinboard associated with a channel is hosted at a URL, which is not known to users. Joining a channel loads this URL into the app's browser.

*tk.............*
