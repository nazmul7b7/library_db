# library_db


-- View all books with author and category
SELECT 
    b.book_id,
    b.title,
    a.name AS author,
    c.category_name,
    b.published_year,
    b.available_copies
FROM books b
LEFT JOIN authors a ON b.author_id = a.author_id
LEFT JOIN categories c ON b.category_id = c.category_id;


-- Find all members who borrowed books
SELECT DISTINCT m.member_id, m.name, m.email
FROM members m
JOIN borrowings bo ON m.member_id = bo.member_id;


-- Show borrowing history with book titles
SELECT 
    bo.borrow_id,
    m.name AS member,
    b.title AS book,
    bo.borrow_date,
    bo.return_date
FROM borrowings bo
JOIN members m ON bo.member_id = m.member_id
JOIN books b ON bo.book_id = b.book_id
ORDER BY bo.borrow_date DESC;


-- Find overdue books (not returned for more than 14 days)
SELECT 
    m.name AS member,
    b.title AS book,
    bo.borrow_date
FROM borrowings bo
JOIN members m ON bo.member_id = m.member_id
JOIN books b ON bo.book_id = b.book_id
WHERE bo.return_date IS NULL
  AND bo.borrow_date < CURDATE() - INTERVAL 14 DAY;

  
-- Count total books per category
SELECT 
    c.category_name,
    COUNT(b.book_id) AS total_books
FROM categories c
LEFT JOIN books b ON c.category_id = b.category_id
GROUP BY c.category_name;
