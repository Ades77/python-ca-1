# python-ca-1
library = {9780307277671: ['The Da Vinci Code', 'Dan Brown', 5],
           9780679783268: ['Pride and Prejudice', 'Jane Austen', 0],
           9780345476876: ['Interview with the Vampire', 'Anne Rice', 3]}

loaned_books = []  # variable to hold a list of all books loaned out


def print_details(library):
    """Prints all the items in the library."""
    print('Printing library...')
    print('')
    for key, value in library.items():
        print('ISBN:', key)
        print('Title:', value[0])
        print('Author:', value[1])
        print('Copies available:', value[2])
        print('')


def add_book(library, isbn, title, author, copies):
    """Allows the user to update the number of copies if a book is already in the library
    or to add a new entry to the library if the book is not in the library."""
    if isbn in library:  # updates number of copies
        library[isbn][2] += copies
        print('No. of copies of', library[isbn][0], ' updated')
        print('')
    elif isbn not in library:  # adds new entry to library
        library[isbn] = [title, author, copies]
        print('Entry added to the catalogue')
        print('')


def edit_details(library, isbn, title, author, copies):
    """Allows the user to edit an entry already in the library by using ISBN."""
    if isbn in library:
        library[isbn] = [title, author, copies]
        print('Entry updated successfully')
        print('')
    else:
        print('That entry does not exist')
        print('')


def loan_book(library, isbn):
    """Allows the user to loan out a book using ISBN"""
    if isbn in library and library[isbn][2] > 0:  # checks that book is in the library and there are copies available
        print(library[isbn][0],'available for loan, Loan successful')
        print('')
        library[isbn][2] -= 1  # reduces the no. of copies of book by 1
        loaned_books.append(isbn)  # adds the ISBN of that book to a list of loaned books
    elif isbn not in library:
        print('That book is not in the library')
        print('')
    else:
        print(library[isbn][0], 'not available for loan')
        print('')


def return_book(library, isbn):
    """Allows the user to return a book to the library only if it is in the loaned_books list."""
    if isbn in loaned_books:  # checks the list to see if the book was loaned out
        library[isbn][2] += 1  # returns the copy and updates the no of copies by 1
        loaned_books.remove(isbn)  # removes that book from the list of loaned books
        print(library[isbn][0],'returned successfully')
        print('')
    elif isbn not in loaned_books:
        print('Error, That book was not loaned out')
        print('')


def book_search(library, title):
    """Allows the user to search for the ISBN of a book using the book title."""
    count = 0
    flag = 0
    for key,value in library.items():
        count +=1  # adds a number to count for every iteration
        if title.lower() == value[0].lower():  # checks each key,value pair in library and title entered, converts both
            # to lowercase, checks if title entered same as title in library, returns key(ISBN)
            print('ISBN:',key)
            print('')
            flag = 1
        elif count == len(library) and flag == 0:  # when the loop has gone through every entry
            # in library and title not found
            print('That book is not in the library')
            print('')


def isbn_check(keyword):
    """Checks that input from user for ISBN is a 13 digit number"""
    try:  # checks that ISBN is a number
        isbn = int(input('Enter the ISBN of the book you want to '+keyword))
        if len(str(isbn)) != 13 or isbn < 0:  # checks that ISBN is 13 digits and positive
            print('ISBN must be a 13 digit number')
            isbn = isbn_check(keyword)
    except ValueError:
        print('ISBN must be a number')
        isbn = isbn_check(keyword)
    return isbn


def copies_check():
    """Checks that input from user for copies is a positive number"""
    try:  # checks that copies is a number
        copies = int(input('Enter the number of copies'))
        if copies < 1:  # checks if number entered is positive
            print('The amount of copies must be a positive number')
            copies = copies_check()
    except ValueError:
        print('Copies must be a number')
        copies = copies_check()
    return copies


print('****Library Catalogue****')
print('')
print('1. Print library catalogue.')
print('2. Add a book to the library.')
print('3. Edit an entry for a book in the library.')
print('4. Check out a book for loan.')
print('5. Return a book to the library.')
print('6. Search library for a book by title to find ISBN.')
print('7. Print all menu options again')
print('0. Exit the library catalogue.')
print('')

user_input = ''

while user_input != '0':  # menu keeps going until user inputs 0, calls function depending on number entered
    user_input = input('Input the number of the option you want followed by enter to use the catalogue.'
                       'Input 7 for menu options.')
    if user_input == '1':
        print_details(library)
    elif user_input == '2':
        isbn = isbn_check('add')
        copies = copies_check()
        title = input('Enter the title')
        author = input('Enter the author')
        add_book(library, isbn, title, author, copies)
    elif user_input == '3':
        isbn = isbn_check('edit')
        copies = copies_check()
        title = input('Enter the title')
        author = input('Enter the author')
        edit_details(library, isbn, title, author, copies)
    elif user_input == '4':
        isbn = isbn_check('borrow')
        loan_book(library, isbn)
    elif user_input == '5':
        isbn = isbn_check('return')
        return_book(library, isbn)
    elif user_input == '6':
        title = input('Enter the title of the book you want to search')
        book_search(library, title)
    elif user_input == '7':
        print('1. Print library catalogue.')
        print('2. Add a book to the library.')
        print('3. Edit an entry for a book in the library.')
        print('4. Check out a book for loan.')
        print('5. Return a book to the library.')
        print('6. Search library for a book by title to find ISBN.')
        print('7. Print all menu options.')
        print('0. Exit the library catalogue.')
        print('')
    elif user_input == '0':
        print('Thank you for using the library catalogue')
    else:
        print('Input invalid')
