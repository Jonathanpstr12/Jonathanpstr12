<?php

class Book {
    public $title;
    protected $author;
    private $price;

    public function __construct($title, $author, $price) {
        $this->title = $title;
        $this->author = $author;
        $this->price = $price;
    }

    public function getDetails() {
        return "Title: {$this->title}, Author: {$this->author}, Price: \${$this->price}";
    }

    public function setPrice($price) {
        $this->price = $price;
    }

    public function __call($name, $arguments) {
        if ($name === 'updateStock') {
            echo "Stock updated for '{$this->title}' with arguments: " . implode(', ', $arguments) . PHP_EOL;
        } else {
            echo "Method '{$name}' not defined in class " . __CLASS__ . PHP_EOL;
        }
    }
}

class Library {
    public $name;
    private $books = [];

    public function __construct($name) {
        $this->name = $name;
    }

    public function addBook(Book $book) {
        $this->books[$book->title] = $book;
    }

    public function removeBook($title) {
        if (isset($this->books[$title])) {
            unset($this->books[$title]);
            echo "Book '{$title}' removed from the library." . PHP_EOL;
        } else {
            echo "Book '{$title}' not found in the library." . PHP_EOL;
        }
    }

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

    public function __destruct() {
        echo "The library '{$this->name}' is now closed." . PHP_EOL;
    }
}

$book1 = new Book('The Great Gatsby', 'F. Scott Fitzgerald', 10.99);
$book2 = new Book('1984', 'George Orwell', 8.99);

$library = new Library('City Library');

$library->addBook($book1);
$library->addBook($book2);

$book1->setPrice(12.99);

$book1->updateStock(50);

$library->listBooks();

$library->removeBook('1984');

echo "Books in the library after removal:" . PHP_EOL;
$library->listBooks();

unset($library);

?>
