# Spark AR - Event Helpers

## Description

This is a helper package that includes a utility for scheduling repeated, randomized events, and a utility for limiting the repeated execution of a provided event function.

## Usage

```javascript
const Audio = require("Audio");
const Time = require("Time");
const TouchGestures = require("TouchGestures");

const EventHelpers = require("sparkar-event-helpers");

// Sonar sound playback controller & event function
const sonarController = Audio.getPlaybackController("sonar_controller");
function playSonarSound() {
    sonarController.reset();
    sonarController.setPlaying(true);
}

// Tap sound playback controller & event function
const tapController = Audio.getPlaybackController("tap_controller");
function playTapSound() {
    tapController.reset();
    tapController.setPlaying(true);
}

// Play the sonar sound every 2-4 seconds, and cancel after 10 seconds
const sonarEvent = EventHelpers.scheduleEvent(playSonarSound, 2000, 4000);
Time.setTimeout(() => {
    sonarEvent.cancel();
}, 10000);

// Play the tap sound when tapping the screen, at most once each second
const limitedTapEvent = EventHelpers.makeLimitedEvent(playTapSound, 1000);
TouchGestures.onTap().subscribe(limitedTapEvent);
```