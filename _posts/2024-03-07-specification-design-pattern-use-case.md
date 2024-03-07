---
title: Specification Design Pattern – Use Case
description: Using the Specification Design Pattern for Smart Playlists
unlisted: true
---

<img src="https://core0.staticworld.net/images/article/2013/05/smart-playlist3-new-100036189-orig.png" alt="Smart Playlist" title="Image Source: https://core0.staticworld.net/images/article/2013/05/smart-playlist3-new-100036189-orig.png" width = "70%" align="center" style="padding-right: 35px;">
 
# Introduction
[Specification Design Pattern](https://jhumelsine.github.io/2024/03/06/specification-design-pattern.html) introduced the Specification Design Pattern, but it was getting a bit too long. This blog continues the story with a Use Case.

# Use Case
Smart Playlists are probably one of the best real world programming examples I can think of for Specification.

## Smart Playlist Description
iTunes allows users to create playlists of their favorite music. I think most people curate their playlists manually, which means that if you load a new CD into iTunes and you wanted some of the tracks in your playlist, then you’d need to manually place them there.

There is another type of iTunes playlist, which I prefer even more. It is the Smart Playlist. See [How to Create Smart Playlists in Apple Music & iTunes](https://www.lifewire.com/creating-smart-playlists-itunes-1999279) for an article describing Smart Playlists with a few screenshots.

Users can create a named smart playlist and define a set of criteria for the tracks that they want to appear in the playlist. Criteria is based upon the following:
* A track attribute, for which there are several dozen including: Beats Per Minute, Genre, Artist, Album, Rating, Last Played, etc.
* __All__, __Any__, and __Not__ groupings
* A named playlist

Unlike manual playlists, which are static, smart playlists are dynamic. Anytime a new track is added to iTunes it will automatically be part of a smart playlist for which its attributes matches the smart playlist criteria. Likewise, if the user modifies the smart playlist definition, then all tracks in iTunes will automatically adjust to be added, removed, or remain based upon the updated smart playlist criteria.

A Beats Per Minute workout smart playlists could be a common smart playlist. If the user uploads a new pop song with a fast beat, then it will automatically be added to the workout playlist.

I created several smart playlists for my own use in these three basic categories:
* Organizing my music such that my higher rated tracks would play more frequently via shuffle than my lower rated tracks.
* Organizing my podcasts.
* Organizing my audio books with a bookmark to remember where I had left off.

The __All__ grouping is a logical __AND__. The __Any__ grouping is a logical __OR__.

I don’t know if the smart playlist developers at Apple used Specification for their design, but if I had been assigned the smart playlist design and implementation, I definitely would have considered Specification.

## A Smart Playlist Design
The Smart Playlist Design is going to mirror Specification Design so closely, that for the most part, I’m going to focus upon diagrams more than text.

### Smart Playlist Design via Strategy
Other than name changes, this is identical to the [Specification Strategy Design](https://jhumelsine.github.io/2024/03/06/specification-design-pattern.html#strategy):
* The `Specification`’s Context is a `Track`.
* The `Track`s are managed by the `TrackManager`.
* `SmartPlaylistBuilder` creates the `Specification` instances.

<img src="/assets/SpecificationPlaylistStrategy.png" alt="Leaf Playlist Classes" width = "90%" align="center" style="padding-right: 35px;">

### Smart Playlist Design via Composites
Other than name changes, this is identical to the [Specification Composite Design](https://jhumelsine.github.io/2024/03/06/specification-design-pattern.html#composite), even the implementation details; however the `final` details are not shown due to space constraints. See [Specificaiton Use Case - My Final Comment Or My Comment About Final](https://jhumelsine.github.io/2024/03/06/specification-design-pattern.html#my-final-comment-or-my-comment-about-final) for those details.

<img src="/assets/SpecificationPlaylistComposite.png" alt="Leaf Playlist Classes" width = "90%" align="center" style="padding-right: 35px;">

## Smart Playlist Specification
Let's say you wanted the following Smart Playlist Specification:
> Alternative, Rock, or New Wave 5-star tracks as long as they are not The Rolling Stones or U2.

You’d be able to design this Smart Playlist with this GUI specification:
<img src="https://core0.staticworld.net/images/article/2013/05/smart-playlist3-new-100036189-orig.png" alt="Smart Playlist" title="Image Source: https://core0.staticworld.net/images/article/2013/05/smart-playlist3-new-100036189-orig.png" width = "80%" align="center" style="padding-right: 35px;">

The code that might construct this Specification could look something like:
```java
Specification genres = new AnySpecification();
genres.add(new GenreSpecification(Alternative));
genres.add(new GenreSpecification(Rock));
genres.add(new GenreSpecification(NewWave));

Specification acceptedArtists = new AllSpecification();
acceptedArtists.add(new NotSpecification(new ArtistSpecification(“The Rolling Stones”)));
acceptedArtists.add(new NotSpecification(new ArtistSpecification(“U2”)));

Specification constraints = new AllSpecification();
constraints.add(new RatingSpecification(5));
constraints.add(acceptedArtists);

Specification fiveStarAltRock = new AllSpecification();
fiveStarAltRock.add(genres);
fiveStarAltRock.add(constraints);
```

The above is a sample of how to build the Specification/Composite tree statically. However, this would not support the flexible GUI. The software behind the GUI would still call the same methods shown above, but it would do so in a more piecemeal approach were each element would be considered individually, created and assembled into the whole structure.

The Specification/Composite tree would be:
<img src="/assets/SpecificationPlaylistObjects.png" alt="Leaf Playlist Classes" width = "90%" align="center" style="padding-right: 35px;">

### A Redundant All Grouping?
I found the above Smart Playlist example on the internet. I think there's a redundant `All` grouping in it. While the redundant `All` isn't incorrect, here's a static implementation that I think satisfies the same tracks:
```java
Specification genres = new AnySpecification();
genres.add(new GenreSpecification(Alternative));
genres.add(new GenreSpecification(Rock));
genres.add(new GenreSpecification(NewWave));

Specification acceptedArtists = new AllSpecification();
acceptedArtists.add(new NotSpecification(new ArtistSpecification(“The Rolling Stones”)));
acceptedArtists.add(new NotSpecification(new ArtistSpecification(“U2”)));

Specification fiveStarAltRock = new AllSpecification();
fiveStarAltRock.add(genres);
fiveStarAltRock.add(new RatingSpecification(5));
fiveStarAltRock.add(acceptedArtists);
```

It's Specification/Composite tree would be:
<img src="/assets/SpecificationPlaylistObjects2.png" alt="Leaf Playlist Classes" width = "90%" align="center" style="padding-right: 35px;">

# Summary
This Use Case shows how Specification could be used as the design/implementation mechanism for Smart Playlists.
Specification allows a user to create any number of Smart Playlists customized to each user’s specifications.

# References
See: [Specification Design Pattern/References](https://jhumelsine.github.io/2024/03/06/specification-design-pattern.html#references).
