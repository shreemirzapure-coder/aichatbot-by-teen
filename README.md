import re
import random
from datetime import datetime

class AIChatbot:
    def __init__(self, name="AI Assistant"):
        self.name = name
        self.user_name = None
        self.conversation_history = []
        
        self.jokes = [
            "Why don't scientists trust atoms? Because they make up everything!",
            "What do you call a fake noodle? An impasta!",
            "Why did the scarecrow win an award? He was outstanding in his field!",
            "What's orange and sounds like a parrot? A carrot!",
            "Why don't eggs tell jokes? They'd crack each other up!"
        ]
        
    def process_input(self, user_input):
        # Convert to lowercase for matching
        text = user_input.lower()
        
        # Store in history
        self.conversation_history.append(("You", user_input))
        
        # Greetings
        if any(word in text for word in ['hi', 'hello', 'hey', 'greetings']):
            return self.greet()
        
        # How are you
        elif 'how are you' in text:
            return self.how_are_you()
        
        # Name related
        elif 'your name' in text:
            return f"My name is {self.name}!"
        
        elif 'my name is' in text:
            # Extract name
            parts = user_input.split('my name is')
            if len(parts) > 1:
                self.user_name = parts[1].strip().split()[0]
                return f"Nice to meet you, {self.user_name}!"
            return "Nice to meet you!"
        
        elif "i am" in text and len(text) < 20:
            parts = user_input.split('i am')
            if len(parts) > 1:
                self.user_name = parts[1].strip().split()[0]
                return f"Hello {self.user_name}! Great to meet you!"
        
        # Jokes
        elif any(word in text for word in ['joke', 'funny', 'laugh']):
            return random.choice(self.jokes)
        
        # Time
        elif 'time' in text:
            now = datetime.now()
            return f"The current time is {now.strftime('%I:%M %p')}"
        
        # Date
        elif 'date' in text:
            now = datetime.now()
            return f"Today's date is {now.strftime('%B %d, %Y')}"
        
        # Calculations
        elif 'calculate' in text or any(op in text for op in ['+', '-', '*', '/']):
            return self.calculate(text)
        
        # Help
        elif 'help' in text or 'what can you do' in text:
            return self.show_help()
        
        # Thanks
        elif any(word in text for word in ['thanks', 'thank you']):
            return "You're welcome! 😊"
        
        # Goodbye
        elif any(word in text for word in ['bye', 'goodbye', 'exit', 'quit']):
            return "goodbye"
        
        # Default response
        else:
            return self.default_response()
    
    def greet(self):
        greetings = [
            f"Hello! I'm {self.name}. How can I help you today?",
            f"Hi there! Great to see you!",
            f"Hey! What's on your mind?"
        ]
        return random.choice(greetings)
    
    def how_are_you(self):
        responses = [
            "I'm doing great, thanks for asking!",
            "I'm functioning at 100%! How about you?",
            "All systems operational and ready to chat!"
        ]
        return random.choice(responses)
    
    def calculate(self, text):
        try:
            # Extract numbers and operator
            # Remove words and keep only numbers and operators
            expression = re.sub(r'[^0-9+\-*/.]', '', text)
            if expression:
                result = eval(expression)
                return f"The result is: {result}"
            else:
                return "I couldn't find a calculation. Try something like '5 + 3'"
        except:
            return "Sorry, I couldn't calculate that. Try something like '5 + 3'"
    
    def show_help(self):
        help_text = """
🤖 I can help you with:
━━━━━━━━━━━━━━━━━━━━━━
🗣️  General conversation (hello, how are you)
😂 Telling jokes (say 'tell me a joke')
🧮 Simple math (try '5 + 3' or 'calculate 10/2')
⏰ Current time and date
📝 Remembering your name
👋 Friendly chat and more!

Just type naturally and I'll respond!
━━━━━━━━━━━━━━━━━━━━━━
        """
        return help_text
    
    def default_response(self):
        responses = [
            "That's interesting! Tell me more.",
            "I see. What else would you like to talk about?",
            "Hmm, that's fascinating! Go on...",
            "Really? Tell me more about that!",
            "Interesting perspective!"
        ]
        
        if self.user_name:
            return f"{self.user_name}, {random.choice(responses).lower()}"
        
        return random.choice(responses)
    
    def chat(self):
        print("\n" + "="*50)
        print(f"🤖 {self.name} - AI Chatbot".center(50))
        print("="*50)
        print("Type 'help' to see what I can do")
        print("Type 'quit' to exit")
        print("-"*50)
        
        while True:
            try:
                # Get user input
                user_input = input("\nYou: ").strip()
                
                if not user_input:
                    continue
                
                # Process the input
                response = self.process_input(user_input)
                
                # Check for exit
                if response == "goodbye":
                    farewells = [
                        f"Goodbye {self.user_name if self.user_name else 'friend'}! Have a great day! 👋",
                        "See you later! Come back anytime! 🌟",
                        "Bye! It was nice chatting with you! 💫"
                    ]
                    print(f"\n🤖 {self.name}: {random.choice(farewells)}")
                    
                    # Show conversation summary
                    print("\n" + "="*50)
                    print("📊 Conversation Summary")
                    print(f"Total messages: {len(self.conversation_history)//2}")
                    if self.user_name:
                        print(f"User name: {self.user_name}")
                    print("="*50)
                    break
                
                # Print response
                print(f"\n🤖 {self.name}: {response}")
                
            except KeyboardInterrupt:
                print("\n\n👋 Goodbye! Thanks for chatting!")
                break
            except Exception as e:
                print(f"\n⚠️  Error: {e}")

# Run the chatbot
if __name__ == "__main__":
    print("\n" + "🌟"*20)
    print("WELCOME TO YOUR AI CHATBOT CREATOR")
    print("🌟"*20)
    
    # Get bot name
    bot_name = input("\nWhat would you like to name your chatbot? \n(Press Enter for 'AI Assistant'): ").strip()
    if not bot_name:
        bot_name = "AI Assistant"
    
    # Create and start chatbot
    chatbot = AIChatbot(bot_name)
    chatbot.chat()
    
    # Ask if they want to save conversation
    save = input("\n💾 Do you want to save this conversation? (yes/no): ").strip().lower()
    if save in ['yes', 'y']:
        filename = f"chat_{datetime.now().strftime('%Y%m%d_%H%M%S')}.txt"
        with open(filename, 'w') as f:
            f.write(f"Chat with {bot_name}\n")
            f.write("="*50 + "\n")
            for speaker, message in chatbot.conversation_history:
                f.write(f"{speaker}: {message}\n")
        print(f"✅ Conversation saved to {filename}")
