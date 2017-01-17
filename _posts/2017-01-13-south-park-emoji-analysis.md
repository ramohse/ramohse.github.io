---
layout: post
title:  "Emoji Analysis - Can You Replicate Data Science from South Park?"
date:   2017-01-13 16:52:07
categories: emoji word2vec twitter
tags: emoji word2vec twitter
image: /images/2016-01-13-Emoji/emoji_analysis_icon.jpg
---

One of the most exciting things about learning data science is having a toolkit and framework to answer questions, no matter how practical or absurd. For instance, the week before we started our third project at Metis, an episode of South Park aired that contained this clip:

[![Emoji Analysis](https://github.com/ramohse/ramohse.github.io/blob/master/images/2016-01-13-Emoji/emoji_analysis_screencap.png?raw=true)](http://southpark.cc.com/clips/vjp3m9/its-called-emoji-analysis "Emoji Analysis")

So how would one... do that? How would one measure emoji sophistication? In my view, there are three general classes of emoji usage:

- **Simple usage** - The emoji and the text convey the same idea, i.e. "LMAO 😂". Here the emoji add little, serving instead to reinforce the meaning of the text.

- **Intermediate usage** - The emoji qualify or provide subtext for the text. For instance, the phrase "My dad is visiting" in isolation could be positive, negative, or neutral, but with "My dad is visiting 💀🔪", the meaning is more clear.

- **Advanced usage** - Several emoji are used to convey a complex idea completely different than the text or with no text at all. For instance, "🏃🏻✈️🇨🇦" might reflect how some people feel after the 2016 presidential election. [Here](https://www.sporcle.com/games/felix/movies-by-emoji) [are](https://www.sporcle.com/games/emilymarie07/disney-songs-in-emojis) [some](https://www.sporcle.com/games/felix/movies-by-emoji-2) fun quizzes that further illustrate this idea.


Thus, to be able to measure emoji sophistication, one would have to not only count the emoji used in a given message, but also measure their innate "meanings" relative to the text. As it happens, there is a well-known neural network for deriving word meanings in text--Google's [Word2Vec](https://en.wikipedia.org/wiki/Word2vec) model. Without getting too deep in the weeds, what the model does is scan a body of text, generating a vector representation of each word based on the words that surround it. What you get is a numerical representation of the meaning(s) of each word, which can lend itself to some cool semantic analysis--for instance, using the official Word2Vec model you could calculate the equation ["king - man + woman = queen" or "Paris - France + England = London"](https://blog.acolyer.org/2016/04/21/the-amazing-power-of-word-vectors/) 

As it happens, someone beat me to a pure emoji word vector model by [only a few weeks](https://arxiv.org/pdf/1609.08359.pdf), but for the purposes of my analysis I was less interested in the emoji and their vector representations by themselves, and more interested in the hybrid usage of emoji and text to convey ideas. As such, I decided to train my own model. To do so would require a massive amount of data, and there are very few readily-available resources for a ton of text with emoji. The best option at present is Twitter, so I set up an API script to track live tweets and save any tweets with emoji in them. In the end I obtained over 1 million tweets to train my word vector model. Initial results were... pretty good! Though more data would always help, and there were some weird missteps given the sloppiness of the data, using a measure of "similarity" on popular emoji indicates the model to some degree understands the nuanced meanings of certain emoji:

### Words Most Similar to Popular Emoji as Measured by the Model
---
![Tears of Joy](https://github.com/ramohse/ramohse.github.io/blob/master/images/2016-01-13-Emoji/emoji_similarity_tears_of_joy.png?raw=true)
![Heart Eyes](https://github.com/ramohse/ramohse.github.io/blob/master/images/2016-01-13-Emoji/emoji_similarity_heart_eyes.png?raw=true)
![Eggplant](https://github.com/ramohse/ramohse.github.io/blob/master/images/2016-01-13-Emoji/emoji_similarity_eggplant.png?raw=true)
![Poop](https://github.com/ramohse/ramohse.github.io/blob/master/images/2016-01-13-Emoji/emoji_similarity_poo.png?raw=true)
---

Now that I had vectors of both words and emoji, I could use a measure of vector similarity known as [cosine similarity](https://en.wikipedia.org/wiki/Cosine_similarity) to gauge how different the emoji and text of a given tweet are. With this as a foundation, I created a "simple dumb model of emoji sophistication": 

### A Simple Dumb Model of Emoji Sophistication
---
![Simple Dumb Model](https://github.com/ramohse/ramohse.github.io/blob/master/images/2016-01-13-Emoji/emoji_model.png?raw=true)
---

The individual components are:

- **Unique emoji / Total emoji** - A simple ratio representing emoji "eloquence." For example, "Happy Day! 😊😊😊" would have a score of 0.33 (only one type of emoji used three times), vs. something like "🍔🙅‍🍣💁", which would have a score of 1.0. This is our initial score.

- **Unique emoji cosine similarity** - A measure of how similar the unique emoji used are on a scale from 0 to 1--more similar emoji would have a higher score, and thus a greater "penalty" to the initial score, and more diverse emoji would have a lower score/lower penalty. For example, "😊😀😃" would have a high score, as these all convey similar ideas, and "🙈⏰🏢" would have a low score.

- **Emoji and text vector cosine similarity** A measure of how similar the aggregate emoji vectors are with the text. Using my examples above, "LMAO 😂" would have a high score (similar text and emoji meanings) and "My dad is visiting 💀🔪" or "🏃🏻✈️🇨🇦" would have low scores.

Using this simple model I calculated the level of sophistication for each tweet in my corpus, and the initial results are encouraging--the most complex tweet in my corpus was:  

**Break room today = 😁 - 😣 + (😲 -🔥)× 🙌/👌+×💔\*$^📉📈**

Not bad, model! Not bad.

# So... what's next?

As a result of the initial project I am now *extremely* interested in further emoji analysis. As part of the initial survey I made a quick network chart, showing which emoji were most frequently used with other emoji. Analysis of semantic meanings of pairs or triplets of emoji is the next step in evaluating complexity, I feel, as the meanings of certain emoji change when paired with one another--think "😂💀" (I'm dying laughing) vs. my earlier "💀🔪" (I want to murder someone). 

Getting here will require a much larger, better-curated body of text than just 1 million raw tweets--an endeavor I am working on at present. Stay tuned! Eventually we might get to this:

[![Emoji Math](https://github.com/ramohse/ramohse.github.io/blob/master/images/2016-01-13-Emoji/emoji_math.png?raw=true)](http://southpark.cc.com/clips/23obdq/trying-to-see-patterns#source=08f60a6f-24a8-4d88-88a3-eb5588494cbc:879fd28e-c96b-4f9d-a437-e05c1bcf80aa&position=159&sort=views "Emoji Math")
