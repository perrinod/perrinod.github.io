---
layout: default
title: "Saturday Night Limited Standings Version 0.0.2 (03/11/2020)"
description: "03/11/2020"
tags: saturday night limited, saturday night limited standings, twitch extension, twitch api, smashgg, smash gg api
---

<center><h1>Saturday Night Limited Standings Version 0.0.2 (03/11/2020)</h1></center>

***

<center><h2>Creating an Extension That Gets Real Time Standings</h2></center>

***

<a href="https://github.com/perrinod/saturday-night-limited" target="_blank">GitHub Repository</a>
<a href="https://dashboard.twitch.tv/extensions/8rpnlmi2qua45dyuhmwvt40qogtben-0.0.2" target="_blank">Twitch Extension Page</a>

The simplest method to create an extension that obtains real time standings for a tournament would be to create your own server, database and website.  Have users sign up on your website.  Once the tournament begins you create a new section in the database that contains the tournament information.  The server puts a listener on that section of the database.  When something updates in the database the server will know and pass that information off to the extension.  

Lets say instead of using your own website, you want to use <a href="https://smash.gg/" target="_blank">smash.gg</a> website.  Now some issues begin to present themselves.  We can no longer go with placing a listener on a database because we have no control of that.  Instead you opt to use smash.gg API.  There is nothing wrong with smash.gg API but, it wasn't designed with the intention of being able to be utilized to display real time standings.  

***

<center><h2>Limitation's to Smash.gg API</h2></center>

***

As mentioned before smash.gg API wasn't designed for real time standings.  What I mean by this is when looking at their <a href="https://developer.smash.gg/reference/" target="_blank">schema</a> there is a lot of information you would like to obtain but, can't actually obtain.

If you wanted the total score that a participant has for the event, you can not request that.  You could request all rounds the participant played in and then manually calculate their total score.

If you wanted to know which round it's currently on, you would have to do it by trial and error.  Such as requesting if round 1 has ended, if it has then request round 2 and repeat this process untill you find a round that hasn't ended. 

If you wanted to know how many rounds in total the event has, you would query to see if a round exists.  If it does query one round up and repeat this process untill you find a round number that doesn't exist.

Smash.gg API was more so designed that if you know exactly what to look for you could simply query it.  But, if you have unknown information there is no nice way to determine it.  If you don't know what round number the tournament is on, you have to do some jank method to figure it out, then request the information you need.

***

<center><h2>Limitation's To The Extension</h2></center>

<a href="https://dev.twitch.tv/docs/extensions/reference#listen" target="_blank"> Twitch listen </a> is a function that binds the callback to listen to the target topic.

Here is an example:

```javascript
    // listen for incoming broadcast message from our EBS
    twitch.listen('broadcast', function (target, contentType, json) {
        twitch.rig.log('Received broadcast json');
        SomeFunction(json);
    });
```

You need to adhere to the limitations of 

* 1 message per second per channel
* 5 KB message size

The rate limits to smash.gg API is:

* You may not average more than 80 requests per 60 seconds.
* You are limited to a maximum of 1000 objects per request (this includes nested objects).

Knowing these limitations we can simply say that the extension can only show the top 40 participants of the tournament and have a maximum of 160 participants.  This way we will not go over the 1000 objects per request.  And it will be below 5 kB message size.

You could easily overcome these limitations by doing multiple queries to smash.gg and obtain all the information required.  Then send that information in chunks to twitch.  As long as each chunk has an identifier number associated to it and contains the number of how many pieces there are.  If one of them goes missing or fails to be sent we can determine what piece or pieces we need to request be sent again.

For example:
```
Do three queries.
Label them 1, 2, 3 and total 3.
Send them.
If we recieved 1 and 3 and after some time we haven't recieved 2.
Send request for 2.
Once we have all three, we know we have them all.
Combine all three chunks and display their information.
```

You could even send how many in total they should recieve before sending the chunks.  This way you don't have the total number associated with any of the chunks.

***

<center><h2>Creating The Extension</h2></center>

***
The design of the extension is to get the standings and total score of each participant from smash.gg.  Whether the tournament is live or has already ended.

Important things we need to know:
* Has the tournament ended
* What round is it on
* Has the round ended
* Has the next round begun
* What is the standings of the participant
* What is the total score of the participant

<center><h3>Has The Tournament Ended</h3></center>
We need to know if the tournament has ended because at some given point we need to tell the server to stop getting information from smash.gg and to stop broadcasting information to twitch.

This can easily be obtained by querying the event and getting the <a href="https://developer.smash.gg/reference/activitystate.doc.html" target="_blank">activity state</a>.  If the activity state is 3 we know the event has completed.

``` javascript
query EventStandings($slug: String) {
        event(slug: $slug) {
            state
        }
    }
```

<center><h3>What Round Is It On</h3></center>
We need to know what round it is on because we need to query the score for that round.  Doing a future round would be pointless as it hasn't started yet.  Doing a past round is old information that we have already obtained.  As mentioned before there is no nice way to determine what round you're on without doing some jank method of trial and error.  So instead we make an assumption.

The assumption:
```
Every tournament begins on round 1
```

Regardless if a event has completed or is still ongoing we will return the same results.

Either:
```
It returned scores
It doesn't return scores
```

<center><h4>It Returned Scores</h4></center>
If it returned scores there are two possibilities

Either:
```
We got all the round scores
We got some of the round scores
```

If we got all the round scores we can simply move onto the next round and repeat the process. 

If we got some of the round scores we can simply keep querying the round untill we get all the round scores.

<center><h4>It Doesn't Return Scores</h4></center>
If it doesn't return scores there are two possibilities

Either:
```
The tournament is over
The round hasn't started
```

If the tournament is over and we are querying a non existing round.  It means we got all scores possible in the tournament and we need to stop querying.

If the round hasn't started we can simply keep querying untill the round has scores.

<center><h3>Has The Round Ended</h3></center>
Here we begin to enter the realm of jank.  As it's quite difficult to determine if a round has ended.  As mentioned previously we can move onto the next round if we obtained all the round scores.  So we make an assumption.

The assumption:
```
Total Scores In A Round = Total Participants / 2;
```

This assumption states that every participant will play against someone else. This make sense but, is not a true statement.

Two possible things can occur that break this statement.

* A game in which there was only one participant
* An empty game during the round

<center><h4>A Game In Which There Was Only One Participant</h4></center>
This most likely happens if a participant dropped out or had a bye.  This means if two participants dropped out of the tournament the new formula would look like this.

```
Total Scores In A Round - 1 = Total Participants / 2;
```

Even if a participant dropped out of the tournament they are still registered in the tournament and thus count as a participant.

<center><h4>An Empty Game During The Round</h4></center>
I have no idea why this happens.  During testing of various tournaments occasionally you would have null rounds.  Possibly someone added a score then removed it.  Either way the new formula would look like this.

```
Total Scores In A Round + x = Total Participants / 2;
Where x is some unknown value
```

This isn't that big of an issue because we can choose to simply ignore empty rounds. 

<center><h4>The Solution</h4></center>
You could ignore all null rounds and query for all participants in the event.  Then query for active games in the round.  Compared every participant to all active games and make sure each person appears.  If they do not then we know they dropped out.  This is great but, it only works if the tournament is active.

So instead we make a new assumption:
```
If at least one game has ended in round (x + 1) then I have the score for all games in round (x)
Where x is the round number
```

This solution works for both completed and active tournaments.  The moment we get a game in round (x + 1) we know round (x) has completed.  Since it's complete we can obtain all scores for that round.  The only problem is it's not real time.  What I mean by this is when a game is completed in round (x), we don't actually display it untill round (x + 1) has started to end.  We can overcome this by querying round (x) and round (x + 1).  This way we get round(x) games the moment they are completed and we know to move onto the next round once we get something from round (x + 1).

<center><h3>Has The Round Begun</h3></center>
This issue was actually resolved the moment we created the new assumption.

```
If at least one game has ended in round (x + 1) then I have the score for all games in round(x)
Where x is the round number
```

This means we know the next round has started to end once we recieve the first score of any game in round (x + 1).  Though we are technically not determining if the round has begun, this is still fine.  Because the moment we get a score from round (x + 1) we update the round number, scores and standings.  Thus still operating in real time.

<center><h3>What is the standings of the participant</h3></center>
This is quite easy, we can simply query that.

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

<center><h3>What is the total score of the participant</h3></center>
Since each round we are getting the scores.

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

We can combine this query with the standings query.  Meaning we store the information of the participants scores in an array and store the information of the participants standings in an array.
After the two queries we can combine them only if the scores array has updated (obtained new round information).

***

<center><h2>What Are The Steps</h2></center>
The start process begins with:
```
Broadcaster sends a tournament standings request to the server.
Server queries the tournament for all participants and stores it into an array.
```

This starts a loop of queries
```
Server queries for standings and if the tournament is complete
Server queries for round x matches that have ended
Server queries for round x + 1 matches that have ended
Server combines the standings array with matches array
Server sends that information to twitch only if the information is different than the last time it sent information
```

This loop will break if round x + 1 is empty and the tournament is complete.

If we set the queries to work in 30 second intervals this is well below the 80 requests per 60 second limitation of smash.gg API.

Does there exist a better method than doing it this way?  Since this is only version 0.0.2 there is still a lot of updates and upgrades the extension can go through. Possibly a more efficient method could be determined in the next updates. 

***

