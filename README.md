# project
import json
import os
from collections import defaultdict

class ExpenseTracker:
    def __init__(self):
        self.expenses = defaultdict(list)
        self.categories = set()

    def add_expense(self, amount, description, category):
        self.expenses[category].append({"amount": amount, "description": description})
        self.categories.add(category)

    def get_monthly_summary(self):
        monthly_summary = defaultdict(float)
        for category, expenses in self.expenses.items():
            for expense in expenses:
                monthly_summary[category] += expense["amount"]
        return monthly_summary

    def get_category_summary(self, category):
        return sum(expense["amount"] for expense in self.expenses[category])

    def save_expenses(self, filename):
        with open(filename, "w") as f:
            json.dump(self.expenses, f)

    def load_expenses(self, filename):
        if os.path.exists(filename):
            with open(filename, "r") as f:
                self.expenses = defaultdict(list, json.load(f))
                for category in self.expenses.keys():
                    self.categories.add(category)
        else:
            print("No expense data found.")

    def show_expenses(self):
        for category, expenses in self.expenses.items():
            print(f"Category: {category}")
            for expense in expenses:
                print(f"Amount: ${expense['amount']}, Description: {expense['description']}")

def main():
    tracker = ExpenseTracker()
    tracker.load_expenses("expenses.json")

    while True:
        print("\nExpense Tracker Menu:")
        print("1. Add Expense")
        print("2. View Monthly Summary")
        print("3. View Category-wise Summary")
        print("4. View All Expenses")
        print("5. Save and Quit")

        choice = input("Enter your choice (1-5): ")

        if choice == "1":
            amount = float(input("Enter the amount spent: "))
            description = input("Enter a brief description: ")
            category = input("Enter the category (e.g., food, transportation, entertainment): ")
            tracker.add_expense(amount, description, category)
            print("Expense added successfully!")

        elif choice == "2":
            monthly_summary = tracker.get_monthly_summary()
            print("Monthly Summary:")
            for category, total in monthly_summary.items():
                print(f"{category}: ${total}")

        elif choice == "3":
            category = input("Enter the category to view its summary: ")
            category_summary = tracker.get_category_summary(category)
            print(f"Total {category} expenses: ${category_summary}")

        elif choice == "4":
            tracker.show_expenses()

        elif choice == "5":
            tracker.save_expenses("expenses.json")
            print("Expenses saved successfully. Exiting...")
            break

        else:
            print("Invalid choice. Please enter a number from 1 to 5.")

if __name__ == "__main__":
    main()
![Screenshot (1)](https://github.com/Sailokesh3069/project/assets/162870856/ea960431-a70a-42c3-9960-0f5a09c62d09)


https://github.com/Sailokesh3069/project/assets/162870856/959d711a-49b8-425c-9644-a5f1b038e552

