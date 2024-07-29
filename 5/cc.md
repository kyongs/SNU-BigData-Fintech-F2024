### Concurrency Control 실습

```sql
-- 초기 설정
TRUNCATE TABLE account;
INSERT INTO account (id, balance) VALUES (1, 100);
COMMIT;

------------------------------------
-- Now, under "SERIALIZABLE" mode --
------------------------------------

-- 트랜잭션 T2 (세션 1에서 실행)
\c your_database your_username
BEGIN TRANSACTION ISOLATION LEVEL SERIALIZABLE;
SELECT * FROM account;
-- 결과:
-- id | balance
-- ----+---------
--  1 | 100

-- 트랜잭션 T1 (세션 2에서 실행)
\c your_database your_username
BEGIN;
UPDATE account SET balance = balance + 10 WHERE id = 1;
COMMIT;

-- 트랜잭션 T2 (세션 1에서 실행)
SELECT * FROM account WHERE id = 1;
-- 결과:
-- id | balance
-- ----+---------
--  1 | 100
-- "non-repeatable read" 없음

-- 트랜잭션 T3 (세션 3에서 실행)
\c your_database your_username
BEGIN;
INSERT INTO account (id, balance) VALUES (2, 200);
COMMIT;

-- 트랜잭션 T2 (세션 1에서 실행)
SELECT * FROM account;
SELECT * FROM account WHERE id = 1;
SELECT SUM(balance) FROM account;
-- 결과:
-- id | balance
-- ----+---------
--  1 | 100
-- 합계(sum): 100
-- "phantom phenomenon" 없음

ROLLBACK;

------------------------------------
-- Now, under "READ COMMITTED" mode --
------------------------------------

-- 트랜잭션 T2 (세션 1에서 실행)
SET SESSION CHARACTERISTICS AS TRANSACTION ISOLATION LEVEL READ COMMITTED;
BEGIN;
DO $$
DECLARE
    bal NUMERIC;
BEGIN
    SELECT balance INTO bal FROM account WHERE id = 1;

    -- 트랜잭션 T1 (세션 2에서 실행)
    -- 다른 세션에서 다음 명령을 실행
    PERFORM pg_sleep(1);
    UPDATE account SET balance = balance + 10 WHERE id = 1;

    -- 트랜잭션 T2 (세션 1에서 실행)
    UPDATE account SET balance = bal * 1.1 WHERE id = 1;
END $$;
COMMIT;

-- 트랜잭션 T2 (세션 1에서 실행)
SELECT * FROM account WHERE id = 1;
-- 결과:
-- id | balance
-- ----+---------
--  1 | 110
-- "scholar lost update" 문제 발생

ROLLBACK;

```
