import streamlit as st
import random

# Function to generate a random number
def generate_number():
    return random.randint(1, 100)

# Streamlit app structure
st.title("Number Guessing Game")

# Session state to track the game status
if 'target_number' not in st.session_state:
    st.session_state.target_number = generate_number()
    st.session_state.attempts = 0
    st.session_state.game_over = False

# Input field for the user's guess
user_guess = st.number_input("Guess a number between 1 and 100", min_value=1, max_value=100)

# Button to submit the guess
if st.button("Submit Guess"):
    if not st.session_state.game_over:
        st.session_state.attempts += 1
        if user_guess < st.session_state.target_number:
            st.write("Too low! Try again.")
        elif user_guess > st.session_state.target_number:
            st.write("Too high! Try again.")
        else:
            st.write(f"Congratulations! You guessed the number {st.session_state.target_number} correctly in {st.session_state.attempts} attempts.")
            st.session_state.game_over = True
    else:
        st.write("Game over! You've already guessed the number.")
        restart_button = st.button("Play Again")
        if restart_button:
            st.session_state.target_number = generate_number()
            st.session_state.attempts = 0
            st.session_state.game_over = False
            st.experimental_rerun()

# Display the number of attempts
if not st.session_state.game_over:
    st.write(f"Attempts: {st.session_state.attempts}")
