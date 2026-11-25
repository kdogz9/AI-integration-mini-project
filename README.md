# AI-integration-mini-project

## Planning and objectives (86 words) 

This task required us to implement an AI feature. I chose to use Unity's Behavior Trees to create a visitor NPC within my FMP as this was a feature I had planned to develop for Blush Bay. I wanted the visitors to wait until a rescued fish had been placed in a tank, walk over to view it and show a heart bubble before then going to donate and leave. This system needed to access my exisiting `RescueManager` script to identify which tanks contained fish. 

## Development and implementation (161 words)

To create the visitor AI, I used Unity's Behavior Tree as it allows me to give the NPC different behaviours and decisions in the future. I started with a sequence node which allows the NPC to complete different tasks in a specific order. 

![VisitorBehaviorTree]()

I then created a custom C# action called `FindOccupiedTankAction` which checks the exisiting `RescueManager` script to identify whether any of the tanks contained fish. When a fish is found in the tank, the script assigns the tank's `VisitorStandPoint` to a Blackboard variable called `TargetTank`. 
```C# 
var fishInTank = RescueManager.Instance.GetFishInTank(tankId);

if (fishInTank != null && fishInTank.Count > 0)
{
    // Tank contains at least one fish.
}
```
I used Unity's `NavMesh surface` and `NavMesh Agent` to allow the NPC to navigate around the aquarium. Once the visitor reaches the tank, the Behavior Tree activates a heart bubble for 2 seconds before hiding it again. The visitor then moves to the donation box where another custom C# action `MakeDonationAction` communicates with the `CurrencyManager` to add 10 blush coins to the players balance. 

```C# 
CurrencyManager.Instance.ReceiveDonation(DonationAmount);

hasDonated = true;

return Status.Success;
```
The visitor then walks to the `VisitorExitPoint` and is deactivated. 

## Testing, challenges and solutions (136 words) 

During development, I tested the visitor AI by placing rescued fish into different tanks and observing how the NPC responded. One issue I encountered was that the heart bubble appeared for some tanks but not others even though the visitor had stopped moving. Using Unity's Behaviour Tree debugger, I discovered that the navigation action was still running because the visitor had not successfully reached its target. I adjusted the `VisitorStandPoint` positions to ensure they were accessible on the `NavMesh`.

Another issue was that the visitor would sometimes appear underneath the floor or navigate to the wrong location. I investigated this using Console logs to check navigation paths and positions before adjusting the `NavMesh` and the agent's Base Offset. Finally, I tested that donations updated the currency UI and that the visitor disappeared after reaching the exit.

## Reflection and future improvements (86 words)

The visitor AI makes the aquarium feel more alive and reinforces the core gameplay loop by rewarding players with donations for rescuing and caring for fish. This project helped me understand how Behaviour Trees can organise NPC actions. However, the system still needs polishing such as making visitors wander while waiting and moving the exit off-screen. In the future, I would like to introduce multiple visitors with different needs and donation amounts based on fish stats, encouraging players to maintain healthier fish and improve their aquarium.

## Deliverables 

- `VisitorAIPackage.unitypackage` includes all the files implemented for this task 

- [BlushBay GitHub Repo](https://github.com/kdogz9/Blush-Bay-Ocean-Rescue-2.5D1) - Unity version 6000.4.10f1

- [YouTube video](https://youtu.be/PX3DqFy78EA)
## Declaration and Sources 

- ChatGPT was used to teach and implement the code 

About Unity Behavior | Behavior | 1.0.17 (s.d.) At: https://docs.unity3d.com/Packages/com.unity.behavior@1.0/manual/index.html (Accessed  25/11/2026).

[Tutorial] Build Complex Enemy AI in Minutes with Behavior Trees - AI & Machine Learning (2026) At: https://discussions.unity.com/t/tutorial-build-complex-enemy-ai-in-minutes-with-behavior-trees/1738542 (Accessed  25/11/2026).

Bring Your NPCs to Life! (Unity NPC Wandering Tutorial / BD PRO) (2025) Directed by FuzzyIndie. At: https://www.youtube.com/watch?v=KPVc3FcM8FA (Accessed  25/11/2026).

EASY way to add AI in Unity 6 - Behavior Package Tutorial (2024) Directed by Sunny Valley Studio. At: https://www.youtube.com/watch?v=n5m2aI6Wk8E (Accessed  25/11/2026).

