---
layout: post
title:  "Predicting at the command line @ PAPIs.io '14 [video]"
date:   2015-02-16 12:00:00
description: ""
image: /img/predicting-at-the-command-line.png
---
<script src="http://www.youtube.com/player_api"></script>
<script type="text/javascript">
var player = 0;
function onYouTubeIframeAPIReady() {
    player = new YT.Player('player');
}

function skipTo(time) {
	timeParts = time.innerHTML.split(":");
	minutes = parseInt(timeParts[0]);
	seconds = parseInt(timeParts[1]);
	player.seekTo((minutes * 60) + seconds);
}	
</script>

On November 17 of last year, I gave a tutorial at the first International Conference on Predictive APIs and Apps, also known as [PAPi's.io '14](http://www.papis.io/2014/). The recording is now available for your viewing pleasure:

<a name="video"></a>
<iframe id="player" type="text/html" width="640" height="360" src="http://www.youtube.com/embed/3TieT2eOM1Y?enablejsapi=1&autoplay=0" frameborder="0"></iframe>

After a [myriad of API presentations](http://lanyrd.com/2014/papis2014/schedule/), including those by [Yhat](https://yhathq.com/), [BigML](https://bigml.com/), and [GraphLab](https://dato.com/index.html), 
I thought it would be nice to switch gears by demonstrating how such APIs can be employed from the command line (hence the title "Predicting at the Command Line"). 

Don't worry about [the slides](http://lanyrd.com/2014/papis2014/sdfywx/), they're quite boring. More interesting is the live demo, in which I demonstrate how to use the command line to:

- connect to the [Barcelona bicing](https://www.bicing.cat/) API using `curl` (<a href="#video" onclick="skipTo(this); return false;">12:30</a>),
- convert the returned XML data to CSV using `xml2json`, `jq`, and `json2csv` (<a href="#video" onclick="skipTo(this); return false;">17:59</a>),
- plot the locations of bike stations using `Rio` and the [ggmap](http://cran.r-project.org/web/packages/ggmap/index.html) package (<a href="#video" onclick="skipTo(this); return false;">27:18</a>),
- assign anomaly scores using [BigML]()'s predictive API (<a href="#video" onclick="skipTo(this); return false;">30:59</a>), and as an additional impromptu demonstration, 
- apply [indico](http://www.indico.io)'s language detection API to tweets using `t` and `parallel` (<a href="#video" onclick="skipTo(this); return false;">38:21</a>).

In case you'd like to try this out for yourself, you should check out my book [Data Science at the Command Line](http://datascienceatthecommandline.com/) and the associated [Data Science Toolbox](http://datascienceatthecommandline.com/#dst), which has all these command-line tools pre-installed.

Cheers,

Jeroen


