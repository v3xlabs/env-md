# Lighting Layers

## Use Case

Imagine your perfect home automation setup.
During the day, your living room has its default "Living" scene.
And as soon as you start to watch a movie, the room transforms to your own personal Movie Theatre.

Now what if you had a dimly lit kitchen, some motion sensors, and wanted the lights to brighten as you walk to grab a snack?
What if the lights are still purple (from the movie scene), should they be warmer? When you leave the kitchen, should the lights revert back to the default Living scene, turn off, or go back to the purple, so you can continue watching the movie?

Manually maintaining / automating these multi-layered levels of automation is very error-prone, and not a fun thing to do.
So what if we could streamline the process?

## Concept

We treat every lighting intent as a "layer" with a numeric priority and some sort of human slug (for easily disabling).
Whenever you wish to turn on a light, we push that request onto the lighting stacks of the respective lights involved.
So you can toggle "room.livingroom" or "these 5 lights", and we handle the rest.
When you are done with your request, we pop it off the stack, and re-apply the now highest-priority layer on the stack.

In addition to this, you could also periodically send updates to a specific lighting layer (or hook into the color processing pipeline).
Such that if your hallway light should have a different state depending on the time of day (see [Adaptive Lighting](https://github.com/basnijholt/adaptive-lighting) or [Flux](https://www.home-assistant.io/integrations/flux/)) you could do that.

### Use cases

- Global scenes (Living (default), Movie Mode, Flashbang, Night Time etc).
- Contextual overrides (kitchen presence → max bright, time‑of‑day adjustments).
- Automatic fall‑back: when an override expires or you leave the kitchen, lights revert to the next‑highest active scene (Movie Mode if the TV is on).

This gives you true multi‑source, priority‑based control without "fighting" automations against each other.

## HomeAssistant Extension

_TLDR; I'm in the process of building a custom Home Assistant integration_ that:

1. Exposes services to **push** and **pop** lighting layers with configurable priorities.  
2. Maintains an internal queue per light (or group).  
3. Automatically re‑evaluates and applies the top‑priority layer on any change.  

Stay tuned—I'll report back once there's a working proof of concept and some sample configs!

## HomeAssistant Integration

Ideally at some point this becomes a standardized practice within the HomeAssistant software.
How that will workout, im not sure, but im hopeful.

## Turnkey solutions

Currently looking into [ha-lighting-manager](https://github.com/zachcheatham/ha-lighting-manager).
