# AnimeBot: Anime Recommendation Chatbot - University Project

A rule-based chatbot that recommends anime by genre. It matches what you type against regular expressions, pulls its intents and responses from a JSON file, and replies from a set of hand-picked recommendations.

Built in Python with no external libraries.

## Features

- **Genre recommendations:** ask for a genre ("recommend action anime", "suggest a fantasy show") and get four picks
- **Genre keywords:** type a genre on its own ("comedy") and the bot prompts you to ask for recommendations
- **Top anime:** ask for "top anime" or "best anime" for a list of popular series
- **Greetings and goodbyes**, with randomly chosen replies so the bot doesn't repeat itself
- **Data-driven design:** intents, patterns, and responses live in `intents.json`, so you can add new ones without touching the code
- **Memory file:** a `memory.json` file and a `show memory` command for storing user details

### Supported genres

Action, romance, comedy, fantasy, isekai, slice of life, horror, psychological, sports, and mecha.

## Getting started

You need Python 3 and Jupyter (or VS Code with the Jupyter extension).

1. Clone the repository:
   ```bash
   git clone https://github.com/tomisinse/AnimeChatbot.git
   cd AnimeChatbot
   ```
2. Open `chatbott.ipynb`.
3. Run the cells from top to bottom. The cell with the `while True` loop starts the chat.
4. Type a message at the `You:` prompt. Type `exit` or `quit` to leave.

The notebook expects `intents.json` and `memory.json` to be in the same folder.

## Example conversation

```
You: hi
AnimeBot: Hey there! Need anime suggestions?
You: recommend action anime
AnimeBot: For action, I recommend: Jujutsu Kaisen, Attack on Titan, Demon Slayer, Vinland Saga
You: comedy
AnimeBot: comedy? Nice choice. Want a recommendation?
You: top anime
AnimeBot: Top anime right now: Chainsaw Man, Jujutsu Kaisen, Demon Slayer, Attack on Titan.
You: goodbye
AnimeBot: See you later! Come back for more anime recs!
```

Replies are picked at random from several options per intent, so your output may differ.

## How it works

1. `intents.json` defines each intent: a tag, a list of regex patterns, and a list of possible responses.
2. On startup, the notebook compiles every pattern with `re.compile(..., re.I)`, so matching ignores case.
3. `match_intent()` checks the user's message against each intent in order and uses the first match.
4. Capture groups such as `recommend (.*) anime` pull out the genre, which is looked up in the `anime_recommendations` dictionary and filled into the response template.

## Project structure

```
├── chatbott.ipynb   # Bot logic, chat loop, and test cells
├── intents.json     # Intents, regex patterns, and response templates
├── memory.json      # Saved user data (starts empty)
└── README.md
```

## Adding your own content

- **New genre:** add an entry to the `anime_recommendations` dictionary in the notebook, and add the genre name to the `genres` patterns in `intents.json`.
- **New response or intent:** edit `intents.json`. Use `{0}` for the first captured group and `{recommendation}` where the genre's picks should appear.

## Known limitations

- **Memory is not wired up yet.** Saying "my favourite anime is One Piece" gets a reply, but nothing is written to `memory.json`. Asking "what anime do i like" returns the template text `{memory}` instead of a saved answer.
- **Matching is loose.** Patterns are searched anywhere in the message, so "hi" also matches inside words like "something". Anchoring patterns with `\b` word boundaries would fix this.
- **Some phrases match the wrong intent.** "i like sports" is caught by the genre keyword intent, and "what anime do i like" can collide with the `i like (.*)` pattern.
- **Rule-based only.** The bot understands the phrasings in `intents.json` and nothing else.

## Ideas for improvement

- Save favourites and liked genres to `memory.json` and recall them
- Use word boundaries and reorder intents to stop false matches
- Add more genres and a "surprise me" option
- Add fuzzy matching or an NLP library for more flexible input

## License

Add a license of your choice (for example, MIT) before sharing publicly.
