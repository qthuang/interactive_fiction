
HW 2 Created by Qintian Hunag, Yuan Yuan

Course Website: https://interactive-fiction-class.org/index.html
Game Adapted from Action Castle: https://memento-mori.com/pdf/parsely-preview-n-play-edition

## Part 1

The playthrough was actually smooth. There are definitely hallucinations. For example, in the system setting, we specifically stated that the parser should speak like John Wick, but not treat the player as John Wick. However, the parser was not able to remember it, and there are multiple times that it called me or the user as the John Wick (saying things like "You, John Wick"). We suspect this could be due to its data about John Wick. Probably most of the data it was trained on described John Wick as someone else, rather than speaking as John Wick. Thus, the parser was not able to understand what it means to speak like John Wick. Another guess is that the instructions to the system and the input of the user are compiled together, so the model itself cannot distinguish the person of the narration.

Other than some hallucinations, like describing non-existent objects and giving me wrong identities, the parser did a pretty good job at generating the content of the games.

## Part 2

Starting in Part 2, the number of errors increases. The performance of the generation became unpredictable. For example, we run the same code multiple times, there are a few times when the parser could not match "go fishing with the pole" with "catch fish".

We also created a few more tests to see the power of the parser. Its performance was consistent. Errors happen from time to time in different tests. We also noticed that when we increased the length of the command, the parser has a higher chance of making mistakes. It's probably becaus a long command has a lot more irrelevant information that confuses the parser. The parser also does not seem to have the ability to recognize grammatical mistakes and generate using the correct versions. For example, in the test "gu phishxing wuth the poorle": "catch fish", although the wrong words pronounce and look similar to their correct versions, the parser was not able to recognize them and return "examine."

The most tricky problem we faced was "pick rose". At first, it always matches "pick rose" with action "get", since "pick" and "get" are very similar words. This problem can be solved by modifying the instruction to "pay attention to both nouns and verbs in the command", but still fails from time to time.

## Part 3

For Part3, we used ChatGPT to match intended item, character, and direction.

ChatGPT's understanding of the directions is very robust.  
For example, it successfully recognized "opposite to west", "right", "e", "where the sun rises" as descriptions of "East".
It also understands synonyms, such as "down" and "beneath", "up" and "above", even if "go above" is not a fluent phrase.
I suppose the reason is that "direction" is very clear in its semantics, and it is relatively easy for ChatGPT to output a desired answer.

For item matching, it seems that ChatGPT could not handle all situations.
For example, it could not match names of fish, such as "tuna" and "salmon" to "fish".
Adding an adjective to the noun also fails the matching, such as "dead fish".
However, specific types of fish, such as "catfish", which has "fish" in its wording, can make a successful match.
Interestingly, "jellyfish" doesn't work. Maybe ChatGPT knows jellyfish is not a fish.

GhatGPT sometimes doesn't match a lot of synonyms for characters either.
For example, for the "troll", neither of "monster", "ogre", "dwarf" or "goblin" work.
Therefore, we recommend use the exact same name for the character for character matching.
