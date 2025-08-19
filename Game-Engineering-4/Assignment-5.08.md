# Assignment 5.08: Group Logic

## Requirements

- Carry over the same key/door/treasure logic from the **State Machines** and **Behavior Trees** assignments.
- Make two actors that utilize the same AI controller.
  - The AI controller can be based on either a State Machine or a Behavior Tree... your choice.
  - The two actors must spawn at random locations.
  - The two actors may have a shared inventory for storing keys.
- Your AI should exhibit reasonably good "squad behavior".
  - The AI should work together to gather all the keys, open all the doors, and collect the treasure as quickly as possible.
  - Each AI entity communicates with fellow team members via messages.
  - Messages include **Intents** like **I'm going for the red key**, and **Status** like **I've just collected the red key**
  - Each AI builds up his own data, called the **Situation**, based on these messages. It can then base it's next decision on that situation.
- Create a scrolling **Message Event Window** where all the AI messages are displayed.