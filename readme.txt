------------------------------------------------------------------------
JS/HTML/CSS 
Дано:

table .c { color: red }
.a .c { color: green }

<table id=”t”>
    <tr>
        <td class=”c”>Текст</td>
    </tr>
</table>

Задача: написать JavaScript код, делающий “Текст” зелёным, предложите как минимум три варианта (можно больше) (1-2 могут использовать JS библиотеки) только самого кода (копировать задание в ответ не нужно).

Ответ:

1. <script> document.getElementById("t").classList.add("a"); </script>

2. <script> document.getElementsByClassName('c')[0].style.color = 'green'; </script>

3. <script src="https://ajax.googleapis.com/ajax/libs/jquery/2.1.3/jquery.min.js"></script>
    <script> $(".c").css( "color", "green" ); </script>

------------------------------------------------------------------------
БД
Дана таблица с деревом категорий

CREATE TABLE category (
    id integer not null primary key,
    parent_category_id integer references category(id),
    name varchar(100) not null
);

Напишите запросы (БД - “правильная”, умеющая делать подзапросы, различные соединения и прочее):
1. На выборку всех категорий верхнего уровня, начинающихся на “авто”
2. На выборку всех категорий, имеющих не более трёх подкатегорий следующего уровня (без глубины)
3. На выборку всех категорий нижнего уровня (т.е. не имеющих детей)

Ответ:

1. ALTER TABLE category ADD INDEX (name, parent_category_id);
SELECT * FROM category WHERE name RLIKE '^avto' AND parent_category_id = 0;

2. ALTER TABLE category ADD INDEX (parent_category_id);
SELECT *, (SELECT COUNT(*) FROM category WHERE category_new.id = category.parent_category_id) as count
    FROM category as category_new HAVING count <= 3;

3. ALTER TABLE category ADD INDEX (parent_category_id);
SELECT category.* FROM category 
    LEFT JOIN category AS category_new
        ON category.id = category_new.parent_category_id
            WHERE category_new.parent_category_id IS NULL
ALTER TABLE category ADD INDEX (parent_category_id);

------------------------------------------------------------------------
PHP
Дана строка текста.

Написать программу на php, которая определяет является ли строка текста полиндромом (читается с обоих сторон одинаково) и осуществляет вывод строки следующим способом:

а) если строка является полиндромом, то она выводится полностью.

б) если строка не является полиндромом - выводится самый длинный под-полиндром этой строки, т.е. самая длинная часть строки, являющаяся полиндромом.

в) если подполиндромы отсутствуют в строке - выводится первый символ строки.

Примеры полиндромов:
- Аргентина манит негра
- Sum summus mus

Ответ:

<?php
$text = mb_strtolower('Sum summus mus');
$text_new = '';
for ($i = strlen($text) - 1; $i >= 0; $i--) {
    if (strpos($text, $text_new . $text[$i]) === 0){
        $text_new .= $text[$i];
    } else {
        if(strlen($text_new) == 0){
            $text_new = $text['0'];
        }
        break;
    }
}
echo $text_new;

------------------------------------------------------------------------
