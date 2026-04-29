Завдання 1 Контрольний запуск 

Параметри для завдань:
NOISE_LEVEL = 0
LEARNING_RATE = 0.01
INITIAL_GUESS = [50000, 50000]
TRUE_POSITION = [45000, 35000]

Результати:
Знайдена позиція: (45000.00, 35000.00)
Похибка: 0.00 м
Ітерацій: 433

<img width="1548" height="506" alt="image" src="https://github.com/user-attachments/assets/d1a60ea2-480c-46c7-96c2-d6023581794f" />
<img width="1568" height="432" alt="image" src="https://github.com/user-attachments/assets/606f37e7-1c67-416b-bcfb-56a41a890db9" />


Відповіді на питання:

1. Чому похибка не завжди рівно 0.00 м, навіть без шуму?
Алгоритм зупиняється не тоді, коли досяг точного мінімуму, а коли різниця між Loss на сусідніх ітераціях стає меншою за `TOLERANCE`. Тобто він просто перестає помітно рухатися і зупиняється. На графіку "Delta Loss" це момент, коли синя крива перетинає червону пунктирну лінію.

2. Як пов'язані графіки "Величина Градієнта" і "Крива навчання"?
Вони виглядають однаково - обидва монотонно спадають. Це логічно: розмір кроку алгоритму = `learning_rate × gradient`. Чим менший градієнт - тим менший крок — тим повільніше падає Loss. Градієнт — це "двигун" навчання.


Завдання 2 - Дослідження learning_rate 

Параметри: NOISE_LEVEL = 1e-6, TRUE_POSITION = [45000, 35000], INITIAL_GUESS = [50000, 50000]

<img width="1568" height="525" alt="image" src="https://github.com/user-attachments/assets/0207d91f-0d0c-4818-b0f9-b11fbab2e91e" />
<img width="1568" height="425" alt="image" src="https://github.com/user-attachments/assets/1ced2458-430b-40c6-b1cf-9d1218bc1b54" />
<img width="1512" height="500" alt="image" src="https://github.com/user-attachments/assets/faf2b99c-1091-443b-82ad-90aa1a6642b7" />
<img width="1521" height="473" alt="image" src="https://github.com/user-attachments/assets/9b055881-67c4-4d97-94ff-2d19e527c66a" />
<img width="1568" height="476" alt="image" src="https://github.com/user-attachments/assets/569e8348-9902-4e5e-86c7-4ea3a09f59ea" />
<img width="1568" height="438" alt="image" src="https://github.com/user-attachments/assets/85bf35b3-31b1-4d82-a041-bc6d5159eb6e" />


<table>
  <thead>
    <tr>
      <th>Експеримент</th>
      <th>LEARNING_RATE</th>
      <th>Ітерацій</th>
      <th>Похибка</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>2a</td>
      <td>2.0</td>
      <td>100 000 (не збігся)</td>
      <td>740 789 м</td>
    </tr>
    <tr>
      <td>2b</td>
      <td>1e-5</td>
      <td>100 000 (не збігся)</td>
      <td>237 м</td>
    </tr>
    <tr>
      <td>2c</td>
      <td>0.01</td>
      <td>433</td>
      <td>304 м</td>
    </tr>
  </tbody>
</table>

Відповіді на питання:

1. Що сталося у 2a? (Розбіжність)
LR занадто великий - алгоритм на кожному кроці "перестрибував" через мінімум і летів ще далі. На графіку Loss одразу підскочив і завис. На 2D-графіку - хаотична спіраль за сотні кілометрів від цілі. Це і є розбіжність: замість спуску в "чашу" - вилітаємо з неї.

2. Що сталося у 2b? (Стагнація)
LR занадто малий - кроки мікроскопічні. Loss повільно падає, але за 100 000 ітерацій так і не досяг Tolerance. У 2c вистачило 433 ітерацій. Це стагнація: алгоритм рухається правильно, але надто повільно.

3. Чому LR - найважливіший параметр?
Він контролює розмір кроку. Завеликий = розбіжність. Замалий = стагнація. Тільки правильний LR дає швидку і точну збіжність


Завдання 3 - Дослідження NOISE_LEVEL 

Параметри: LEARNING_RATE = 0.01, TRUE_POSITION = [45000, 35000], INITIAL_GUESS = [50000, 50000]

<img width="1568" height="499" alt="image" src="https://github.com/user-attachments/assets/6d3e1717-7c37-4af1-8430-af8279a20d71" />
<img width="1568" height="446" alt="image" src="https://github.com/user-attachments/assets/b062283f-e226-4149-9dbe-021ab76593b6" />
<img width="1568" height="488" alt="image" src="https://github.com/user-attachments/assets/ed56c179-792b-4423-99e3-a2947918043a" />
<img width="1568" height="451" alt="image" src="https://github.com/user-attachments/assets/840b6f04-e813-4e1f-b5ce-fa25df1b090b" />
<img width="1526" height="507" alt="image" src="https://github.com/user-attachments/assets/ec63f30e-3604-4e92-a041-6dc15487e9b3" />
<img width="1524" height="472" alt="image" src="https://github.com/user-attachments/assets/38d36dcd-84f7-4e19-b542-0c1ccbac6dd4" />


1. Тбалиця
<table>
  <thead>
    <tr>
      <th>NOISE_LEVEL (с)</th>
      <th>Шум (м)</th>
      <th>Фінальна похибка (м)</th>
      <th>Ітерацій</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1e-9 (1 нс)</td>
      <td>~0.3 м</td>
      <td><strong>0.09 м</strong></td>
      <td>433</td>
    </tr>
    <tr>
      <td>1e-6 (1 мкс)</td>
      <td>~300 м</td>
      <td><strong>318.79 м</strong></td>
      <td>433</td>
    </tr>
    <tr>
      <td>1e-4 (100 мкс)</td>
      <td>~30 000 м</td>
      <td><strong>16 423.13 м</strong></td>
      <td>547</td>
    </tr>
  </tbody>
</table>

Відповіді на питання:

2. Чому у 3c алгоритм збігся, але похибка величезна?
Алгоритм правильно знайшов мінімум - але мінімум зашумленої функції втрат. Вхідні дані TDoA були настільки спотворені, що "правильна відповідь для брудних даних" знаходиться далеко від реальної позиції. Loss падає і збігається - але до хибного мінімуму. Принцип: алгоритм не знає, що дані неправдиві.


Завдання 4 - Дослідження INITIAL_GUESS 

Параметри: NOISE_LEVEL = 1e-6, LEARNING_RATE = 0.01, TRUE_POSITION = [45000, 35000]

<img width="1561" height="510" alt="image" src="https://github.com/user-attachments/assets/e347fb75-d953-49f2-8ec5-9601d033b070" />
<img width="1563" height="440" alt="image" src="https://github.com/user-attachments/assets/b97221a5-9aef-4187-90cb-cf91a41fd9ec" />
<img width="1568" height="496" alt="image" src="https://github.com/user-attachments/assets/df341317-9061-4629-9b83-533ad71654d4" />
<img width="1568" height="421" alt="image" src="https://github.com/user-attachments/assets/2dd9a90d-f934-4521-b811-364b38116d7f" />
<img width="1536" height="498" alt="image" src="https://github.com/user-attachments/assets/5fa49630-4b35-49dc-a7fc-a2faa6f72c78" />
<img width="1568" height="430" alt="image" src="https://github.com/user-attachments/assets/4c16a135-851b-44a9-b245-87cee5551b60" />


<table>
  <thead>
    <tr>
      <th>Експеримент</th>
      <th>INITIAL_GUESS</th>
      <th>Ітерацій</th>
      <th>Похибка</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>4a</td>
      <td>[50000, 50000]</td>
      <td>433</td>
      <td>117.03 м</td>
    </tr>
    <tr>
      <td>4b</td>
      <td>[0, 0]</td>
      <td>364</td>
      <td>244.46 м</td>
    </tr>
    <tr>
      <td>4c</td>
      <td>[-500000, -500000]</td>
      <td>57 918</td>
      <td>961 591 м</td>
    </tr>
  </tbody>
</table>

Відповіді на питання:

1. Порівняння ітерацій:
4a і 4b зійшлися приблизно однаково (приблизно 400 ітерацій). 4c знадобилося у 134 рази більше - бо старт був за 500+ км від цілі, і алгоритм довго "добирався".

2. Чи вплинула початкова точка на точність?
Так, і суттєво - але не через форму функції. У 4c алгоритм стартував у зовсім іншому регіоні з іншим рельєфом функції втрат і збігся до хибного локального мінімуму. Якби функція була ідеальною опуклою "чашею" - початкова точка впливала б лише на швидкість. Але реальна TDoA-функція нелінійна, тому дуже далекий старт може призвести до неправильного результату.

Завдання 5 - Дослідження геометрії 

Параметри: NOISE_LEVEL = 1e-6, LEARNING_RATE = 0.01, INITIAL_GUESS = [50000, 50000]

<img width="1538" height="492" alt="image" src="https://github.com/user-attachments/assets/f5eee296-63ae-4e59-9dfc-0f75150fc6b2" />
<img width="1517" height="458" alt="image" src="https://github.com/user-attachments/assets/d6ff838d-1aaa-4dab-878f-6dd3b269ef29" />
<img width="1506" height="512" alt="image" src="https://github.com/user-attachments/assets/75ffac19-25c1-41d5-8d9c-87ce20b28776" />
<img width="1506" height="437" alt="image" src="https://github.com/user-attachments/assets/f24ca792-12f0-4677-8cce-a0cf8e78eeba" />


<table>
  <thead>
    <tr>
      <th>Експеримент</th>
      <th>TRUE_POSITION</th>
      <th>Ітерацій</th>
      <th>Похибка</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>5a (хороша геометрія)</td>
      <td>[45000, 35000]</td>
      <td>434</td>
      <td><strong>78.04 м</strong></td>
    </tr>
    <tr>
      <td>5b (погана геометрія)</td>
      <td>[45000, -135000]</td>
      <td>100 000 (не збігся)</td>
      <td><strong>1 922.36 м</strong></td>
    </tr>
  </tbody>
</table>
Відповіді на питання:

1. Чому похибки відрізняються у 25 разів при однаковому шумі?
У 5a ціль всередині трикутника станцій - сигнали приходять з різних сторін, гіперболи TDoA перетинаються під великим кутом, і позиція визначається точно. У 5b всі станції знаходяться приблизно в одному напрямку від цілі - гіперболи майже паралельні, перетинаються під мізерним кутом. Маленький шум у часі перетворюється на великий шум у просторі. Це і є GDOP: погана геометрія багаторазово підсилює вплив шуму.

2. Чому алгоритм зупинився у 5b, хоча похибка ще велика?
Функція втрат у зоні поганої геометрії має "пласке дно каньйону" - вздовж одного напрямку Loss майже не змінюється навіть при зміщенні позиції на кілометри. Градієнт мізерний, кроки крихітні, Delta Loss не досягає Tolerance. Алгоритм не може "побачити" різниці між близькою і далекою позицією - рельєф занадто пологий.


Завдання 6 - Дослідження TOLERANCE 

Параметри: TRUE_POSITION = [45000, -135000] ,NOISE_LEVEL = 1e-6, LEARNING_RATE = 0.01

<img width="1568" height="471" alt="image" src="https://github.com/user-attachments/assets/19ffb46c-e4dd-4031-b934-84a500c815ec" />
<img width="1532" height="459" alt="image" src="https://github.com/user-attachments/assets/9554bb0f-641f-49c3-b49f-968b076d5f89" />
<img width="1525" height="508" alt="image" src="https://github.com/user-attachments/assets/716aa47d-7677-4542-b6a2-65cb8349367b" />
<img width="1548" height="437" alt="image" src="https://github.com/user-attachments/assets/00aec57b-8b73-47bf-8da9-bfaab47212ad" />
<img width="1499" height="505" alt="image" src="https://github.com/user-attachments/assets/e31f6272-070f-49e8-86ab-cddc71e9aa08" />
<img width="1512" height="449" alt="image" src="https://github.com/user-attachments/assets/ecab10e2-e2f0-4096-8987-3add98941165" />


<table>
  <thead>
    <tr>
      <th>Експеримент</th>
      <th>TOLERANCE</th>
      <th>Ітерацій</th>
      <th>Фінальна похибка</th>
      <th>Фінальний Loss</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>6a</td>
      <td>1e-2</td>
      <td>100 000 (не збігся)</td>
      <td>600.29 м</td>
      <td>~3 000 м²</td>
    </tr>
    <tr>
      <td>6b</td>
      <td>1e-7</td>
      <td>100 000 (не збігся)</td>
      <td>12 734.23 м</td>
      <td>~60 000 м²</td>
    </tr>
    <tr>
      <td>6c</td>
      <td>1e-12</td>
      <td>100 000 (не збігся)</td>
      <td>7 123.94 м</td>
      <td>~14 000 м²</td>
    </tr>
  </tbody>
</table>

Відповіді на питання:

1. Порівняння трійки:
Жоден з трьох не збігся - всі вичерпали MAX_ITERATIONS. Через погану геометрію (GDOP) функція втрат має "пласке дно", і Delta Loss повільно зменшується - занадто повільно, щоб досягти будь-якого з порогів. Різниця у фінальних похибках пояснюється різним випадковим шумом у кожному запуску, а не ефектом Tolerance

2. Чи варто платити 50 000+ ітерацій за перехід до 1e-12?
Ні, бо усі три варіанти дали похибки від 600 м до 12 км - і жоден не збігся. TOLERANCE не може виправити погану геометрію чи великий шум. Це лише критерій зупинки. Якщо функція втрат деформована шумом або має пологий рельєф через GDOP - ніяке зменшення порогу не покращить результат. Алгоритм просто довше повзтиме по "пласкому дну каньйону".

Загальний висновок

В ході лабораторної роботи було досліджено роботу алгоритму градієнтного спуску для задачі мультилатерації TDoA. Ось головні висновки:

Що найбільше впливає на точність:
1. Рівень шуму (NOISE_LEVEL) - найпряміший вплив. Більший шум = більша похибка. Алгоритм чесно знаходить мінімум, але якщо дані брудні - мінімум знаходиться не там де треба
2. Геометрія базових станцій (GDOP) - якщо ціль знаходиться далеко збоку від усіх станцій, похибка зростає в десятки разів навіть при однаковому шумі Погана геометрія підсилює шум і робить функцію втрат "пласкою" - алгоритм не може знайти точний мінімум
3. Початкова точка (INITIAL_GUESS) - дуже далекий старт може призвести до збіжності у хибний локальний мінімум і похибки в сотні кілометрів

Що найбільше впливає на швидкість:
1. Learning rate - найважливіший параметр. Завеликий - розбіжність (алгоритм "вилітає" з мінімуму). Замалий - стагнація (тисячі зайвих ітерацій). Правильний LR = швидка і стабільна збіжність
2. Початкова точка - чим далі від цілі, тим більше ітерацій потрібно

Головний висновок: TOLERANC і MAX_ITERATIONS - це лише параметри зупинки, вони не можуть виправити фундаментальні проблеми (шум і погану геометрію). Реальна точність системи позиціонування визначається якістю вхідних даних і геометрією розміщення станцій, а не налаштуваннями оптимізатора.
