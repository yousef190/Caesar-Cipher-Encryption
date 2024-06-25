def caesar_cipher(text: str, key: str, encrypt: bool = True) -> str:
    """
    Applies the Caesar Cipher to the given text using a custom key.

    Args:
        text (str): The text to encrypt or decrypt.
        key (str): The custom key to use for encryption or decryption.
        encrypt (bool, optional): If True, encrypts the text. If False, decrypts the text. Defaults to True.

    Returns:
        str: The encrypted or decrypted text.
    """
    result = ""
    
    if key.isdigit():  # Key is a numerical value
        key_value = int(key)
    elif len(key) == 1:  # Key is a single character
        key_value = ord(key)
    else:  # Key is a custom mapping (e.g. A=C)
        key_original, key_mapped = key.split("=")
        key_value = ord(key_mapped) - ord(key_original)
    
    for char in text:
        if char.isalpha():
            ascii_offset = 65 if char.isupper() else 97
            if encrypt:
                result += chr((ord(char) - ascii_offset + key_value) % 26 + ascii_offset)
            else:
                result += chr((ord(char) - ascii_offset - key_value) % 26 + ascii_offset)
        elif char.isdigit():
            if encrypt:
                result += str((int(char) + key_value) % 10)
            else:
                result += str((int(char) - key_value) % 10)
        elif char.isspace():
            result += char
        else:
            if encrypt:
                result += chr((ord(char) - 32 + key_value) % 94 + 32)
            else:
                result += chr((ord(char) - 32 - key_value) % 94 + 32)
    
    return result


def main():
    while True:
        choice = input("Do you want to (E)ncrypt, (D)ecrypt, or (Q)uit? ").upper()
        if choice == 'Q':
            print("Goodbye!")
            break
        elif choice in ['E', 'D']:
            message = input("Enter your message: ")
            while True:
                key = input("Enter your custom key (a numerical value, a single character, or a custom mapping like A=C): ")
                if key.isdigit() or (len(key) == 1) or ("=" in key):
                    break
                else:
                    print("Invalid key format. Please enter a numerical value, a single character, or a custom mapping like A=C.")
            result = caesar_cipher(message, key, choice == 'E')
            print(f"The result is: {result}")
        else:
            print("Invalid choice. Please choose E, D, or Q.")


if __name__ == "__main__":
    main()
