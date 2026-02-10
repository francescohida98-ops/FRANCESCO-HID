import random
import json
import urllib.request

# -------------------------------
# STEP 3: Load questions from JSON (GitHub)
# -------------------------------
url = "https://raw.githubusercontent.com/mehmetmeral/quiz_questions/main/questions.json"

with urllib.request.urlopen(url) as response:
    quiz_questions = json.loads(response.read().decode())

# Each question format:
# ("Question", "Correct Answer", "Wrong1", "Wrong2", "Wrong3")

# -------------------------------
# Function to run quiz for a player
# -------------------------------
def play_quiz(player_name):
    print(f"\n🎮 {player_name}'s turn!")
    score = 0

    # Select 5 random questions
    selected_questions = random.sample(quiz_questions, 5)

    for i, q in enumerate(selected_questions, 1):
        question = q[0]
        correct_answer = q[1]
        options = list(q[1:])  # Convert answers to list

        random.shuffle(options)

        print(f"\nQuestion {i}: {question}")
        for idx, option in enumerate(options, 1):
            print(f"{idx}. {option}")

        try:
            choice = int(input("Choose your answer (1-4): "))
            user_answer = options[choice - 1]

            if user_answer == correct_answer:
                print("✅ Correct!")
                score += 1
            else:
                print(f"❌ Wrong! Correct answer: {correct_answer}")
        except:
            print("❌ Invalid input!")

    print(f"\n{player_name}'s final score: {score}/5")
    return score

# -------------------------------
# MAIN PROGRAM
# -------------------------------
print("🎯 WELCOME TO THE PYTHON QUIZ GAME 🎯")

player1 = input("Enter Player 1 name: ")
player2 = input("Enter Player 2 name: ")

score1 = play_quiz(player1)
score2 = play_quiz(player2)

print("\n🏆 FINAL RESULTS 🏆")
print(f"{player1}: {score1}")
print(f"{player2}: {score2}")

if score1 > score2:
    print(f"🎉 Winner: {player1}")
elif score2 > score1:
    print(f"🎉 Winner: {player2}")
else:
    print("🤝 It's a tie!")
