# COURSE-FINAL-CIS-30
Program allowing the user to find something to eat and choose a prep method. Have you ever had trouble trying to figure out where to eat? Worry no longer. 

# fooddata.py

# List of preparation methods
prep_methods = ["Cook from Home", "Sit Down Restaurant", "Pickup/Delivery"]

# Default food categories
default_categories = ["American", "BBQ", "Seafood", "Mediterranean", "Mexican", "Italian", "Asian Fusion"]


# Import built-in and custom modules
import random
import fooddata   # custom module

# -----------------------------
# Base Class
# -----------------------------
class FoodSearch:
    def __init__(self, category, price_limit):
        self.category = category
        self.price_limit = price_limit

    def display_info(self):
        print("Food Category:", self.category)
        print("Price Limit: under $", self.price_limit)

    def generate_link(self):
        return ""


# -----------------------------
# Subclass
# -----------------------------
class RestaurantSearch(FoodSearch):
    def __init__(self, category, price_limit, distance):
        FoodSearch.__init__(self, category, price_limit)
        self.distance = distance

    def generate_link(self):
        link = "https://www.google.com/search?q=" + self.category
        link += "+restaurant+under+$" + str(self.price_limit)
        link += "+within+" + str(self.distance) + "+miles"
        return link

    def display_info(self):
        FoodSearch.display_info(self)
        print("Distance:", self.distance, "miles")


# -----------------------------
# Functions
# -----------------------------
def get_price_limit():
    while True:
        try:
            price = int(input("Enter your price limit: "))
            if price > 0:
                return price
            else:
                print("Price must be positive.")
        except:
            print("Invalid number. Try again.")


def get_distance():
    while True:
        try:
            distance = int(input("Enter max distance (1-80 miles): "))
            if 1 <= distance <= 80:
                return distance
            else:
                print("Distance must be between 1 and 80.")
        except:
            print("Invalid number. Try again.")


# -----------------------------
# Main Program Loop
# -----------------------------
def main():

    print("Welcome to the Food Randomizer!")

    price_limit = get_price_limit()

    locked_prep = None
    locked_category = None

    while True:

        # Random selection if not locked
        if locked_prep == None:
            prep = random.choice(fooddata.prep_methods)
        else:
            prep = locked_prep

        if locked_category == None:
            category = random.choice(fooddata.default_categories)
        else:
            category = locked_category

        print("\nPreparation Method:", prep)

        # Create object based on prep method
        if prep == "Cook from Home":
            search = FoodSearch(category, price_limit)
            link = "https://www.google.com/search?q=" + category
            link += "+recipes+under+$" + str(price_limit)

        else:
            distance = get_distance()
            search = RestaurantSearch(category, price_limit, distance)
            link = search.generate_link()

        search.display_info()
        print("Search Link:", link)

        # File output
        file = open("food_results.txt", "a")
        file.write("Prep: " + prep + "\n")
        file.write("Category: " + category + "\n")
        file.write("Price: $" + str(price_limit) + "\n")
        file.write("Link: " + link + "\n")
        file.write("----------------------\n")
        file.close()

        # Menu
        print("\nOptions:")
        print("1 - Restart")
        print("2 - Lock Prep Method")
        print("3 - Lock Category")
        print("4 - Lock Both")
        print("5 - Exit")

        choice = input("Choose option: ")

        if choice == "1":
            locked_prep = None
            locked_category = None
            price_limit = get_price_limit()

        elif choice == "2":
            locked_prep = prep

        elif choice == "3":
            locked_category = category

        elif choice == "4":
            locked_prep = prep
            locked_category = category

        elif choice == "5":
            print("Goodbye!")
            break

        else:
            print("Invalid choice.")




