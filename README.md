<!DOCTYPE html>
<html lang="en">
<head>
    <script type="module"></script>
    <script src="https://www.gstatic.com/firebasejs/11.6.0/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/11.6.0/firebase-analytics.js"></script>
    <script>
    const firebaseConfig = {
        apiKey: "AIzaSyAKPBkP_X3IHJdjNO8AAtX-PTWD6Eq6xe4",
        authDomain: "ka-library.firebaseapp.com",
        databaseURL: "https://ka-library-default-rtdb.asia-southeast1.firebasedatabase.app",
        projectId: "ka-library",
        storageBucket: "ka-library.firebasestorage.app",
        messagingSenderId: "296231938633",
        appId: "1:296231938633:web:94ea16b6500fdc460aed98",
        measurementId: "G-PLEX4FXJ2H"
      };
      // Initialize Firebase
  const app = initializeApp(firebaseConfig);
  const analytics = getAnalytics(app);
    </script>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>KA Library</title>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@4.5.2/dist/css/bootstrap.min.css" rel="stylesheet" />
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #f8f9fa;
    }
    .navbar {
      background-color: #007bff;
    }
    .navbar-brand, .nav-link {
      color: white !important;
    }
    .nav-link:hover {
      color: #ffcc00 !important;
    }
    .section {
      padding: 40px 20px;
      text-align: center;
    }
    input {
      width: 250px;
      margin: 5px;
      padding: 10px;
    }
    button {
      padding: 10px 20px;
    }
  </style>
</head>

<body>

<!-- Navigation Bar -->
<nav class="navbar navbar-expand-lg">
  <a class="navbar-brand" href="#">KA Library</a>
  <div class="collapse navbar-collapse">
    <ul class="navbar-nav ml-auto">
      <li class="nav-item"><a class="nav-link" href="#search">Search</a></li>
      <li class="nav-item"><a class="nav-link" href="#addBook">Add Book</a></li>
      <li class="nav-item"><a class="nav-link" href="#bookList">All Books</a></li>
    </ul>
  </div>
</nav>

<!-- Search Section -->
<div class="section" id="search">
  <h2>🔍 Search Books</h2>
  <input type="text" id="searchInput" placeholder="Enter title or author..." />
  <button onclick="searchBooks()">Search</button>
  <div id="searchResults" class="mt-3"></div>
</div>

<!-- Add Book Section -->
<div class="section bg-light" id="addBook">
  <h2>➕ Add a Book</h2>
  <input type="text" id="bookTitle" placeholder="Book Title" />
  <input type="text" id="bookAuthor" placeholder="Author" />
  <button onclick="addBook()">Add Book</button>
  <div id="addBookMessage" class="mt-2 text-success"></div>
</div>

<!-- Book List Section -->
<div class="section" id="bookList">
  <h2>📚 All Books</h2>
  <ul id="booksDisplay" class="list-group w-75 mx-auto"></ul>
</div>

<script>
  function addBook() {
    const title = document.getElementById("bookTitle").value.trim();
    const author = document.getElementById("bookAuthor").value.trim();
    const message = document.getElementById("addBookMessage");

    if (!title || !author) {
      message.textContent = "Please enter both title athor.";
      message.classList.add("text-danger");nd au
      return;
    }

    // Get existing books from localStorage
    let books = JSON.parse(localStorage.getItem("books")) || [];

    // Add new book
    books.push({ title, author });

    // Save to localStorage
    localStorage.setItem("books", JSON.stringify(books));

    // Clear input fields and show success message
    document.getElementById("bookTitle").value = "";
    document.getElementById("bookAuthor").value = "";
    message.textContent = "Book added successfully!";
    message.classList.remove("text-danger");
    message.classList.add("text-success");

    // Refresh All Books list
    displayBooks();
  }

  function displayBooks() {
    const books = JSON.parse(localStorage.getItem("books")) || [];
    const list = document.getElementById("booksDisplay");
    list.innerHTML = ""; // Clear previous list

    if (books.length === 0) {
      list.innerHTML = "<li class='list-group-item'>No books available.</li>";
    } else {
      books.forEach((book, index) => {
        const item = document.createElement("li");
        item.className = "list-group-item text-left";
        item.textContent = `${book.title} by ${book.author}`;
        list.appendChild(item);
      });
    }
  }

  function searchBooks() {
    const query = document.getElementById("searchInput").value.toLowerCase().trim();
    const resultDiv = document.getElementById("searchResults");
    const books = JSON.parse(localStorage.getItem("books")) || [];

    const results = books.filter(book =>
      book.title.toLowerCase().includes(query) ||
      book.author.toLowerCase().includes(query)
    );

    resultDiv.innerHTML = "<h5>Results:</h5>";

    if (results.length === 0) {
      resultDiv.innerHTML += "<p>No matching books found.</p>";
    } else {
      results.forEach(book => {
        resultDiv.innerHTML += `<p><strong>${book.title}</strong> by ${book.author}</p>`;
      });
    }
  }

  // Load book list on page load
  window.onload = displayBooks;
</script>

</body>
</html>
