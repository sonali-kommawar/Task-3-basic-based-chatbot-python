# Task-3-basic-based-chatbot-python
A simple rule-based chatbot built using Python. It takes user input like "hello", "how are you", or "bye" and responds with predefined replies using if-elif logic, functions, loops, and I/O operations.
# ✅ Run this in Google Colab
!pip install -q gradio spacy
!python -m spacy download en_core_web_sm

import gradio as gr
import spacy
import random

# Load spaCy model
nlp = spacy.load("en_core_web_sm")

# Predefined intent-based responses
responses = {
    "greeting": ["Hello! 👋", "Hi there! 😊", "Hey! How can I help you today?"],
    "python": ["Python is a powerful programming language!", "Use `print()` to display output.", "Try loops like `for` or `while` in Python."],
    "blockchain": ["Blockchain is a secure distributed ledger system.", "It powers Bitcoin, Ethereum, and other cryptocurrencies."],
    "thanks": ["You're welcome! 😊", "Happy to help!", "Anytime!"],
    "bye": ["Goodbye! 👋", "See you next time!", "Take care!"],
    "howareyou": ["I'm just code, but I'm doing great! 😄", "All systems operational! 🤖", "Feeling binary-tastic! How about you?"]
}

# Detect intent using spaCy
def get_intent(user_input):
    user_input_lower = user_input.lower()
    if "how are you" in user_input_lower:
        return "howareyou"
    doc = nlp(user_input_lower)
    for token in doc:
        if token.lemma_ in ["hello", "hi", "hey"]:
            return "greeting"
        elif token.lemma_ in ["python", "code", "program"]:
            return "python"
        elif token.lemma_ in ["blockchain", "crypto", "ethereum", "bitcoin"]:
            return "blockchain"
        elif token.lemma_ in ["thank", "thanks"]:
            return "thanks"
        elif token.lemma_ in ["bye", "goodbye", "exit"]:
            return "bye"
    return None

# Chat function for looped conversation
def chatbot(message, history):
    intent = get_intent(message)
    if intent:
        response = random.choice(responses[intent])
    else:
        response = "Sorry, I didn't understand that. 🤖"
    return response

# Launch chat interface
gr.ChatInterface(
    fn=chatbot,
    title="🤖 ChatBuddy - NLP Chatbot",
    description="Ask me about Python, blockchain, or just say hello!",
    theme="default"
).launch()
