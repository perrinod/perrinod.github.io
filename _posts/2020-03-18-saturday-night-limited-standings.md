---
layout: default
title: "Saturday Night Limited Standings Version 0.0.2 (03/18/2020)"
description: "03/18/2020"
tags: saturday night limited, saturday night limited standings, twitch extension, twitch api, smashgg, smash gg api
---

<center><h1>Saturday Night Limited Standings Version 0.0.3 (03/18/2020)</h1></center>

***

<center><h2>Previous Blog Post</h2></center>

***

<a href="https://github.com/perrinod/saturday-night-limited" target="_blank">GitHub Repository</a>
<a href="https://dashboard.twitch.tv/extensions/8rpnlmi2qua45dyuhmwvt40qogtben-0.0.2" target="_blank">Twitch Extension Page</a>

In my <a href="https://perrinod.github.io/2020/03/11/saturday-night-limited-standings.html" target="_blank">previous blog post</a> I wrote about my attempt at creating a twitch extension that displays live tournament standings.  The extension went live and was utilized for a tournament.  After watching the behaviour of the extension I learned that it was displaying incorrect information.  I will explain what went wrong and how I plan to fix these problems for version 0.0.3, along with new features that version 0.0.3 will bring.

***

<center><h2>Updates For Version 0.0.3</h2></center>

***

Version 0.0.3 will have new features that include:
* Extension viewable on Android devices
* Displaying what matches are currently on-going or over in the round
* Allowing the broadcaster to have a viewable image of the standings for obs

<center><h2>Development for Android</h2></center>

***

According to twitch mobile guidelines:

```
If your extension is intended for mobile devices, you can provide a second front end for those devices. You can use an identical front end for mobile and web, if it is responsive enough to render and perform well on both platforms; otherwise, submit different front ends.
```

Since our code for the front end is quite simple we can simply just copy paste the code we have for video_component.html into mobile.html and add the line:


```javascript
<meta name="viewport" content=" width=device-width; initial-scale=0.5, maximum-scale=0.5, minimum-scale=0.5; " />
```

And then it's done.

<center><h4>Saturday Night Limited Mobile Images</h4></center>

<img src="https://drive.google.com/uc?export=view&id=1364sWC56hHBl4EE9EuzDs7046nmqhvTa" alt="saturday night limited mobile image" title="saturday night limited extension">

<img src="https://drive.google.com/uc?export=view&id=133_BFW51paXb91nDBnhzNDs4jErEO4NL" alt="saturday night limited mobile image" title="saturday night limited extension">

<center><h2>Displaying Active And Inactive Matches</h2></center>

***

Currently we have the server operating on a loop of multiple queries that looks like this:

```
Server queries for standings and if the tournament is complete
Server queries for round x matches that have ended
Server queries for round x + 1 matches that have ended
Server combines the standings array with matches array
Server sends that information to twitch only if the information is different than the last time it sent information
```

We need to change and add queries so the loop would look like this:

```
Server queries for round x matches that have ended
Server queries for round x + 1 matches that have ended and if the tournament has ended
Server queries for round x matches that haven't ended
Server queries for round x + 1 matches that haven't ended
Server sends that information to twitch only if the information is different than the last time it sent information
```

We no longer care for the standings due to formating reasons but, also is explained later about issues involving v0.0.2.

<center><h3>Query Round X For Matches That Haven't Ended</h3></center>

We start by changing the time in which the server queries smash.gg from 30 to 15 seconds.  This is still within the accepted rate limits of smash.gg.

* You may not average more than 80 requests per 60 seconds.
* You are limited to a maximum of 1000 objects per request (this includes nested objects).

We need to query the current round for any matches that are currently active so we can display that information on the extension.  We can do this by simply modifying the match score query to:

```javascript
query EventStandings($slug: String, $page: Int!, $perPage: Int!, $roundNumber: Int!) {
    event(slug: $slug) {
        sets(page: $page,
            perPage: $perPage,
            sortType: STANDARD,
        filters:{
            roundNumber: $roundNumber
            state: 2,
        }){
            pageInfo{
                total
            }
            nodes{
                slots{
                    entrant{
                        name
                    }
                }
            }
        }
    }
}
```

This will tell us the active matches for the round.  When we send the information to twitch the front end will be able to highlight that match.

<center><h3>Query Round X + 1 For Matches That Haven't Ended</h3></center>

This may look confusing as to why the server is doing this at first.  If we go through each query:

1. We query to see if a game has ended so we can display the participants new score.
```
Server queries for round x matches that have ended
```

2. We query to see if matches have ended in the next round.  If it has then we need to increment x.  If the tournament has ended and there is no matches that have ended in round x + 1.  We can stop the queries.
```
Server queries for round x + 1 matches that have ended and if the tournament has ended
```

3. We query to see if there are any active matches so we can hightlight these in the extension.
```
Server queries for round x matches that haven't ended
```

4. We query to see if the next round has started.  If it has then we need to increment x.
```
Server queries for round x + 1 matches that haven't ended
```
2 and 4 look to be doing the exact same thing but, they serve a different purpose.  2 is used to handle a ended round while 4 is used to handle a non ended round. For example, lets says a tournament is on round 3 and then they use the extension.

For both round one and two, 2 would apply but, 4 would not.  Once it gets to round three then 4 would apply but, 2 would not.

<center><h2>Allow The Broadcaster To Have Standings For OBS</h2></center>

***

All we need to do is modify live_config.html and have exactly what is being displayed to the viewer be placed here.  The only problem is we need to make sure that it will not take up too much space for the broadcaster when they display it in OBS.

We can simply just use setInterval to make it so it auto scrolls the tournament standings.

```javascript
setInterval(function () {
    var pos = $('#standing').scrollTop();
    $('#standing').scrollTop(pos + 2);

    if ($('#standing').scrollTop() + $('#standing').height() >= $('#standing').prop('scrollHeight')) {
        setInterval(function () {
            $('#standing').scrollTop(0);
        }, 2600);
    }
}, 200);
```

<center><h2>What Went Wrong</h2></center>

***

The extension was designed to:
* Display the title of the event and players
* Display what round it is
* Display the standings of everyone
* Display the scores of everyone

The extension did everything correct except determine the standings and scores of everyone correctly.

<center><h2>Incorrect Standings</h2></center>

***

In the previous blog post I made mention that we could determine the standings of a participant by making a query as followed:

```javascript
query EventStandings($slug: String, $page: Int!, $perPage: Int!) {
        event(slug: $slug) {
            standings(query: {
            perPage: $perPage,
            page: $page
        }){
            pageInfo{
                total
            }
            nodes {
                placement
                entrant {
                    name
                }
            }
        }
    }
}
```

There is nothing wrong with this query, it does indeed return the standings of each participant.  Once the extension went live I found out that the standings of an event do not update untill the tournament is over.

So throughout the entire tournament the extension will just display the same information:

```
(x) participants name
x = total participants
```

<center><h4>Correcting Tournament Standings</h4></center>

This is very trivial to fix since we are already getting the score of each participant with this query: 

```javascript
query EventStandings($slug: String, $page: Int!, $perPage: Int!, $roundNumber: Int!) {
    event(slug: $slug) {
        sets(page: $page,
            perPage: $perPage,
            sortType: STANDARD,
            filters:{
                state: 3,
                roundNumber: $roundNumber
            }){
            pageInfo{
                total
            }
            nodes{
                slots{
                    entrant{
                        name
                    }
                    standing{
                        stats{
                            score{
                                displayValue
                            }
                        }
                    }
                }
            }
        }
    }
}
```

The server could calculate their win percentage as it records scores.  It could use that to determine the actual standings of each participant.  Since this is backend code and not front end code, this can be corrected at any time.

<center><h2>Incorrect Scores</h2></center>

***

In the previous blog post I made mention that we could determine the round scores of a participant by using the following query:

```javascript
query EventStandings($slug: String, $page: Int!, $perPage: Int!, $roundNumber: Int!) {
    event(slug: $slug) {
        sets(page: $page,
            perPage: $perPage,
            sortType: STANDARD,
            filters:{
                state: 3,
                roundNumber: $roundNumber
            }){
            pageInfo{
                total
            }
            nodes{
                slots{
                    entrant{
                        name
                    }
                    standing{
                        stats{
                            score{
                                displayValue
                            }
                        }
                    }
                }
            }
        }
    }
}
```

What this query returns is the score for the match.  The server is currently designed to handle one vs one in a best of three.

Thus, the displayValue for the two participants in the match would be:

```javascript
standing{
    stats{
        score{
            displayValue = 0 || 1 || 2
        }
    }
}
```
The server was manually calculating who won the match by saying if the participant obtain 2 wins in a best of three they must be the winner while the other participant must be the loser.  I didn't believe there was anything wrong with this way of thinking untill you learn it can actually return:

```javascript
standing{
    stats{
        score{
            displayValue = -1
        }
    }
}
//return -1 if the participant was DQ (disqualified)
```
This would make it so you would end up with a game where the score was:

```javascript
slots{
    entrant{
        Player
    }
    standing{
        stats{
            score{
                0
            }
        }
    }
    entrant{
        Opponent
    }
    standing{
        stats{
            score{
                -1
            }
        }
    }
}
```
The winning players against an opponent who was disqualified were getting scores of 0 instead of 2.  Making the calculations wrong and thus, the extension was displaying incorrect information.  

<center><h4>Correcting Scores</h4></center>

Instead of calculating the scores manually we request who won and loss by getting the placement instead.

```javascript
query EventStandings($slug: String, $page: Int!, $perPage: Int!, $roundNumber: Int!) {
    event(slug: $slug) {
        sets(page: $page,
            perPage: $perPage,
            sortType: STANDARD,
        filters:{
            state: 3,
            roundNumber: $roundNumber
        }){
            pageInfo{
                total
            }
            nodes{
                slots{
                    entrant{
                        name
                    }
                    standing{
                        placement
                    }
                }
            }
        }
    }
}
```

The placement will return:
```javascript
standing{
    placement = 1 || 2
}
```
If the player has a 1 it means they beat the other opponent thus winning the match.

This looked incredibly easy to fix.  Except this doesn't fix everything wrong about the scores.  It is possible that a match ends prematurely and the score is recorded.  Also it is normally possible to query for bye matches with smash.gg API but, is currently not working since January 2020.

<center><h3>Correcting Match Scores</h3></center>

It's possible to edit a match score from a previous round and possible for a match to end prematurely.  These create a problem since the server operates one round at a time and only records the match score once.

This means we need to take our query that obtains the score for the matches in the round and loop through to the current round.

```javascript
for (var i = 1; i <= roundNumber; i++) {
    await roundScores(i, slug);
}
```

The only problem with doing this is we do not want this to occur every 15 seconds because we may go over the rate limit.  Therefore we make sure to only do the loop when the round number has changed.

By doing this the other queries will be used to display the tournament standings in real time. Then when we progress to a new round we get all the round scores from scratch just to ensure we get any changes that may have happened.  All while staying within the rate limits.

We can also just add another piece of code where we check to see if any participant didn't play during that round. If they didn't we know they were on a bye and thus we can manually modify their total score.