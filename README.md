
By Qintian Huang
Starter Code from https://interactive-fiction-class.org/index.html

# Analysis

## Part 1 Generate Augmented Prompts and Description

The prompt for generating augmented prompt:

Imagine that this was going to form the scene in a novel. What style of novel would it be?  Would it be narrated in the 3rd person?  Past tense? If there are multiple characters, and we found them in medias res, what would the conversation be about?

Turn this paragraph into a prompt for a LLM to describe how to write the scene.
Also let it know that it shouldn't mention anything that is not in the given information.
Output only the prompt. Start with describe a scene ...

By explicitly mentioning that "shouldn't mention anything that is not in the given information" and "start with describe a scene ...", I was able to make it work as expected.

Example of an augmented prompt:

Describe a scene set in a richly detailed world of fantasy or adventure. The scene will revolve around elements of nature and spirit worship, with a mythical creature that is to be captured. Unlock your creativity and paint a vivid picture of various characters, each having their own unique goals and relationships with the environment. \n\nThe narration should be in third person perspective since it captures the inner thoughts and goals of each character, suggesting an omniscient narrator. Narrate the story in past tense as is traditional in fantasy genre novels. \n\nMake sure to touch on an engaging conversation between two characters christened as 'The Lake Dweller' and 'The Sacred Maiden'. The Lake Dweller is hoping to catch a mythical fish and The Sacred Maiden, concerned about irritating the water gods, voices her disagreement. This conversation should spawn a contemplative discussion about respect for the natural world against the pursuit of personal ambitions. \n\nRemember to stay within the parameters defined and avoid adding or suggesting any information not given in these instructions.


Example of description:

Once upon a time, on the northern shore of a shimmering lake, an ancient dispute took place over the waters that reflected the ever-changing dance of the sky. The sandy and rocky expanse, dotted with shrubs and thatched with a carpet of green grass, was a humble paradise that served both practical and mystical purposes. It was a place of everyday life for the locals, washing clothes in the soft waves. But it was also a platform for those seeking the favor of the water gods, lavishing the small stone altar with offerings and quiet prayers. \n\nOn this particular day, two characters stood toe to toe in the heart of this picturesque scenario. The Lake Dweller, an old man weathered as much by time as the driftwood scattered along the shore, was focusing keenly on the lake. With his simple yet trusty fishing rod gripped in weather-beaten hands, he was a fixture amongst the daily tableau of the shore. Hidden within his rough exterior was a childlike ambition - to capture the mythical giant fish that was rumored to dwell in the heart of the lake.\n\nOpposing him was the Sacred Maiden, the self-proclaimed custodian of the water gods. Her life was an offering to these divine entities, an act of reverence and devotion. Hoisting a sturdy ceramic jug filled with pristine water crystal clear as the day\u2019s sky, she adorned the altar with fluid grace. Yet, concern clouded her usually serene eyes, the harmony of their sacred space unsettled by the Lake Dweller's relentless pursuit.\n\nWith a wary glance at the old man, the Maiden voiced her worries. \u201cYou disrespect the peace of our waters, Lake Dweller. Your insatiable pursuit for the giant fish disturbs the sacred balance. Are you not concerned about the wrath of the water gods?\"\n\nThe Lake Dweller, a patient man accustomed to the long silences of his solitary fishing sessions, paused to consider the Maiden's words. Looking away from the placid waters, he turned towards the young woman. \u201cAh, Maiden,\" he murmured, \"In my lifelong dance with the lake, I have come to realize that nature and us mortals are entwined in a relationship of mutual respect, where desires and worship co-exist. My lure for the giant fish is no blight to the gods, but simply an expression of my admiration and curiosity towards their creation.\"\n\nThe Sacred Maiden listened, her firm beliefs gently challenged by the old man's perspective. It seemed the ties between devotion, respect and personal ambition were not as starkly delineated as she had considered them to be. The lake was their mirror, reflecting their actions and aspirations, but also their reverence for the natural world.\n\nThe sun began to dip as the day aged, rippling the lake's surface with shades of molten gold and crimson. The Northern Shore, with its squabbles and serenity, stood as a testament that personal aspirations and eternal respect for the natural world were not at odds, but rather interwoven threads of the vast and wondrous fabric of life. The day ended on a peaceful note, the setting sun a bridge between the realms of the mythical and the mundane, the Lake Dweller and the Sacred Maiden both integral parts of a symphony composed by the hands of nature and time.

It generates clear and strong description of the JSON object in a stylistic way.


## Part 2 Create Fine-Tuning Data and Call Fine-Tuned Model

Generally, the fine-tuned model works well, and outputs JSON objects with correct format.

However, one of the problem I met was that, if the example that we provided is already know by ChatGPT, it will have more hallucination. For example, by using the example:

Nearly ten years had passed since the Dursleys had woken up to find their nephew on the front step, but Privet Drive had hardly changed at all.
The sun rose on the same tidy front gardens and lit up the brass number four on the Dursleys' front door;
it crept into their living room, which was almost exactly the same as it had been on the night when Mr. Dursley had seen that fateful news report about the owls.
Only the photographs on the mantelpiece really showed how much time had passed.
Ten years ago, there had been lots of pictures of what looked like a large pink beach ball wearing different-colored bonnets - but Dudley Dursley was no longer a baby, and now the photographs showed a large blond boy riding his first bicycle, on a carousel at the fair, playing a computer game with his father, being hugged and kissed by his mother.
The room held no sign at all that another boy lived in the house, too.
Yet Harry Potter was still there, asleep at the moment, but not for long.
His Aunt Petunia was awake and it was her shrill voice that made the first noise of the day.

Since ChatGPT knows that this paragraph is about Harry Potter, the JSON object it generated includes locations such as "Hogwarts", "Train Station", which are not provided in the example scene.

If we prevent mentioning the name of the characters or the locations, there will be less relevant hallucination, but it still generates characters and locations that are not mentioned.


## Part 3

My part 3 is base on Stardew Vally, while I didn't explicitly mention it in order to reduce hallucination.

I first wrote narrative description of the scenes, such as

Kim, the player, comes to the Town.
As Kim steps into the sunlit embrace of the Town, the quaint charm of this close-knit community unfolds before them,
Pierre's General Store stands as a beacon of local commerce and community, its shelves laden with the fruits of the valley, tended by Pierre himself, a merchant passionate about his produce and the townsfolk.
Nearby, the Clinic offers solace and healing, under the careful watch of Harvey, the town doctor, whose dedication to the health of Pelican Town's residents is as steadfast as the seasons.
The heart of the town, however, beats strongest at the Community Center, a once-vibrant hub now shrouded in mystery and silence, waiting for someone to uncover its secrets and restore it to its former glory.
In contrast, the Stardrop Saloon buzzes with life, a nightly retreat for the townspeople, where Gus, the jovial barkeep, serves up warmth, stories, and the occasional hearty meal beneath the glow of neon lights.
Yet, casting a long shadow over these staples of small-town life is the JojaMart, a gleaming monument to modern convenience.
Managed by Morris, its presence looms as a stark reminder of the encroaching outside world, challenging the town's traditional ways with its cold, fluorescent allure.
The west entrance to town connects to The Farm.
Directly south of town is the Beach.
North of town connects to The Mountain.

However, although I introduced each character that should appear in the Town, the model doesn't recognize them well. Alternatively, I changed the scene description to a more straight-forward way:

Kim is a character. Kim is the protagonist of the game.
Pierre is another character. Pierre is the owner of Pierre's General Store.
Harvey is another character. Harvey is the towrn doctor who works at the Clinic.
Gus is another character. Gus is the owner of Stardrop Saloon, a bar and restaurant.
Morris is another character. Morris is the owner of the JojaMart.
The west entrance to Town connects to Farm.
Directly south of town is Beach.
Pierre's General Store stands as a beacon of local commerce and community,
its shelves laden with the fruits of the valley, tended by Pierre himself, a merchant passionate about his produce and the townsfolk.
Nearby, the Clinic offers solace and healing, under the careful watch of Harvey,
the town doctor, whose dedication to the health of Pelican Town's residents is as steadfast as the seasons.
Stardrop Saloon buzzes with life, a nightly retreat for the townspeople, where Gus,
the jovial barkeep, serves up warmth, stories, and the occasional hearty meal beneath the glow of neon lights.
Managed by Morris, its presence looms as a stark reminder of the encroaching outside world,
challenging the town's traditional ways with its cold, fluorescent allure.

Then the model works much better with this kind of description.

One problem is that, although I emphasized that the player's name is "Kim", the model doesn't generate this character. Instead, it has every NPC in the character list.

I solved this by hard coding ---- add the player to the custom_game.json file to make the game work. Future steps of the project can be modifying the starter code so it can distinguish the player and the NPCs.
