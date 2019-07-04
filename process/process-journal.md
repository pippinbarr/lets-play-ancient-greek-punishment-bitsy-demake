# Initial thoughts, Bitsy thoughts, Aesthetics, Specific levels (Wednesday, 3 July 2019, 21:51PM)

## Initial thoughts

Because I'm on vacation more or less and that means roughly full-time parent life, which is nice, I need something relatively small and straightforward to poke at in the evenings. The Twine was pretty satisfying design-wise and only required a day to implement, so it makes sense to me to turn to the "other" extremely simple and clean game engine that gets used by new creators especially, Bitsy.

I've used Bitsy exactly one time before to make b r 1 and really enjoyed the experience and the challenges associated with working with the limitations of 2D, specific tile-sets, particular avatar representations, and so forth. Bitsy is hyper limited in a way that I think could be quite rewarding to work with on these punishment games, especially its extreme lack of interactivity.

## Bitsy thoughts

So if the primary question is around interactivity and what I can represent for a player to engage with then

- Spatial navigation (technically the space is implied by the tiles etc., but it's the usual way of using Bitsy so I roughly assume I'll stick with this concept in order to stay true to the form). You move an avatar around in the space of a "room" and it can collide with "walls".
- Triggers. If I recall correctly Bitsy has the ability to define tiles that take you to other rooms on overlap, allowing you to do all kinds of stuff potentially. Most obviously travel to a new location, but also to portray a change in state.
- Items. You can "pick things up" in Bitsy. That is, you can overlap them, they disappear, and you can display a message associated with this event.
- NPCS. You can bump into something (wall-like) and it displays a message (over and over).
- Plugins. Alonside those basics there, I became aware through a talk by Enric Llagostera that there are tons of extensions of Bitsy that allow all sorts of other potential agencies/affordances. I'm currently unsure that I want to go down that path just because I feel like it might detract a bit from the pure contraint of a demake into Bitsy. Will think about it though, pending how frustrating the project is.

Along with that I suspect I'll need/want to implement each level as a separate Bitsy game because I think the limitations especially around the avatar and just management might get out of hand. Actually the avatar's probably not such a big deal, but I guess I just worry that if there's a shit-ton of rooms it might get ugly? Maybe we'll just see on that front.

## Aesthetics

How is this going to look aesthetically? I need a consistent style. Minimalist I assume? I don't think I have it in my to try to make something as beautiful as several Bitsy games I've seen have been? We'll see.

## Specific levels

So with that in mind, what are my initial thoughts for how to represent this stuff?

### Sisyphus

- One thought is a bit of a joke-oriented idea, which is that you see the boulder in the first screen and "pick it up", then carry it up a representation of a hill, and then on reach the top trigger a message telling you it has rolled back down to the bottom, and then you have to go back down? There might be some weird state stuff involved even in something as simple as this, but it could be a funny representation (I'll need to use the idea of a pickup at least once, I think, anyway - though there are other opportunities).
- The other idea is more of a recreation of a frame-by-frame animation where each step up the hill triggers a new room with the boulder element moved further up. Sounds like hell in terms of just redrawing the same thing over and over, to which end it may be better to go with the other option?

### Tantalus

- Here the 'obvious' approach is to have an apple and water element(s) and then to make them inaccessible specifically by placing a wall, or alternatively to trigger a second room when you "reach" for them that places them beyond your ability to grasp. Will just have to play around a bit with representations and see what sticks? I think this basic idea is pretty obvious though?
- A related idea might be that you do pick them up but then trigger back to a level where they're back where they were? This doesn't make as much sense though given that we don't want even an underlying representation of a successful pickup?

### Prometheus

- Should make sense in terms of representing a little dude trapped on a rock who can't move, but then how does the eagle come into play? If you allow movement with triggers (each of which always leads to a room where you haven't moved?) then each subsequent room could animate the eagle closer, and you just end up using a sequence of rooms as a form of animation?
- Alternately the eagle could be an NPC (it's the only real opportunity for this in the game so maybe that's a good option?). The problem with that being there's no way to track state at all, so I would have to be okay with the idea that I don't represent the cyclical nature of the punishment. It would be more satisfying to literally "chain down" the player and have no movement, but because Bitsy is movement-oriented, that then ties my hands (ha ha) in terms of actually making anything happen. So.

### Danaids

- As always, the most complex one in terms of step, but should make sense to be able to pick up the water and carry it to the bath? The bath as an NPC that registers your contribution? Then use a trigger (or two?) that resets back to the initial screen?

### Zeno

- This one scares me a little bit because I doubt I'm going to be able to come up with any particularly deep representation of the idea of a kind of fractal space? As simple as just allowing them to get half-way there? Or a sequence of rooms in which the flag gets "further away"? I'm really unsure of how to make this one work actually, maybe it's the hardest one?

---

# Avatar, sprites, items, oh my :(, Be the boulder you want to see in the world?, Separate Bitsies?, Menu?, Extensions? (Thursday, 4 July 2019, 14:36PM)

Hi there, just reporting in after my initial foray into actually doing stuff in Bitsy. Specifically I wanted to clarify that I understood the relationships between items, sprites, the avatar, and exits.

## Avatar, sprites, items, oh my :(

It turns out to be more frustrating than expected, especially for someone who wants to make something cyclical. As per my previous commit message

- Items can be placed multiply but if you pick one up it's gone for good (across rooms during a playing), so to they're very temporary but have an irritating permanence when they're gone, which makes them a bit useless for anything you want to happen more than once
- Sprites can only exist in one place (across rooms during a playing), so they're "stable" and can be returned to, but can't move or vanish. And as far as I can tell no trickery available in terms of triggering a room shift on examining/interacting with a sprite.

The result of this is that I suspect I'm going to have to think significantly differently about how to represent these myths. In particular, my cute idea of "picking up" the boulder as an item and transporting it to the top isn't going to work, because when you return to the bottom the item will be gone.

In fact at least initially I have felt quite defeated by it. If the boulder is an item, it vanishes and can't be repeated. If the boulder is a sprite it can't move and I'd be limited to something horrendous like an "animation" room by room of "pushing" it up the hill. Which I guess is fine, but just feels like you're fighting what Bitsy is good at. There's something to that (consider that moth game I played that did some cool animation effects), but I'm wanting to mostly explore how to "express" the myths through this engine, not bend it totally to my will.

## Be the boulder you want to see in the world?

In my despair I did think that perhaps an approach to Sisyphus is to make the avatar the boulder. This is quite satisfying in that it collapses the myth in a way that seems videogame-y. An alternative would be to try to represent Sisyphus+Boulder in a single tile, but that would be impossible given the tile size. And one reading of boulder-as-avatar is that then the player is very specifically Sisyphus, "pushing" the boulder uphill with the keys?

This would allow the requisite looping behaviour because you'd go to the top and hit a hidden exit that drops you back at the bottom. This would require you to know that it had happened though, so there would need to be some distinguishing features that differentiate it all. Probably want to just represent the whole myth on a single screen?

## Separate Bitsies?

This would also more or less require each myth to be a separate Bitsy rather than doing them all within a single one which is a shame. Interesting that the nature of the engine, though, perhaps kills the idea of an avatar performing multiple different kinds of tasks? I guess the set of verbs and objects just can't encompass it. And perhaps most of all because movement is the most powerful (almost only) agency in a Bitsy game and so you do need to hold it back for specific actions and not just wandering around.

One nice thing about needing to use separate Bitsies per myth is that it would allow a return to the colour schemes of the different levels, so that would be something.

## Menu?

How would I link them all together I wonder? Just an HTML page with links? Seems a shame, but if that's how it is then that's how it is.

## Extensions?

Looking through the list of bitsy extensions at https://github.com/seleb/bitsy-hacks I see some possibilities to overcome certain issues. Notably

- I see you can execute JS from a dialog which could suffice for creating a Bitsy that is the menu linking to the other Bitsies
- More extreme I see you can do things like have a sprite follow an avatar which could provide a model for Sisyphus pushing a boulder
- I also see you can change the avatar in different rooms, which would potentially put me back in position to be able to produce all the game in one Bitsy (because you can have distinct palettes per room, so I wouldn't be losing out on that).

The question with all of these is whether using hacks like this devalues the overall experiment in using what Bitsy provides in order to be able to produce the myths. The menu I think I can get away with, but is having a singular avatar just a part of Bitsy I shouldn't be opting out of?

This is a bit of a challenging question, pretty philosophical.

I _think_ my feelings here are that I should be using default Bitsy as much as possible in order to treat it as the language/material of expression for the project. The more I tweak it the less case I suppose I have for talking about (and experiencing) the limitations set by such an approach.

As such I imagine allowing myself to use JS dialog in order to create a menu more in-keeping with the experience (and not intruding into the myths themselves) but otherwise staying vanilla.
