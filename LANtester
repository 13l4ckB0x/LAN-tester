#include <LiquidCrystal.h> // подключаем стандартную библиотеку LiquidCrystal

// Определяем все аналоговые пины
#define ALine1 A0
#define ALine2 A1
#define ALine3 A2
#define ALine4 A3
#define ALine5 A4
#define ALine6 A5
#define ALine7 A6
#define ALine8 A7

// Определяем все цифровые пины
#define DLine1 9
#define DLine2 8
#define DLine3 7
#define DLine4 6
#define DLine5 5
#define DLine6 4
#define DLine7 3
#define DLine8 2

// Определяем перемееные
float UTest1, UTest2, UTest3, UTest4, UTest5, UTest6, UTest7, UTest8; // Измеренное напряжение для каждой из линий
int stats = 0; // Переменная-флаг для выбора вариантов

// инициализируем объект-экран, передаём использованные
// для подключения контакты на Arduino в порядке:
// RS, E, DB4, DB5, DB6, DB7
LiquidCrystal lcd(13, 12, 0, 1, 10, 11);

void setup() {
  // Выполняется однократно при включении

  lcd.begin(20, 4); // Устанавливаем размер (количество столбцов и строк) экрана

  showLogo(); // Покажем логотип

  // ---------- Стадия 1. Начальная проверка -------------
  /* Однократно, при включении, проверим, не подключено ли к линии какое-либо активное оборудование
     Все управляющие цифровые линии (порты с D2 по D9) переводим в Hi-Z состояние,
     измеряем напряжение на всех линиях. Они должны быть околонулевыми.
     В противном случае мы понимаем, что с другой стороны провода подключено что угодно, но не наша ответная часть,
     и дальше продолжать смысла не имеет. А имеет смысл сообщить об этом на экран, «На линии есть напряжение!»
     Порты Arduino установленные в режим INPUT находятся в высокоимпедансном состоянии.
     Это означает то, что порт ввода дает слишком малую нагрузки на схему, в которую он включен.
     Эквивалентом внутреннему сопротивлению будет резистор 100 МОм подключенный к выводу микросхемы.
     http://arduino.ru/Reference/Constants */

  // Все линии (порты с D2 по D9) переводим в Hi-Z состояние
  pinMode(DLine1, INPUT);
  pinMode(DLine2, INPUT);
  pinMode(DLine3, INPUT);
  pinMode(DLine4, INPUT);
  pinMode(DLine5, INPUT);
  pinMode(DLine6, INPUT);
  pinMode(DLine7, INPUT);
  pinMode(DLine8, INPUT);

  delay(200); // Задержка. Без нее на DLine1 почему-то 0,04V

  // Измеряем напряжение на всех линиях
  readAnalogVoltage();
  lcd.clear();
  lcd.print("TEST-1: ");
  lcd.setCursor(0, 1);
  lcd.print("Main:   12345678");
  lcd.setCursor(0, 3);
  lcd.print("Remote: \xFF\xFF\xFF\xFF\xFF\xFF\xFF\xFF");
  lcd.setCursor(8, 2);
  lcd.print("????????");

  // Проверяем напряжение на всех линиях
  checkLineVoltage();

  // Проверяем напряжения на всех линиях. Они должны быть околонулевыми
  if ((UTest1 or UTest2 or UTest3 or UTest4 or UTest5 or UTest6 or UTest7 or UTest8) > 0)
  {
    lcd.setCursor(8, 0);
    lcd.print("Line is USED");
  }
  else
  { // Условие не выполнено
    lcd.setCursor(8, 0);
    lcd.print("Line is FREE");
    stats = 1; // Поднимаем флаг для запуска Стадии 2
  }
  //delay(5000);

  switch (stats) {
    case 1:
      // Выполняется, когда stats равно 1
      // ---------- Стадия 2. Проверка целостности линий и наличия коротких замыканий -------------
      /* Для каждой из 8 линий проделываем следующее.
          Подаем на нее +5В с порта C, сохраняя все остальные линии порта в высокоимпедансном состоянии,
          и измеряем напряжение на остальных линиях. Если на всех линиях околонулевые значения — исследуемая линия оборвана.
          Если же на какой-то из линий тоже появилось +5В — это КЗ.
          В норме мы увидим некие промежуточные значения */

      // Готовим DLine1 к подаче 5 вольт
      pinMode(DLine1, OUTPUT);
      // Подаем 5 вольт
      digitalWrite(DLine1, HIGH);
      // Измеряем напряжение на всех линиях
      readAnalogVoltage();
      if ((UTest1 or UTest2 or UTest3 or UTest4 or UTest5 or UTest6 or UTest7 or UTest8) == 0)
      {
        Serial.println("Line 1 X");
      }
      Serial.println("Line 1 Done");

      // Возвращаем DLine1 в высокоимпедансное состояние
      pinMode(DLine1, INPUT);
      // Готовом DLine2 и производим действия аналогично предыдущим
      pinMode(DLine2, OUTPUT);
      digitalWrite(DLine2, HIGH);
      readAnalogVoltage();
      Serial.println("Line 2 Done");

      pinMode(DLine2, INPUT);
      pinMode(DLine3, OUTPUT);
      digitalWrite(DLine3, HIGH);
      readAnalogVoltage();
      Serial.println("Line 3 Done");

      pinMode(DLine3, INPUT);
      pinMode(DLine4, OUTPUT);
      digitalWrite(DLine4, HIGH);
      readAnalogVoltage();
      Serial.println("Line 4 Done");

      pinMode(DLine4, INPUT);
      pinMode(DLine5, OUTPUT);
      digitalWrite(DLine5, HIGH);
      readAnalogVoltage();
      Serial.println("Line 5 Done");

      pinMode(DLine5, INPUT);
      pinMode(DLine6, OUTPUT);
      digitalWrite(DLine6, HIGH);
      readAnalogVoltage();
      Serial.println("Line 6 Done");

      pinMode(DLine6, INPUT);
      pinMode(DLine7, OUTPUT);
      digitalWrite(DLine7, HIGH);
      readAnalogVoltage();
      Serial.println("Line 7 Done");

      pinMode(DLine7, INPUT);
      pinMode(DLine8, OUTPUT);
      digitalWrite(DLine8, HIGH);
      readAnalogVoltage();
      Serial.println("Line 8 Done");

      pinMode(DLine8, INPUT);

      break;
  }

}

void loop() {

}

void showLogo() {
  // Чистим экран
  lcd.clear();
  // Рисуем логотип и надпись
  lcd.setCursor(3, 0);
  lcd.print("\xFF");
  lcd.setCursor(2, 1);
  lcd.print("/");
  lcd.setCursor(1, 2);
  lcd.print("\xFF");
  lcd.setCursor(5, 1);
  lcd.print("\xFF");
  lcd.setCursor(4, 2);
  lcd.print("/");
  lcd.setCursor(3, 3);
  lcd.print("\xFF");
  lcd.setCursor(7, 0);
  lcd.print("Net-tool's");
  lcd.setCursor(7, 1);
  lcd.print("Technologies");
  lcd.setCursor(7, 3);
  lcd.print("LAN-tester");
}

void readAnalogVoltage() {
  // Измеряем напряжение на всех линиях
  UTest1 = analogRead(ALine1) * (5.0 / 1023.0);
  Serial.println(UTest1);
  delay(500);
  UTest2 = analogRead(ALine2) * (5.0 / 1023.0);
  Serial.println(UTest2);
  delay(500);
  UTest3 = analogRead(ALine3) * (5.0 / 1023.0);
  Serial.println(UTest3);
  delay(500);
  UTest4 = analogRead(ALine4) * (5.0 / 1023.0);
  Serial.println(UTest4);
  delay(500);
  UTest5 = analogRead(ALine5) * (5.0 / 1023.0);
  Serial.println(UTest5);
  delay(500);
  UTest6 = analogRead(ALine6) * (5.0 / 1023.0);
  Serial.println(UTest6);
  delay(500);
  UTest7 = analogRead(ALine7) * (5.0 / 1023.0);
  Serial.println(UTest7);
  delay(500);
  UTest8 = analogRead(ALine8) * (5.0 / 1023.0);
  Serial.println(UTest8);
  delay(500);
}

void checkLineVoltage()
{
  if (UTest1 > 0)
  {
    lcd.setCursor(8, 2);
    lcd.print("*");#include <LiquidCrystal.h> // подключаем стандартную библиотеку LiquidCrystal

// Определяем все аналоговые пины
#define ALine1 A0
#define ALine2 A1
#define ALine3 A2
#define ALine4 A3
#define ALine5 A4
#define ALine6 A5
#define ALine7 A6
#define ALine8 A7

// Определяем все цифровые пины
#define DLine1 9
#define DLine2 8
#define DLine3 7
#define DLine4 6
#define DLine5 5
#define DLine6 4
#define DLine7 3
#define DLine8 2

// Определяем перемееные
float UTest1, UTest2, UTest3, UTest4, UTest5, UTest6, UTest7, UTest8; // Измеренное напряжение для каждой из линий
int stats = 0; // Переменная-флаг для выбора вариантов

// инициализируем объект-экран, передаём использованные
// для подключения контакты на Arduino в порядке:
// RS, E, DB4, DB5, DB6, DB7
LiquidCrystal lcd(13, 12, 0, 1, 10, 11);

void setup() {
  // Выполняется однократно при включении

  lcd.begin(20, 4); // Устанавливаем размер (количество столбцов и строк) экрана

  showLogo(); // Покажем логотип

  // ---------- Стадия 1. Начальная проверка -------------
  /* Однократно, при включении, проверим, не подключено ли к линии какое-либо активное оборудование
     Все управляющие цифровые линии (порты с D2 по D9) переводим в Hi-Z состояние,
     измеряем напряжение на всех линиях. Они должны быть околонулевыми.
     В противном случае мы понимаем, что с другой стороны провода подключено что угодно, но не наша ответная часть,
     и дальше продолжать смысла не имеет. А имеет смысл сообщить об этом на экран, «На линии есть напряжение!»
     Порты Arduino установленные в режим INPUT находятся в высокоимпедансном состоянии.
     Это означает то, что порт ввода дает слишком малую нагрузки на схему, в которую он включен.
     Эквивалентом внутреннему сопротивлению будет резистор 100 МОм подключенный к выводу микросхемы.
     http://arduino.ru/Reference/Constants */

  // Все линии (порты с D2 по D9) переводим в Hi-Z состояние
  pinMode(DLine1, INPUT);
  pinMode(DLine2, INPUT);
  pinMode(DLine3, INPUT);
  pinMode(DLine4, INPUT);
  pinMode(DLine5, INPUT);
  pinMode(DLine6, INPUT);
  pinMode(DLine7, INPUT);
  pinMode(DLine8, INPUT);

  delay(200); // Задержка. Без нее на DLine1 почему-то 0,04V

  // Измеряем напряжение на всех линиях
  readAnalogVoltage();
  lcd.clear();
  lcd.print("TEST-1: ");
  lcd.setCursor(0, 1);
  lcd.print("Main:   12345678");
  lcd.setCursor(0, 3);
  lcd.print("Remote: \xFF\xFF\xFF\xFF\xFF\xFF\xFF\xFF");
  lcd.setCursor(8, 2);
  lcd.print("????????");

  // Проверяем напряжение на всех линиях
  checkLineVoltage();

  // Проверяем напряжения на всех линиях. Они должны быть околонулевыми
  if ((UTest1 or UTest2 or UTest3 or UTest4 or UTest5 or UTest6 or UTest7 or UTest8) > 0)
  {
    lcd.setCursor(8, 0);
    lcd.print("Line is USED");
  }
  else
  { // Условие не выполнено
    lcd.setCursor(8, 0);
    lcd.print("Line is FREE");
    stats = 1; // Поднимаем флаг для запуска Стадии 2
  }
  //delay(5000);

  switch (stats) {
    case 1:
      // Выполняется, когда stats равно 1
      // ---------- Стадия 2. Проверка целостности линий и наличия коротких замыканий -------------
      /* Для каждой из 8 линий проделываем следующее.
          Подаем на нее +5В с порта C, сохраняя все остальные линии порта в высокоимпедансном состоянии,
          и измеряем напряжение на остальных линиях. Если на всех линиях околонулевые значения — исследуемая линия оборвана.
          Если же на какой-то из линий тоже появилось +5В — это КЗ.
          В норме мы увидим некие промежуточные значения */

      // Готовим DLine1 к подаче 5 вольт
      pinMode(DLine1, OUTPUT);
      // Подаем 5 вольт
      digitalWrite(DLine1, HIGH);
      // Измеряем напряжение на всех линиях
      readAnalogVoltage();
      if ((UTest1 or UTest2 or UTest3 or UTest4 or UTest5 or UTest6 or UTest7 or UTest8) == 0)
      {
        Serial.println("Line 1 X");
      }
      Serial.println("Line 1 Done");

      // Возвращаем DLine1 в высокоимпедансное состояние
      pinMode(DLine1, INPUT);
      // Готовом DLine2 и производим действия аналогично предыдущим
      pinMode(DLine2, OUTPUT);
      digitalWrite(DLine2, HIGH);
      readAnalogVoltage();
      Serial.println("Line 2 Done");

      pinMode(DLine2, INPUT);
      pinMode(DLine3, OUTPUT);
      digitalWrite(DLine3, HIGH);
      readAnalogVoltage();
      Serial.println("Line 3 Done");

      pinMode(DLine3, INPUT);
      pinMode(DLine4, OUTPUT);
      digitalWrite(DLine4, HIGH);
      readAnalogVoltage();
      Serial.println("Line 4 Done");

      pinMode(DLine4, INPUT);
      pinMode(DLine5, OUTPUT);
      digitalWrite(DLine5, HIGH);
      readAnalogVoltage();
      Serial.println("Line 5 Done");

      pinMode(DLine5, INPUT);
      pinMode(DLine6, OUTPUT);
      digitalWrite(DLine6, HIGH);
      readAnalogVoltage();
      Serial.println("Line 6 Done");

      pinMode(DLine6, INPUT);
      pinMode(DLine7, OUTPUT);
      digitalWrite(DLine7, HIGH);
      readAnalogVoltage();
      Serial.println("Line 7 Done");

      pinMode(DLine7, INPUT);
      pinMode(DLine8, OUTPUT);
      digitalWrite(DLine8, HIGH);
      readAnalogVoltage();
      Serial.println("Line 8 Done");

      pinMode(DLine8, INPUT);

      break;
  }

}

void loop() {

}

void showLogo() {
  // Чистим экран
  lcd.clear();
  // Рисуем логотип и надпись
  lcd.setCursor(3, 0);
  lcd.print("\xFF");
  lcd.setCursor(2, 1);
  lcd.print("/");
  lcd.setCursor(1, 2);
  lcd.print("\xFF");
  lcd.setCursor(5, 1);
  lcd.print("\xFF");
  lcd.setCursor(4, 2);
  lcd.print("/");
  lcd.setCursor(3, 3);
  lcd.print("\xFF");
  lcd.setCursor(7, 0);
  lcd.print("Net-tool's");
  lcd.setCursor(7, 1);
  lcd.print("Technologies");
  lcd.setCursor(7, 3);
  lcd.print("LAN-tester");
}

void readAnalogVoltage() {
  // Измеряем напряжение на всех линиях
  UTest1 = analogRead(ALine1) * (5.0 / 1023.0);
  Serial.println(UTest1);
  delay(500);
  UTest2 = analogRead(ALine2) * (5.0 / 1023.0);
  Serial.println(UTest2);
  delay(500);
  UTest3 = analogRead(ALine3) * (5.0 / 1023.0);
  Serial.println(UTest3);
  delay(500);
  UTest4 = analogRead(ALine4) * (5.0 / 1023.0);
  Serial.println(UTest4);
  delay(500);
  UTest5 = analogRead(ALine5) * (5.0 / 1023.0);
  Serial.println(UTest5);
  delay(500);
  UTest6 = analogRead(ALine6) * (5.0 / 1023.0);
  Serial.println(UTest6);
  delay(500);
  UTest7 = analogRead(ALine7) * (5.0 / 1023.0);
  Serial.println(UTest7);
  delay(500);
  UTest8 = analogRead(ALine8) * (5.0 / 1023.0);
  Serial.println(UTest8);
  delay(500);
}

void checkLineVoltage()
{
  if (UTest1 > 0)
  {
    lcd.setCursor(8, 2);
    lcd.print("*");
  }
  else
  {
    lcd.setCursor(8, 2);
    lcd.print("0");
  }
  if (UTest2 > 0)
  {
    lcd.setCursor(9, 2);
    lcd.print("*");
  }
  else
  {
    lcd.setCursor(9, 2);
    lcd.print("0");
  }

  if (UTest3 > 0)
  {
    lcd.setCursor(10, 2);
    lcd.print("*");
  }
  else
  {
    lcd.setCursor(10, 2);
    lcd.print("0");
  }
  if (UTest4 > 0)
  {
    lcd.setCursor(11, 2);
    lcd.print("*");
  }
  else
  {
    lcd.setCursor(11, 2);
    lcd.print("0");
  }
  if (UTest5 > 0)
  {
    lcd.setCursor(12, 2);
    lcd.print("*");
  }
  else
  {
    lcd.setCursor(12, 2);
    lcd.print("0");
  }
  if (UTest6 > 0)
  {
    lcd.setCursor(13, 2);
    lcd.print("*");
  }
  else
  {
    lcd.setCursor(13, 2);
    lcd.print("0");
  }

  if (UTest7 > 0)
  {
    lcd.setCursor(14, 2);
    lcd.print("*");
  }
  else
  {
    lcd.setCursor(14, 2);
    lcd.print("0");
  }
  if (UTest8 > 0)
  {
    lcd.setCursor(15, 2);
    lcd.print("*");
  }
  else
  {
    lcd.setCursor(15, 2);
    lcd.print("0");
  }
}
  }
  else
  {
    lcd.setCursor(8, 2);
    lcd.print("0");
  }
  if (UTest2 > 0)
  {
    lcd.setCursor(9, 2);
    lcd.print("*");
  }
  else
  {
    lcd.setCursor(9, 2);
    lcd.print("0");
  }

  if (UTest3 > 0)
  {
    lcd.setCursor(10, 2);
    lcd.print("*");
  }
  else
  {
    lcd.setCursor(10, 2);
    lcd.print("0");
  }
  if (UTest4 > 0)
  {
    lcd.setCursor(11, 2);
    lcd.print("*");
  }
  else
  {
    lcd.setCursor(11, 2);
    lcd.print("0");
  }
  if (UTest5 > 0)
  {
    lcd.setCursor(12, 2);
    lcd.print("*");
  }
  else
  {
    lcd.setCursor(12, 2);
    lcd.print("0");
  }
  if (UTest6 > 0)
  {
    lcd.setCursor(13, 2);
    lcd.print("*");
  }
  else
  {
    lcd.setCursor(13, 2);
    lcd.print("0");
  }

  if (UTest7 > 0)
  {
    lcd.setCursor(14, 2);
    lcd.print("*");
  }
  else
  {
    lcd.setCursor(14, 2);
    lcd.print("0");
  }
  if (UTest8 > 0)
  {
    lcd.setCursor(15, 2);
    lcd.print("*");
  }
  else
  {
    lcd.setCursor(15, 2);
    lcd.print("0");
  }
}
