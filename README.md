# Lisbeth.Macros
Here you'll find a collection of Lisbeth macros that several community members have created. Although Lisbeth has a general crafting AI integrated, these might prove quite useful for specific circumstances. You can also use this repository to report bugs by creating an issue for them.

Macros can be used in two ways. The first is to assign them directly to an order, through the order's individual settings. This forces Lisbeth to use that macro for crafting that order above everything else, regardless of priority or conditionals. The second way to use them is as **overrides**. When Lisbeth crafts something, it'll first check every enabled macro that you have to see if any of their conditionals are valid for the craft that is about to happen. If it finds one, it'll use that instead of the default crafting AI. In other words, it *overrides* the default behavior when the conditionals in the macro are met. This happens with primary and secondary orders (suborders). Only enabled macros are used in this manner.

## Macro Syntax

Macros are text files that reside in the **/Rebornbuddy/Settings/Lisbeth/Macros** folder. The files can be named anything you want, as long as their name is unique within the folder. Below you can see an example of a macro:

```
Name: 2 Star CP 505
Priority: 1
Enabled: True
Conditions: Cp >= 505 && MaxDurability == 70  && Specialized == False && MaxProgress == 3543 

S1: 
- Muscle Memory
- Comfort Zone
- Inner Quiet
- Steady Hand II
- Piece by Piece
- Prudent Touch
- Prudent Touch
- Prudent Touch
- Prudent Touch
- Manipulation II
- Steady Hand II
- Prudent Touch
- Comfort Zone
- Prudent Touch
- Prudent Touch
- Prudent Touch
- Steady Hand II
- Prudent Touch
- Great Strides
- Innovation

S2: Condition == Good
- Ingenuity II
- Byregot's Blessing
- Careful Synthesis III
- Careful Synthesis III
- Careful Synthesis III
- Careful Synthesis III

S2: Condition == Normal
- Ingenuity II
- Byregot's Blessing
- Careful Synthesis III
- Careful Synthesis III
- Careful Synthesis III
- Careful Synthesis III

S2: Condition == Poor
- Ingenuity II
- Byregot's Blessing
- Careful Synthesis III
- Careful Synthesis III
- Careful Synthesis III
- Careful Synthesis III

S2: Condition == Excellent
- Byregot's Blessing
- Ingenuity II
- Careful Synthesis III
- Careful Synthesis III
- Careful Synthesis III
- Careful Synthesis III
```
*(Thanks to ArcticWhiteFox for the macro.)*

As you can see, macros have four properties in addition to the actual skills list. Let's go over what each of them do:

* **Name:** This is the name that will show on Lisbeth's UI. Try keep it the same as the file name for consistency; although this is not a requirement. 
* **Priority:** This is the priority of the macro, which can be any positive integer numbre (i.e. 1, 2, 3, 10234). The lower the number, the higher the priority of the macro. Priority is used by Lisbeth to decide which macro to use when they are enabled as *overrides*. If the craft's circumstances fit the conditional of two or more macros, the one with higher priority (lower number) will be used.
* **Enabled:** This enables the macro as an override. You don't need to enable the macro to use it directly on an order's individual settings.
* **Conditions:** These conditions are checked when searching for a macro to use it as an override. Conditions can only have logical AND (&&) statements. 

## Labels & Segments

As you can see from the example above, you can write multiple sequences of skills, or **segments** on a macro, and give each one a label (i.e. S1, S2). Right beside the label, on the same line, you can add a conditional that uses the same syntax as the macro's general conditional property:

```
S2: Condition == Excellent
```
Lisbeth will execute the macro one **label** after the other, as in S1 => S2 => S3. On each label, it'll choose to use the first segment whose conditional is met. It only uses one segment per label (or none if no segments for that label had their conditional met). This means the relationship between segments with the same label is a logical OR.

*For S2, try use this segment first. If the condition is met, execute the skills and proceed to label S3. If the condition is not met, then try this other segment instead.*

## Skill Priorities

There is another way to change the flow of skill execution apart from labels and segments. On individual steps, you can specify multiple skills in a priority hierarchy by using *>* between them. Take a look at the example below:

```
Name: P60 R61 40DUR (995 Craftsmanship 427 Cp)
Priority: 10
Enabled: True
Conditions: MaxDurability == 40 && Craftsmanship >= 995 && Cp >= 427 && RecipeDisplayLevel == 61

S1:
- Comfort Zone
- Inner Quiet
- Careful Synthesis II
- Steady Hand II
- Precise Touch > Basic Touch
- Precise Touch > Basic Touch
- Master's Mend
- Precise Touch > Basic Touch
- Precise Touch > Basic Touch
- Steady Hand II
- Precise Touch > Basic Touch
- Master's Mend
- Precise Touch > Basic Touch
- Steady Hand II
- Great Strides
- Byregot's Blessing
- Careful Synthesis II
- Careful Synthesis II
```
Here, Lisbeth will try to use Precise Touch for Step 5. If it can't be used for whatever reason, then it'll use Basic Touch instead, then proceed to the next step. To determine if a skill can or cannot be used, the ActionManager.CanCast method is checked.

