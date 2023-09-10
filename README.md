<h1 align="center">🏏 Hand Cricket Game 🤖</h1>

<p align="center">
  <img src="https://github.com/Azlaan20/Hand_Cricket_Game/blob/master/1.gif" alt="Hand Cricket Game" width="400">
</p>

<p align="center">
  <em>A classic hand cricket game with an advanced AI twist!</em>
</p>

---

## 💡 Overview

Welcome to the world of Hand Cricket! 🌟 This is a thrilling command-line-based cricket game where you face off against an advanced AI opponent. The goal? Outscore your digital adversary or dismiss the AI batsman with your cunning bowling skills. Are you up for the challenge? 🏆

## 🎮 How to Play

1. Run the `play_game()` function from the Python script.
2. Use your cricketing instincts to choose a number of fingers (1 to 6) while batting or bowling.
3. Swing for the fences while batting and aim to accumulate as many runs as you can.
4. When bowling, strategically match the AI batsman's choice of fingers to claim a wicket.

## 📜 Rules

- When batting, be careful! If your choice of fingers matches the AI's, you're caught out!
- Runs scored while batting are tallied to your total score.
- The game proceeds for a specified number of thrilling overs.
- The player with the highest cumulative score at the end of the game emerges victorious!

## ⚙️ Requirements

- Python 3.x

## 🚀 Usage

1. 📥 Clone this repository to your local machine.
2. 🏏 Fire up your terminal and run the `hand_cricket.py` script using Python.

```bash
import random

class AdvancedAI:
    def __init__(self):
        self.choices = [1, 2, 3, 4, 5, 6]  # Possible choices (number of fingers)
        self.q_table = {}
        self.learning_rate = 0.2
        self.discount_factor = 0.9
        self.epsilon = 0.2

    def get_choice(self, state):
        # Exploration parameter, 0.2 means 20% exploration, 80% exploitation
        if random.random() < self.epsilon:
            # Exploration: randomly select a choice
            return random.choice(self.choices)
        else:
            # Exploitation: select the choice with the highest average score
            if state not in self.q_table:
                return random.choice(self.choices)
            else:
                q_values = self.q_table[state]
                max_q = max(q_values.values())
                best_choices = [choice for choice, q_value in q_values.items() if q_value == max_q]
                return random.choice(best_choices)

    def update_q_table(self, state, action, reward, next_state):
        if state not in self.q_table:
            self.q_table[state] = {action: reward}
        else:
            if action not in self.q_table[state]:
                self.q_table[state][action] = reward
            else:
                q_value = self.q_table[state][action]
                max_next_q = max(self.q_table[next_state].values()) if next_state in self.q_table else 0
                new_q_value = q_value + self.learning_rate * (reward + self.discount_factor * max_next_q - q_value)
                self.q_table[state][action] = new_q_value

    def update_hyperparameters(self, total_runs_player, total_runs_ai):
        # Modify hyperparameters based on the total runs scored by the player and AI
        if total_runs_player >= 50 or total_runs_ai >= 50:
            self.learning_rate = 0.5  # Decrease learning rate for more stable performance
            self.discount_factor = 0.95  # Slightly reduce discount factor for short-term focus
            self.epsilon = 0.01  # Lower exploration rate for more exploitation

        if total_runs_player >= 100 or total_runs_ai >= 100:
            self.learning_rate = 0.3  # Reduce learning rate for fine-tuning
            self.discount_factor = 0.99  # Slightly increase discount factor for long-term focus
            self.epsilon = 0.005  # Lower exploration rate for more exploitation

    def get_babar_azam_score(self):
        # Generate Babar Azam-like batting score within the range of 1-6
        return random.randint(1, 6)

    def get_james_anderson_wickets(self):
        # Generate James Anderson-like wicket count within the range of 1-6
        return random.randint(1, 6)

    def get_commentator_phrase(self, event, player_name):
        batting_phrases = {
            "wicket": ["The " + player_name + " has been dismissed!", "He's got a good ball!", "That's a plumb lbw!", "He's been caught behind!", "He's edged it to the slips!"],
            "six": ["That's gone for six!", "He's hit it out of the park!", "That's a monster!", "He's got all the shots in the book!", "He's in the zone!"],
            "four": ["That's four!", "He's found the boundary!", "He's timing the ball beautifully!", "He's got the measure of the bowler!", "This is a great innings!"],
            "run": ["He is off and going!", "Smart Cricket from him!", "He must run faster!", "He's rotating the strike!", "He's keeping the scoreboard ticking over!"],
            "five": ["He's scored a rare five!", "That's a very unusual shot!", "He's got the timing and the power!", "He's a very talented batsman!", "This is a special innings!"],
            "win": ["They've won the match!", "That's a great victory!", "They've played some outstanding cricket!", "They're deserving winners!", "This is a historic moment!"],
            "excitement": ["This is a thrilling contest!", "It's anybody's game!", "This is a nail-biter!", "This is a classic!", "This is the stuff of legends!"]
        }

        bowling_phrases = {
            "wicket": ["What a delivery from " + player_name + "!", player_name + " has struck with a beauty!", "He's knocked the stumps over!", "Bowled him!", "Clean bowled!"],
            "six": ["That's a big hit from the batsman!", player_name + " couldn't escape from that one!", "He tried the yorker, but the batsman was up to the task!", "That's a maximum for the batsman!", "A big one from the batsman!"],
            "four": ["That's a boundary conceded by " + player_name + ".", player_name + " got punished for that loose delivery!", "He's given too much room to the batsman!", "That's a delightful shot!", "The batsman found the gap perfectly!"],
            "run": [player_name + " concedes a few.", player_name + " gives away a couple of runs.", player_name + " leaks a few runs here.", "The batsmen take a few runs.", "It's a good over by " + player_name + "."],
            "five": ["Oh, that's a misfield from " + player_name + "!", player_name + " will be disappointed with that effort!", "He let the team down with that fielding!", "The batsmen took advantage of the fielding lapse!", "A costly mistake from " + player_name + "!"],
            "lose":["They've lost the match!", "That's a disappointing loss!", "They've been outplayed!", "They're deserving losers!", "This is a historic moment!"],
        }

        if player_name == "batsman" and event in batting_phrases:
            return random.choice(batting_phrases[event])
        elif player_name == "bowler" and event in bowling_phrases:
            return random.choice(bowling_phrases[event])
        else:
            return "No commentary available for this event."

def play_game():
    ai = AdvancedAI()
    total_runs_player = 0
    total_runs_ai = 0
    OVERS = 0
    overs = 1  # You can change this value to play for more overs
    bowls = 1
    print("Playing against the advanced AI in Hand Cricket...")
    player_batsman = True
    player_bowler = False

    while bowls < overs * 7:  # 6 balls per over
        print()
        print("Bowler bowls...")
        ai_bowler_choice = random.choice(ai.choices)

        if player_batsman:
            print()
            print("You are batting...")
            player_choice = int(input("Enter the number of fingers (1 to 6): "))
            print("Bowler shows", ai_bowler_choice, "fingers.")

            if player_choice < 1 or player_choice > 6:
                print("Invalid choice. Please enter a number between 1 and 6.")
                continue

            if player_choice == ai_bowler_choice:
                print()
                print("You're out!")
                ai.update_q_table(str(total_runs_player), str(player_choice), -1, str(total_runs_ai))
                player_batsman = False
                player_bowler = True
                print("You batted for", OVERS, "overs and", bowls, "bowls.")
                OVERS = 0
                bowls = 1
                overs+=1
                print(ai.get_commentator_phrase("wicket", "batsman"))
                continue

            else:
                runs_scored = player_choice
                total_runs_player += runs_scored
                print()
                print("You scored", runs_scored, "runs!")
                if runs_scored == 1 or runs_scored == 2 or runs_scored == 3:
                    print(ai.get_commentator_phrase("run", "batsman"))
                elif runs_scored == 4:
                    print(ai.get_commentator_phrase("four", "batsman"))
                elif runs_scored == 5:
                    print(ai.get_commentator_phrase("five", "batsman"))
                elif runs_scored == 6:
                    print(ai.get_commentator_phrase("six", "batsman"))

                next_state = str(total_runs_player + runs_scored)
                ai.update_q_table(str(total_runs_player), str(player_choice), runs_scored, next_state)
                bowls += 1
                ai.update_hyperparameters(total_runs_player, total_runs_ai)

                if bowls > 6:
                    print()
                    print("Over Complete!")
                    OVERS += 1
                    bowls = 1
                    overs+=1
                    print(ai.get_commentator_phrase("excitement", "batsman"))

        else:
            print()
            print("Your turn to bowl...")
            bowler_choice = int(input("Enter the number of fingers (1 to 6): "))
            # AI's turn to bat
            ai_batsman_choice = ai.get_choice(str(total_runs_ai))

            if ai_batsman_choice == str(bowler_choice):
                print()
                print("AI showed", ai_batsman_choice, "!")
                print("AI is out!")
                print(ai.get_commentator_phrase("wicket", "bowler"))

                ai.update_q_table(str(total_runs_ai), str(ai_batsman_choice), -1, str(total_runs_player))
                player_batsman = True
                player_bowler = False
                print()
                print("AI batted for", OVERS, "overs and", bowls, "bowls.")
                OVERS = 0
                bowls = 1
                break


            else:
                runs_scored = int(ai_batsman_choice)
                total_runs_ai += int(runs_scored)
                print()
                print("AI scored", runs_scored, "runs!")
                if runs_scored == 1 or runs_scored == 2 or runs_scored == 3:
                    print(ai.get_commentator_phrase("run", "bowler"))
                elif runs_scored == 4:
                    print(ai.get_commentator_phrase("four", "bowler"))
                elif runs_scored == 5:
                    print(ai.get_commentator_phrase("five", "bowler"))
                elif runs_scored == 6:
                    print(ai.get_commentator_phrase("six", "bowler"))

                next_state = str(total_runs_ai + runs_scored)
                ai.update_q_table(str(total_runs_ai), str(ai_batsman_choice), runs_scored, next_state)
                bowls += 1
                ai.update_hyperparameters(total_runs_player, total_runs_ai)

                if bowls >= 6:
                    print()
                    print("Over Complete!")
                    OVERS += 1
                    bowls = 1
                    overs+=1
                    print(ai.get_commentator_phrase("excitement", "bowler"))
                    continue

                if total_runs_ai > total_runs_player:
                    break

    print("Your total score:", total_runs_player)
    print("AI's total score:", total_runs_ai)

    if total_runs_ai > total_runs_player:
        print(ai.get_commentator_phrase("lose", "bowler"))
    elif total_runs_ai < total_runs_player:
        print(ai.get_commentator_phrase("win", "batsman"))
    else:
        print("It's a draw!")

    print("Game over!")
```

🙌 Credits
This exciting game of Hand Cricket was crafted with passion by Azlaan Ranjha. Give it a try and let us know your high score! 🎉

<p align="center">
  Let's play some cricket and have a blast! 🏏🎉
</p>
