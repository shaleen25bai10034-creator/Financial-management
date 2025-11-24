# Financial Management Program (Beginner Friendly, Fixed Version)
import json

# Load existing data
try:
    file = open("data.json", "r")
    data = json.load(file)
    file.close()
except:
    data = []

while True:
    print("\n========== FINANCIAL MANAGER ==========")
    print("1. Add Transaction")
    print("2. View All Transactions")
    print("3. View Summary")
    print("4. Delete Transaction")
    print("5. Exit")

    choice = input("Enter your choice: ")

    # ADD TRANSACTION
    if choice == "1":
        print("\n--- Add Transaction ---")
        t_type = input("Enter type (income / expense): ").lower()

        if t_type not in ["income", "expense"]:
            print("❌ Invalid type! Must be 'income' or 'expense'.")
            continue

        if t_type == "income":
            print("Examples of income sources: salary, gift, interest, freelancing")
            category = input("Enter income source: ")
        else:
            print("Examples of expenses: food, rent, transport, entertainment")
            category = input("Enter expense category: ")

        amount = float(input("Enter amount: "))
        date = input("Enter date (YYYY-MM-DD): ")
        desc = input("Enter short description: ")

        # Assign unique ID
        if len(data) == 0:
            new_id = 1
        else:
            new_id = data[-1]["id"] + 1

        record = {
            "id": new_id,
            "type": t_type,
            "category": category,
            "amount": amount,
            "date": date,
            "description": desc
        }

        data.append(record)

        file = open("data.json", "w")
        json.dump(data, file, indent=4)
        file.close()

        print("✅ Transaction Added Successfully!")

    # VIEW TRANSACTIONS
    elif choice == "2":
        print("\n--- All Transactions ---")
        if len(data) == 0:
            print("No records found.")
        else:
            for t in data:
                print(f"ID: {t['id']} | {t['type'].upper()} | {t['category']} | ₹{t['amount']} | {t['date']} | {t['description']}")

    # SUMMARY
    elif choice == "3":
        print("\n--- Summary ---")
        total_income = 0
        total_expense = 0

        for t in data:
            if t["type"] == "income":
                total_income += t["amount"]
            elif t["type"] == "expense":
                total_expense += t["amount"]

        balance = total_income - total_expense

        print("Total Income: ₹", total_income)
        print("Total Expense: ₹", total_expense)
        print("Remaining Balance: ₹", balance)

    # DELETE
    elif choice == "4":
        print("\n--- Delete Transaction ---")
        tid = int(input("Enter ID to delete: "))
        found = False
        for t in data:
            if t["id"] == tid:
                data.remove(t)
                found = True
                break

        if found:
            file = open("data.json", "w")
            json.dump(data, file, indent=4)
            file.close()
            print("🗑️ Transaction Deleted Successfully!")
        else:
            print("❌ Transaction ID not found!")

    # EXIT
    elif choice == "5":
        print("\nExiting Program... Goodbye!")
        break

    else:
        print("Invalid Choice! Please try again.")
