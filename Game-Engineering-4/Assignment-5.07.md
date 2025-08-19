# Assignment 5.07: Behavior Trees

## Requirements

- Carry over the same key/door/treasure logic from the **State Machines** assignment.
- Replace the state machine with a behavior tree.
  - Hopefully you've abstracted the **actions** and **conditionals** in such a way that they can be re-used by the behavior tree without too much effort.
  - Make a visual "log" that shows at least the last 10 node return value changes.  If a node returns "Running" on the first frame, you do NOT want to log "Running" on the next frame... node *changes* only.
- Reference File: BehaviorTree.cs
- The reference code file is based on this article:
- [Behavior Trees for AI](https://www.gamedeveloper.com/programming/behavior-trees-for-ai-how-they-work)
- More reference code examples:
  - [An Introduction to Behavior Trees](http://obviam.net/index.php/game-ai-an-introduction-to-behavior-trees/)
- Also, for an example of a partial CTF behavior tree implementation, check out [this video](http://www.opsive.com/assets/BehaviorDesigner/videos.php?id=7)
- They also have one about their implementation of [**Conditional Aborts**](http://www.opsive.com/assets/BehaviorDesigner/videos.php?id=11) if you want to implement something similar: