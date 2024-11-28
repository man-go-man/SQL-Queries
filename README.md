## Excamples of SQL-Queries

### Schema SQL
CREATE TABLE Products (
  id INT AUTO_INCREMENT PRIMARY KEY,
  p_name VARCHAR (100) NOT NULL,
  manufacturer VARCHAR (50) NOT NULL,
  p_count INT DEFAULT 0,
  price DECIMAL (10,2) NOT NULL
);

INSERT INTO Products (p_name, manufacturer, p_count, price) VALUES
('T3xSuper Lite Stainless Steel fluted .30-06 570мм', 'Tikka', 5, 418570),
('Х-Bolt SF .30-06 Composite Brown ADJ (резьба)', 'Browning', 3, 297960),
('Самозарядный карабин Вепрь СОК-98, L-520 к. 5,45x39', 'Молот', 9, 48300),
('Самозарядный карабин Вепрь-супер СОК-97М к .223 Rem орех L-550', 'Молот', 7, 79680),
('Карабин Bar .308 MK3 Composite Brown Fluted ADJ THR 530', 'Browning', 1, 543550),
('Самозарядный карабин Argo-E .30-06 20"', 'Benelli', 2, 301000),
('Карабин Wild Comfort.30-06 560', 'Benelli', 2, 319200),
('Карабин МЕ16 кал. 22LR черный Люкс Soft Touch', 'Ataman', 1, 96910),
('Самозарядный карабин СОК-95М-01 к .308 Win ламинат L-650', 'Молот', 10, 80400),
('Карабин SPARTAN ELITE .223 Rem, barrel 16"', 'ADC', 1, 515140),
('Bar .308 MK3 Composite Black Brown Fluted HC THR 530', 'Browning', 2, 420300),
('Карабин МЕ16 кал. 22LR, ламинированное покрытие, люкс', 'Ataman', 8, 96910),
('Карабин B.308 Win MK3 Reflex Composite CF fluted HC 530', 'Browning', 1, 472000),
('Карабин SPA Standard Black .22LR, 10-зарядный', 'ISSC', 10, 90170),
('Карабин SPARTAN ELITE .223 Rem, barrel 18"', 'ADC', 1, 515140),
('Карабин Wild Comfort.308 560', 'Benelli', 3, 319200),
('Комбинированное ружье МР-94 L-600 к .30-06 и 12/76', 'Калашников', 10, 48170),
('T1xMTR .22LR 510мм (Резьба) карабин', 'Tikka', 3, 170960),
('Самозарядный карабин Argo-E 9,3x62', 'Benelli', 5, 296300),
('Карабин 100 .30-06 Сlassic XT WS пластик', 'Sauer', 3, 352970)
;
#создать таблицу order_product
CREATE TABLE order_product (
  order_id INT AUTO_INCREMENT PRIMARY KEY,
  order_date DATE,
  customer_name VARCHAR (50) NOT NULL,
  order_item VARCHAR (100) NOT NULL,
  quantity INT NOT NULL,
  price DECIMAL (10,2) NOT NULL,
  cost DECIMAL (10,2) NOT NULL
);

INSERT INTO order_product (order_date, customer_name, order_item, quantity, price, cost) VALUES
(CURDATE()-1, 'Иванов А.И.', 'Самозарядный карабин Вепрь СОК-98, L-520 к. 5,45x39', 1, 48300, quantity*price),
('2024-11-10', 'ООО "ЧВК "Чиполлино"', "Самозарядный карабин Вепрь СОК-98, L-520 к. 5,45x39", 1, 48300, quantity*price),
('2024-11-01', 'ООО "ЧВК "Чиполлино"', 'Карабин МЕ16 кал. 22LR черный Люкс Soft Touch', 10, 96910, quantity*price),
(CURDATE()-15, 'ООО "Патриот"', 'Карабин 100 .30-06 Сlassic XT WS пластик', 5, 352970, quantity*price)
;

### Query SQL

#вывести все данные таблицы Products
SELECT * FROM Products;

#сортировка по цене по убыванию в Products
SELECT * FROM Products ORDER BY price DESC;

#вывести количество записей в Products
SELECT COUNT(*) FROM Products;

#вывести все записи таблицы Products с определённым столбцом
SELECT * FROM Products WHERE manufacturer = "Browning";

#вывести все данные таблицы Products с ценой между 200000 и 300000
SELECT * FROM Products WHERE price BETWEEN 200000 AND 300000;

#вывести записи с группировкой по производителю без Browning с подсч`том количества товаров каждого
SELECT manufacturer, SUM(p_count) FROM Products WHERE manufacturer <> "Browning" GROUP BY manufacturer;

#вывести записи остатков в магазине (Products) после заказа (order_product)
SELECT 
    Products.id AS iD,
    Products.p_name AS product_name,
    Products.p_count - COALESCE(SUM(order_product.quantity), 0) AS rest
FROM 
    Products
LEFT JOIN 
    order_product 
ON 
    Products.p_name = order_product.order_item
GROUP BY 
    Products.id, Products.p_name, Products.p_count;
