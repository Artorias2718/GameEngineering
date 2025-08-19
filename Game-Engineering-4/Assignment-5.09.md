# Assignment 5.09: Capture The Flag

## Requirements

- Make a 3 vs 3 (at least) game of "Capture the flag".
  - Rather than try to explain all the rules here, just watch this [video](http://www.howcast.com/videos/218017-How-to-Play-Capture-the-Flag)
  - Ensure that all the basic rules function correctly: Flags for each team, prison for each team, an easily discernible "territory line".
  - CTF rule questions?  Ask in the class channel on Slack so everyone can see the answer.
- Your AI should exhibit reasonably good **squad behavior**.
  - Each AI entity communicates with fellow team members via messages.
  - Messages include **Intents** like **I'm going in for the flag**, and **Status** like **I've just been captured**
  - Each AI builds up his own data, called the 'situation', based on these messages.  It can then base it's next decision on that situation.
- Create a scrolling **Message Event Window** where all the AI messages from one team are displayed.