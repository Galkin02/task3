# task3
Mission 3
Видео
Link to video

Запросы к бд
Запросы на создание сущностей базы данных
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    username TEXT UNIQUE,
    password TEXT
);

CREATE TABLE messages (
    id SERIAL PRIMARY KEY,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    "from" INT REFERENCES users(id) ON DELETE CASCADE,
    "to" INT REFERENCES users(id) ON DELETE CASCADE,
    text TEXT
);
Запросы на получение из базы данных
Получить список юзернеймов пользователей:
SELECT username FROM users;
Получить количество отправленных сообщений каждым пользователем:
SELECT u.username, COUNT(m.id) AS number_of_sent_messages
FROM users u
LEFT JOIN messages m ON u.id = m."from"
GROUP BY u.id;
Получить пользователя с самым большим количеством полученных сообщений и само количество:
SELECT u.username, COUNT(m.id) AS number_of_received_messages
FROM users u
LEFT JOIN messages m ON u.id = m."to"
GROUP BY u.id
ORDER BY number_of_received_messages DESC
LIMIT 1;
Получить среднее количество сообщений, отправленных каждым пользователем:
SELECT AVG(sent_messages) AS average_sent_messages
FROM (
    SELECT COUNT(m.id) AS sent_messages
    FROM users u
    LEFT JOIN messages m ON u.id = m."from"
    GROUP BY u.id
) AS subquery;
