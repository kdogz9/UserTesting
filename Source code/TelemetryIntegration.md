# Telemetry integration 

In order for the JSON file to track how many fish players rescue and treat during their gameplay, I needed to integrate my telemetry system into an existing script called `RescueManager.cs`.

This script already manages rescued fish, their health and their treatment, so I used it to record these gameplay events automatically.

``` 
  fish.health += healthIncrease;

        if (fish.health > fish.maxHealth)
        {
            fish.health = fish.maxHealth;
        }

        fish.hasBeenTreated = true;
        // Record this fish receiving treatment.
        BlushBayTelemetry.Instance?.RecordTreatment(fish);

        Debug.Log("Treated " + fish.fishName + ". Health is now " + fish.health);
    }
``` 
*Code for recording treated fish.*

I added the `RecordTreatment` method after the fish's health is updated. This means that when a player completes a treatment in the ointment mini-game the telemetry system records that the fish has received treatment.

The telemetry system also checks whether that fish has already been counted to prevent repeated treatments from increasing the number of unique animals treated.

```
 if (!net.hasFish || net.fish == null)
        {
            Debug.Log("This net is empty.");
            return false;
        }

        collectedFish = net.fish;
        fishWaitingForTank.Add(collectedFish);
        // Record this fish as successfully rescued.
        BlushBayTelemetry.Instance?.RecordRescue(collectedFish);

        net.hasFish = false;
        net.fish = null;

        Debug.Log("Collected fish: " + collectedFish.fishName);

        return true;
    }
``` 
*Code for recording rescued fish.*

I added the `RecordRescue` method after the fish has been collected from a rescue net.

This ensures that a fish is only recorded as rescued when the player actually collects it.