import random
import string

class Player:
    def __init__(self, name, health=100, damage=10):
        self.name = name
        self.health = health
        self.damage = damage

    def attack(self, other_player):
        damage_dealt = random.randint(1, self.damage)
        other_player.health -= damage_dealt
        print(f"{self.name} attacks {other_player.name} for {damage_dealt} damage.")

    def is_alive(self):
        return self.health > 0

class PocketDimension:
    def __init__(self, level):
        self.name = self.generate_name()
        self.level = level
        self.difficulty = level * 10
        self.reward = random.randint(level * 5, level * 10)

    def generate_name(self):
        adj = ['Mystical', 'Enchanted', 'Ethereal', 'Forbidden', 'Astral', 'Divine', 'Arcane']
        noun = ['Forest', 'Desert', 'Ocean', 'Mountain', 'Sky', 'Cavern', 'Realm']
        return random.choice(adj) + " " + random.choice(noun)

    def battle(self, player):
        opponent = Player(f"{self.name} Enemy", health=self.difficulty, damage=self.difficulty // 10)
        print(f"Entering Pocket Dimension '{self.name}' (Level {self.level}). Let the battle begin!")
        while player.is_alive() and opponent.is_alive():
            player.attack(opponent)
            if not opponent.is_alive():
                print(f"{self.name} defeated!")
                return self.reward
            opponent.attack(player)
            if not player.is_alive():
                print(f"{self.name} victorious!")
                return 0

class Game:
    def __init__(self):
        self.player = None
        self.current_dimension = 0

    def start(self):
        player_name = self.generate_name()
        self.player = Player(player_name)
        print(f"Welcome, {self.player.name}! Your journey begins.")

    def generate_name(self):
        prefix = ['Brave', 'Fearless', 'Adventurous', 'Bold', 'Valiant', 'Courageous', 'Daring']
        suffix = ['Explorer', 'Hero', 'Traveler', 'Seeker', 'Adventurer', 'Champion', 'Warrior']
        return random.choice(prefix) + " " + random.choice(suffix)

    def level_up(self):
        self.current_dimension += 1
        if self.current_dimension >= 5:  # Assuming there are 5 dimensions
            print("Congratulations! You have mastered all dimensions!")
            return False
        else:
            return True

    def play(self):
        while True:
            dimension = PocketDimension(self.current_dimension + 1)
            reward = dimension.battle(self.player)
            if reward > 0:
                print(f"You have defeated {dimension.name} and gained {reward} experience.")
                if not self.level_up():
                    break
            else:
                print("Game over. Try again.")
                break

# Example usage:
if __name__ == "__main__":
    game = Game()
    game.start()
    game.play()
