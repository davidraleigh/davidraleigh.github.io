---
layout: post
title: Results from Pairing Screen-Casts With Static Content in Technical Blog Post
---

You can see the motivation for the experiment referred to in this blog post [here](http://nsipplswezey.github.io/2016/04/07/Pairing-Screencasts-and-Text.html)

I believe in eating my own dogfood. If you build something, and you're a potential user of whatever you built, you should consume it, and reflect on the experience. If you do this enough, you end up making things that taste good.

The goal of this experiment was to see how useful it is to pair static text and images, with a screen-cast representation of the same material. The screen-cast exists as a toggleable overlay that occupies about 50% of the vertical screen space of the static content. It's styled similar to a code snippet, and drop-shadowed to give it a visual sense of overlay. It toggles quickly and gracefully with a fade. The toggle control is a check box.

First impressions:
* It's very difficult to read and listen at the same time!
* Even though I have the choice to leave the video un-toggled that little blue check box is distracting from my reading!
* The video content and the written content fall out of sync as a navigate. I can't tell if I'm ahead or behind! And it takes time to figure that out.
* I'm not sure when I'm supposed to read, and when I'm supposed to listen. It's exhausting to try and figure that out.
* Interrupting the visual path between me and my text content feels A LOT like a popup ad.
* Generally, to solve all these problems, I'd probably just play the video if it were embedded at the top of the page, and then read the text content as necessary, while following the video. I can reference video to text, because the text is searchable. I can't reference text the video.



### I love screen-casts as a medium for communicating technical information. Why?
So let's see what we gain and lose relative to either screen-cast or technical blog post. Some of these benefits were listed in part 1:

* [LOST] They often reveal the character of the software engineer
There are two characters in this medium; the blogger and the screen-caster. It's very difficult to keep track of both of them. And by trying, I don't really get a clear sense of either of them.

* [LOST] They tend to move briskly, we often get to see real engineers thinking through real problems, elucidating their reasoning, making mistakes and fixing them.
When you introduce text, it feels like I need to understand every step before I move on. A screen-cast relieves the user of this need, while text reimposes it.

* [LOST] They typically offer a shorter attention interval than text posts covering the same material.
The perception of the duration of the screen cast gets LONGER based on my assessment of the duration of the whole piece based on the length of the text. The blog post looks like 60 minutes of reading. The screen cast is only 14 minutes of content. This dissonance sucks.

* [NEUTRAL] They engage both visual and audio learning channels.
This is where there is some enhancement, and some potential loss. The text content adds to the visual channel. A code snippet that exists for 4 seconds of screen time, exists permanently in the text. I can re-read it at my leuisure.

### Non-video content built around screencasts offers some enhancement to the screen-cast:

* [POSITIVE] A set of static instructions can be searched for keywords, copied, and pasted
This text based reference is really useful, and searching, copying and pasting is really useful.

* [NEUTRAL] The organizational structure of static text and images is transparent and can be interpreted quickly by skimming titles and headings.
If the static image and text content doesn't stick closely to the screen-cast, then there isn't value added. But if the static text and iamge content adheres closely –or strictly– to the screen-cast content, then this is useful. In this blog post, there were moments of alignment –specifically around code snippets– that were useful. But parts where the text and image departed from the screen-cast were detrimental.

* [NO TEST]Static text and image is persistent, and can be easily updated over time, where video content might need to be re-recorded.
No updating was necessary at this point.

* [POSITIVE]Code snippets in a blog post can reduce complexity, by focusing only on specific files, classes, methods or lines of code, and ignoring the rest of the text on screen.
Managing complexity is one of the hardest part of programming, and even more difficult when looking over someones shoulder at a screen-cast. The moments where code snippets focused attention added value.

* [NEUTRAL]Static text and images can be reviewed at the pace of the reader, where as screen-casts move at the pace of the producer.
It's unclear which piece of media is leading: is the the video or the text? Which pace should I go at? The pace of the screen-caster, or the pace of my reading of the blog post.

* [NEUTRAL]Text content can go into more depth on subjects of interest that may be passed over in screen-cast.
Going into depth using text commentary and links can interrupt the screen-cast flow. Typically that's not a big problem, and the value add of some background content can make a big positive difference.


### Some interesting surprises

* The interface for toggling the video allows the use of the space bar to toggle the video, as long as the video is in focus.
This is actually super useful. If the video play state can be toggled at the click of a common keyboard button, then the transition between text and video content –along with searching the text of the document– doesn't require a mouse. The downside is that if the video iframe is in focus, then the space bar pauses the video, which is useful and engrained gesture.

* The perception of a tasteful overlay is important.
Having the check box floating out in space on the margin, not positioned in any way relative to the video content it toggles... is shockingly irritating. On the flip side, hitting space bar and having your reference video appear and dissapear, is really pleasant!

* Code snippets are really where the value is.
A code snippet is actually the best landmark/milage sign for syncing between the technical blog content and the screen-cast content. Excess narrative in text is actually distracting. A link is the correct way to enable narrative content for anyone who wants to go deeper.

* The overlay metaphor is powerful
A little drop shadow on the video gives the page dimension. This is not a flat design. It is a design of thin layers: video control, video content, structured code snippets, links for depth. Too much content in the middle layer becomes a serious issue. Additionally there exists problematic initial confusion around the focus-dependent space-bar gesture that pauses the video control layer in one instance, then toggles the video content layer in the next.

### Conclusion

In the motivation of this experiment, I wrote:

> I think the real magic of any future medium for improving thinking, is finding the best combinations of our current tools; combinations that leverage the strengths of different current mediums, and enhance the communication of information without any penalty.

I haven't been able to falsify that statement, so I'll stick with it as a heuristic. A new medium can't be a compromise of two existing mediums. It has to be the best of both mediums, without noticeable loss to either of them. This first experiment lost the flow of the screen-cast with excess text and interrupted the clean flat focus of static scrollable text content with a video.

The exciting part is that there's a clear way forward for the next iteration:

* Design for thin layers.
Relocate text narrative, where possible, to links. Maybe try anchor tagging content to a foot-notes section. I've always liked this style, and this is a great rationale.
Emphasize 1-to-1 relationships between text code snippets and heading structure and video flow.

* Address each layer with as little user effort as possible.
Leave the video control layer pause for space bar. Try another key for video content layer toggle. Get rid of the check box, as it interrupts the flow of the text layer.
