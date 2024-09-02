<?php

// Book Class Definition
class Book {
    public $title;
    protected $author;
    private $price;

    // Constructor to initialize title, author, and price
    public function __construct($title, $author, $price) {
        $this->title = $title;
        $this->author = $author;
        $this->price = $price;
    }

    // Method to get details of the book
    public function getDetails() {
        return "Title: {$this->title}, Author: {$this->author}, Price: \${$this->price}";
    }

    // Method to set the price of the book (only within class or its subclasses)
    public function setPrice($price) {
        $this->price = $price;
    }

    // Magic method to handle calls to non-existent methods
    public function __call($name, $arguments) {
        if ($name === 'updateStock') {
            echo "Stock updated for '{$this->title}' with arguments: " . implode(', ', $arguments) . PHP_EOL;
        } else {
            echo "Method '{$name}' not defined in class " . __CLASS__ . PHP_EOL;
        }
    }
}

// Library Class Definition
class Library {
    public $name;
    private $books = [];

    // Constructor to initialize the library with a name
    public function __construct($name) {
        $this->name = $name;
    }

    // Method to add a book to the library
    public function addBook(Book $book) {
        $this->books[$book->title] = $book;
    }

    // Method to remove a book from the library by title
    public function removeBook($title) {
        if (isset($this->books[$title])) {
            unset($this->books[$title]);
            echo "Book '{$title}' removed from the library." . PHP_EOL;
        } else {
            echo "Book '{$title}' not found in the library." . PHP_EOL;
        }
    }

    // Method to list all books in the library
    public function listBooks() {
        if (empty($this->books)) {
            echo "No books available in the library." . PHP_EOL;
        } else {
            echo "Books in the library:" . PHP_EOL;
            foreach ($this->books as $book) {
                echo $book->getDetails() . PHP_EOL;
            }
        }
    }

    // Destructor to clear the library and output a message
    public function __destruct() {
        echo "The library '{$this->name}' is now closed." . PHP_EOL;
    }
}

// Implementation of the classes and methods

// Creating instances of Book
$book1 = new Book('The Great Gatsby', 'F. Scott Fitzgerald', 10.99);
$book2 = new Book('1984', 'George Orwell', 8.99);

// Creating an instance of Library
$library = new Library('City Library');

// Adding books to the library
$library->addBook($book1);
$library->addBook($book2);

// Updating the price of a book
$book1->setPrice(12.99);

// Attempting to call a non-existent method to trigger __call()
$book1->updateStock(50);

// Listing all books in the library
$library->listBooks();

// Removing a book from the library
$library->removeBook('1984');

// Listing all books after removal
echo "Books in the library after removal:" . PHP_EOL;
$library->listBooks();

// Destroying the Library object to trigger the destructor
unset($library);
?>
