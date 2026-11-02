# QA & Telemetry Devlog

## Planning (78)
For this project, I needed to create a player feedback form and implement a system that collects gameplay information from Blush Bay. I chose JSON files because they are easy to read, store and compare. Each recorded session creates a separate file which should allow me to examine differences between playtests. I focused on fish rescued, fish treated and total playtime because these relate directly to the game's main rescue and rehabilitation loop throughout a complete gameplay session.

## Development and Implementation (158)
I created a C# script called BlushBayTelemetry.cs in Unity. It contains a `[Serializable]` session class that stores the game version, platform, date, rescue count, treatment count and playtime. Making the class serialisable allows Unity's `JsonUtility` to convert this information into JSON. I connected the telemetry to my existing `RescueManager.cs` script by calling `RecordRescue` when a fish is collected from a net and `RecordTreatment` when treatment is applied. These calls use the telemetry singleton instance and the null-conditional operator `(?.)` which avoids an error if the instance is unavailable. I used HashSet collections to prevent the same fish being counted repeatedly even if it receives multiple treatments. The script then uses `JsonUtility.ToJson()` with indentation enabled and `File.WriteAllText()` to save the results. Initially, recording stopped when I changed scenes. I found that the manager was not a root GameObject which prevented `DontDestroyOnLoad()` from working correctly. Moving it to the root of the Hierarchy allowed the telemetry to exist across scenes.

## Player Feedback and Consent (106)
I designed a Google Form with two sections. The first asks for consent: selecting no takes the participant directly to submission while selecting yes opens the testing questions. These include quantitative ratings about enjoyment and difficulty alongside qualitative questions about bugs and the overall experience. When I have play testers these answers could help explain telemetry results. For example, a low rescue count might be linked to confusing instructions. The telemetry script also includes a Consent Granted setting in the Unity Inspector. During testing I would enable it after being given permission.

## Testing and Challenges (88)
I carried out my own development test by rescuing and treating fish then checking the exported JSON. One test file recorded two rescued fish, two treated fish and approximately 47.5 seconds of playtime. This demonstrated that the event counters and file export were working in my test. I also checked that the telemetry continued working after travelling between scenes.

## Reflection and Future Improvements (71)
This project gave me a way to gather gameplay statistics alongside feedback once participants start testing. I could compare session counts and playtime with survey responses to identify confusing mechanics and prioritise improvements rather than relying on numbers alone. In the future I would replace the Inspector consent setting with an in-game choice. I could also record failed treatment attempts, released fish and visitor donations to understand more of the gameplay loop.

## Final outcome (45)
Overall, I successfully implemented a working JSON telemetry system that records rescued fish, treated fish and gameplay duration. I also created a consent-based player feedback form. Development testing confirmed that the telemetry system works although the form has not yet been used with external playtesters.

[YouTube video](https://youtu.be/eb7zRUNFZ4w)

[Google form for user testing](https://forms.gle/Ncn8pdd8hUH3GoddA)

*546 words in total*