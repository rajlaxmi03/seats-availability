# seats-availability

Problem Description:
1. There are 80 seats in a coach of a train with only 7 seats in a row and last row of only 3
seats. For simplicity, there is only one coach in this train.
2. One person can reserve up to 7 seats at a time.
3. If a person is reserving seats, the priority will be to book them in one row.
4. If seats are not available in one row then the booking should be done in such a way that the
nearby seats are booked.
5. User can book as many tickets as s/he wants until the coach is full.
6. You don’t have to create login functionality for this application.

* How it should function?

1. Input required will only be the required number of seats. Example: 2 or 4 or 6 or 1 etc.
2. Output should be seats numbers that have been booked for the user along with the display of
all the seats and their availability status through color or number or anything else that you may
feel fit.



**code:
# Initial setup for seats: 80 seats in total, arranged in rows of 7 (11 rows, last row has 3 seats)
seats = [['O' for _ in range(7)] for _ in range(11)]
seats[-1] = ['O', 'O', 'O']  # Last row has only 3 seats


# Display seats function
def display_seats(seats):
    print("\nCurrent Seat Layout:")
    for row in seats:
        print(" ".join(row))
    print()


# Function to book seats
def book_seats(requested_seats):
    if requested_seats < 1 or requested_seats > 7:
        return "You can only book between 1 and 7 seats."

    # Try to find a row with enough continuous available seats
    for row_idx, row in enumerate(seats):
        free_seats = [i for i, seat in enumerate(row) if seat == 'O']

        if len(free_seats) >= requested_seats:
            for i in range(requested_seats):
                row[free_seats[i]] = 'X'  # Mark the seat as booked
            booked_seats = [(row_idx + 1, free_seat + 1) for free_seat in free_seats[:requested_seats]]
            return f"Seats booked: {booked_seats}"

    # If no single row can accommodate the request, try booking nearby seats across rows
    booked_seats = []
    for row_idx, row in enumerate(seats):
        for seat_idx, seat in enumerate(row):
            if seat == 'O' and len(booked_seats) < requested_seats:
                row[seat_idx] = 'X'
                booked_seats.append((row_idx + 1, seat_idx + 1))
            if len(booked_seats) == requested_seats:
                return f"Seats booked across rows: {booked_seats}"

    return "Not enough seats available."


# Example usage
while True:
    display_seats(seats)
    try:
        num_seats = int(input("Enter the number of seats to book (1-7): "))
        result = book_seats(num_seats)
        print(result)
        if result == "Not enough seats available.":
            break
    except ValueError:
        print("Please enter a valid number.")
