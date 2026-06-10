# Short Response: Schema Design and Normalization

Answer each question below. Write in complete sentences (3–5 per answer).

---

## Question 1

The table below stores data for a library's checkout system. Identify every normalization rule it violates and describe how you would fix the schema. You do not need to write SQL — describe the tables you would create and why.

| checkout_id | patron_id | patron_name | patron_email     | book_id | book_title                | author_name       | genres                   |
| ----------- | --------- | ----------- | ---------------- | ------- | ------------------------- | ----------------- | ------------------------ |
| 1           | 10        | Maya Patel  | maya@email.com   | 201     | The Left Hand of Darkness | Ursula K. Le Guin | Science Fiction, Fantasy |
| 2           | 11        | Jordan Kim  | jordan@email.com | 202     | Beloved                   | Toni Morrison     | Fiction, Historical      |
| 3           | 10        | Maya Patel  | maya@email.com   | 202     | Beloved                   | Toni Morrison     | Fiction, Historical      |
| 4           | 12        | Sam Torres  | sam@email.com    | 201     | The Left Hand of Darkness | Ursula K. Le Guin | Science Fiction, Fantasy |

**Your answer:** 

- In this table, multiple columns have more than one value, which breaks the atomic rule. We need to have only one value in each column.
- There is no unique primary key for book_id, which makes it difficult to identify the exact book.
- There is repeated data in multiple columns. For example, patron information repeats every time a book is checked out.

- A patrons table to store patron_id, patron_name, and patron_email so patron information is never repeated.
- A books table to store book_id, book_title, and author_name so book information is never repeated.
- A genres table to store each genre as its own row instead of cramming multiple genres into one cell.
- A book_genres association table to connect books and genres since a book can have many genres and a genre can belong to many books.
- A checkouts table to store checkout_id, patron_id, and book_id as foreign keys to record who checked out what without repeating any patron or book details.

## Question 2

Explain the difference between one-to-many relationships and many-to-many relationships by providing a real world example of each. Then describe how each is represented in a relational database. Use the term "association/bridge" table in your response.

**Your answer:**

- A `One-to-many `relationships are row in a table can be referenced by many rows in the table. For example: one team can have many players but each player belongs to only one team. In a relational database the foreign key is represented on the `many` side, so many players point back to one team. 

- A `many-to-many` relationships is where rows in a table can be referenced by many rows in aother tables. For example: one player can play in many games, and game can have many players. In a relational database, this can't be represented with just a foreign key, that is why we need `association/bridge` that connects two tables together. 

## Question 3

What is referential integrity? How does PostgreSQL enforce it, and why does this enforcement determine the order in which you must create — and drop — tables?

**Your answer:**

- Referential integrity means every foreign key value in a table must enforces a value that actually exists in the parent table. 
- PostgreSQL enforece it automatically. It checks every insert, update, and delete against anything that does not matching the insert and the values for it. 
- The table has to exist before we are able to reference it. Also, we can't drop a table that other tables are still pointing to, so we have to first drop the (child table) and then we are able to drop the (parent table). 


## Question 4

Why does an association table need a `UNIQUE (col1, col2)` constraint on its two foreign key columns? What specific problem does this prevent, and why wouldn't making each column individually `UNIQUE` solve it?

**Your answer:**

- The association table need a `UNIQUE` because two rows can't share the same `(col1, col2)`, without it we may accidentally add the same player to the same game twice. 
- By using `UNIQUE` we solve the problem of repetition, so we do not include the same thing twice. 
- By making each column `UNIQUE` we only allow one player to each game, and each player can play on one game. This will break the rule of `many-to-many`.

