# Math_153_Initial_Code

import math

def is_prime(num):
    if num < 2:
        return False
    for i in range(2, int(math.sqrt(num))+1):
        if num % i == 0:
            return False
    return True

def guess_number():
    print("Please specify the range of numbers.")
    lower_limit = int(input("Enter the lower limit of the range: "))
    upper_limit = int(input("Enter the upper limit of the range (maximum allowed: 10,000): "))

    if upper_limit > 10000:
        print("The upper limit cannot exceed 10,000. Please restart the program.")
        return

    if lower_limit < 0 or lower_limit >= upper_limit:
        print(
            "Invalid range. The lower limit must be non-negative and less than the upper limit. Please restart the program.")
        return

    possible_numbers = [str(num) for num in range(lower_limit, upper_limit + 1)]

    print(f"Range accepted: {lower_limit} to {upper_limit}.")
    print(f"Possible numbers initialized: {len(possible_numbers)} possibilities.")

    print("Answer the following questions about your number:")
    is_even = input("Is your number even? (yes/no): ").strip().lower()
    if is_even == "yes":
        possible_numbers = [num for num in possible_numbers if int(num) % 2 == 0]
    elif is_even == "no":
        possible_numbers = [num for num in possible_numbers if int(num) % 2 != 0]

    is_square = input("Is your number a perfect square? (yes/no): ").strip().lower()
    if is_square == "yes":
        possible_numbers = [num for num in possible_numbers if math.isqrt(int(num)) ** 2 == int(num)]
    elif is_square == "no":
        possible_numbers = [num for num in possible_numbers if math.isqrt(int(num)) ** 2 != int(num)]

    is_cube = input("Is your number a perfect cube? (yes/no): ").strip().lower()
    if is_cube == "yes":
        possible_numbers = [num for num in possible_numbers if round(int(num) ** (1 / 3)) ** 3 == int(num)]
    elif is_cube == "no":
        possible_numbers = [num for num in possible_numbers if round(int(num) ** (1 / 3)) ** 3 != int(num)]

    is_prime_number = input("Is your number a prime number? (yes/no): ").strip().lower()
    if is_prime_number == "yes":
        possible_numbers = [num for num in possible_numbers if is_prime(int(num))]
    elif is_prime_number == "no":
        possible_numbers = [num for num in possible_numbers if not is_prime(int(num))]

    print(f"Filtered numbers after general questions: {len(possible_numbers)} possibilities.")

    digit_sum = int(input("Enter the sum of the digits of your number: "))
    digit_product = int(input("Enter the product of the digits of your number: "))

    def calculate_product(num_str):
        product = 1
        for char in num_str:
            product *= int(char)
        return product

    possible_numbers = [
        num for num in possible_numbers
        if sum(int(digit) for digit in num) == digit_sum and calculate_product(num) == digit_product
    ]

    print(f"Filtered numbers after sum and product: {len(possible_numbers)} possibilities.")

    while len(possible_numbers) > 1:
        relevant_digits = set(digit for num in possible_numbers for digit in num)

        for digit in sorted(relevant_digits):
            print(f"Does the digit '{digit}' appear in your number? (yes/no)")
            response = input().strip().lower()

            if response == "yes":
                position = int(input(f"At which position (1 to {len(possible_numbers[0])}) is the digit '{digit}'? "))
                if not (1 <= position <= len(possible_numbers[0])):
                    print(f"Invalid position. Please enter a position between 1 and {len(possible_numbers[0])}.")
                    continue

                possible_numbers = [
                    num for num in possible_numbers if position <= len(num) and num[position - 1] == digit
                ]

            elif response == "no":
                possible_numbers = [
                    num for num in possible_numbers if digit not in num
                ]

            else:
                print("Invalid response. Please answer with 'yes' or 'no'.")

            # Early exit if only one number remains
            if len(possible_numbers) == 1:
                print(f"The number is: {possible_numbers[0]}")
                return

        for num in possible_numbers:
            for digit in relevant_digits:
                if num.count(digit) > 1:
                    print(f"The digit '{digit}' is repeated in your number.")
                    repeated_position = int(input(f"At which position is the other repeated digit '{digit}'? "))
                    if not (1 <= repeated_position <= len(num)):
                        print(f"Invalid position. Please enter a position between 1 and {len(num)}.")
                        continue

                    possible_numbers = [
                        num for num in possible_numbers if num.count(digit) > 1 and num[repeated_position - 1] == digit
                    ]

                    if len(possible_numbers) == 1:
                        print(f"The number is: {possible_numbers[0]}")
                        return

guess_number()
