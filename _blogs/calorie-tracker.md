---
title:  "Automated Calorie Counting"
date:   2021-12-05
permalink: calorie-tracker-google-sheets
categories: ["calories","google-sheets","javascript","wellness"]
layout: content
---
# How I calculated calories with Google Sheets 🧮

Coming from a household whose primary staple food was just Indian, it was hard to count my calories with any popular application like MyFitnessPal. The application itself was great. But, they do not have the food items consumed predominantly by Indians, and even if they do, the calorie count seems to be way off. The only way forward was to enter these values manually by creating a recipe for every food item I eat in MyFitnessPal, or to build something myself. The engineer in me chose the latter.

## What to automate?

The absolute basis of the whole program is to know what to automate, as with any software. The process has to be manual first to give me a clear understanding of all the nuances. I want to calculate the calories for every food item I eat. The calculation is more accurate when I know the individual calories of all the food items used in the recipe.

## Constructing the calorie database

The first thing in this is to understand how many calories a food item contains. For most items, the nutrition information label will have the data. For the rest of it, I have to take my best guess. I can do this by collating the nutrition information from multiple sources. I figured the best metric is `calories per gram` to measure my intake.

This led to the creation of a map like this

![food database sample](assets/images/food_db_sample.png){:class="img-fluid"}

Suppose I eat 250 grams of white rice and 200 grams of spinach, then I will cross-reference this with the list and arrive at the number of calories I have consumed and subtract this from the number of calories I have left for the day.

## Deficit Calculation

To understand how many calories I am losing on any given day, I have to first understand my [Basal Metabolic Rate](https://en.wikipedia.org/wiki/Basal_metabolic_rate) and my Total Daily Energy Expenditure(TDEE). You can calculate yours [here](https://tdeecalculator.net/)

So with this, I can have a simple metric `Daily calorie need based on lifestyle - Calories consumed on that day` to understand how many calories I am losing everyday

I decided to adjust the metric to include calories lost due to exercise, and the final formula looked like 

`( Daily calorie need based on lifestyle -  Calories consumed on that day ) + Calories lost due to exercise`

## Logging the food items

I have the calorie database ready, and I have the formula ready. The only thing left for me to do is to keep logging every food item I eat. I decided to follow the csv pattern so my food entry started to look like this 

![food log sample](assets/images/food_log_sample.png){:class="img-fluid"}

Now I am ready to automate the whole entry. Manually calculating the calories for each item by cross lookup in the calorie database was a cumbersome process.

## Automate with Google Appscript

I was delighted to see Google Sheets contained Appscript that is just javascript internally. This means I can stick to making the food entry for the day, and the script would calculate the calories. I suppose Laziness is the mother of all automation.

You can find the code for the script here

<script src="https://gist.github.com/ayrusme/06c09db4f2e395add3f1eed79ab94d86.js"></script>

## Seeing the script in action

I am pretty much set to crush my fitness goals at this point, so I input the data, run the script and watch it calculate the calories, and then the formula renders the deficit for the day. 

![Calorie Tracker](assets/images/calorie_tracker.gif){:class="img-fluid"}

## Learnings, What's next?

Understanding how to calculate macros will be the next step. If I can provide the macro data for everything in the food database, then I can track the macros for them. But, at that point, I feel this becomes a full-blown app by itself. However, if we know certain foods have the essential fats ( more protein fewer carbs ), then this could still track the same nutrition but only from a calorie standpoint.

On the same note, this can be used as a starter application to reduce food cravings and increase healthy eating for three to four months. If we can stick to the same pattern for 120-150 days, then it will become a habit by then. We will also have a general idea of how many calories a food item will have without even entering it into the sheet.

Happy Wellness to you! ✨

#### Resources

[Food Database](https://docs.google.com/spreadsheets/d/1fIVRhOXIL37u-omPIM13pIx5XxoLwDgvhfOgxATXpsk/edit?usp=sharing)
