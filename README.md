# Load Testing a Game on `efish-n-sea.github.io` Using Loadster

This guide will walk you through the steps to use Loadster for performing load testing on a game hosted at [Moving Day](https://efish-n-sea.github.io/Pages/IDS/IDS.html).

## Step 1: Register a Loadster Account

First, go to the [Loadster website](https://loadster.app/) and register for an account. Each account comes with 50 Loadster Fuel credits, which can be used for running load tests. **You do not need to spend any money on Loadster.**

## Step 2: Create a Browser Script

Since the game includes Unity content, we will create a **Browser Script**. The Browser Script simulates user interactions like clicks and drags, which are common in Unity-based games.

## Step 3: Record the Script

Click on the **Record** button to start recording the script, as shown in Figure 1.

![image](https://github.com/user-attachments/assets/d9533dbd-5d3c-4828-b2ba-9ab8d53e5f8c)

*Figure 1: The Record button in Loadster*

In the recording configuration screen (Figure 2), enter the URL of the game. When the recording begins, you might be prompted to install the Loadster browser extension. After installation, the game page will open automatically.

![image](https://github.com/user-attachments/assets/dceacf37-65ee-4570-acb9-649441ec0299)

*Figure 2: Recording Configuration Screen*

Once the game loads, you can play the game briefly to verify that it functions correctly. To save time, you can close the page once you've confirmed it loads and plays.

## Step 4: Adjust the Script

After recording, Loadster will generate a sequence of steps in the Script. You can validate that the Script runs correctly by viewing the steps below. 
![image](https://github.com/user-attachments/assets/4fca8a34-b421-4902-a9d0-1753d96d56bb)
I added a step to **WAIT FOR `#unity-canvas` to be visible** to confirm that the game canvas is loaded successfully. If `#unity-canvas` is not visible, an error will be recorded, indicating a loading issue.

## Step 5: Set Up Load Testing

Next, initiate load testing by clicking on the **Load Testing** button and selecting **New Scenario** to create a new test scenario.
![image](https://github.com/user-attachments/assets/8ffb9e5f-0e57-40c9-ab34-8297ad47d645)


## Step 6: Apply a Binary Search Method for Load Testing

As an example, I'm using a dichotomy to find maximum capacity. You can also use the method you like.

Use a binary search approach to find the threshold of users the game can handle before performance degrades. Run each test with a single group:
![image](https://github.com/user-attachments/assets/d638298d-1271-422e-b1b2-d31ff8b65077)


1. **Test 1**: Start with 10 bots in the group. Observe the results; I found zero errors and an average response time of 0.017 seconds.
2. **Test 2**: Increase to 100 bots and observe an average response time of 0.339 seconds, indicating a significant delay. This suggests that 100 bots may be too high.
3. **Test 3**: Set the bot count to 50. The average response time was 0.015 seconds, similar to Test 1, indicating that 50 bots do not impact performance.
4. **Test 4**: Increase the count to 75 bots, resulting in an average response time of 0.532 seconds, showing noticeable impact.
5. **Test 5**: Set the count to 62 bots, with an average response time of 0.369 seconds, confirming that the threshold is affected.

After Test 5, I used all the Loadster Fuel credits available.

**Result**: Based on the binary search approach, the ideal user load for this game appears to be around 50-62 users.

---

> **Note:** You do not need to spend any money on Loadster. You just need to experiment with your favorite method, and when the Loadster Fuel is exhausted, analyze the maximum capacity of the stress test according to the existing results.

