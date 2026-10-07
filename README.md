Task-4

```csharp
using System;

class Program
{
    static void Main()
    {
        int playerIntegrity = 150;
        int playerMemory = 10;
        int restoreUses = 5;

        Console.WriteLine("=== СЕТТИНГ: КИБЕРПАНК ===");
        Console.WriteLine("Вы — нетраннер-наемник. Ваша цель — получить доступ к ядру ИИ.\n");

        int[][] enemies = new int[][]
        {
            new int[] { 1, 80, 10 },
            new int[] { 2, 120, 15 },
            new int[] { 3, 150, 20 }
        };

        bool isPlayerAlive = true;
        bool isVictory = false;

        for (int wave = 0; wave < enemies.Length; wave++)
        {
            string enemyName = GetEnemyName(enemies[wave][0]);
            int enemyHp = enemies[wave][1];
            int enemyDamage = enemies[wave][2];

            Console.WriteLine($"\n--- Волна {wave + 1}: Загрузка противника... {enemyName} ---");
            Console.WriteLine($"Целостность: 150/150 | Оперативная память: {playerMemory}/10");

            while (playerIntegrity > 0 && enemyHp > 0)
            {
                DrawBar("Целостность", playerIntegrity, 150, ConsoleColor.Green);
                DrawBar("Оперативная память", playerMemory, 10, ConsoleColor.Cyan);
                DrawBar("HP " + enemyName, enemyHp, enemies[wave][1], ConsoleColor.Red);
                Console.WriteLine();

                int action;
                bool isValid;
                do
                {
                    Console.WriteLine("\nВыберите действие:");
                    Console.WriteLine("1 — Загрузить вирус (расход ЦП: 0)");
                    Console.WriteLine("2 — Взлом ядра (расход ОЗУ: 2)");
                    Console.WriteLine("3 — Экранирование (снижение входящего урона на 50%)");
                    Console.WriteLine("4 — Восстановление ОЗУ (расход: 1 использование)");
                    Console.Write("Ваш выбор: ");

                    string input = Console.ReadLine();

                    // ЧИТ-КОД: Проверка секретной комбинации
                    if (input == "777")
                    {
                        action = 777;
                        isValid = true;
                    }
                    else
                    {
                        isValid = int.TryParse(input, out action) && action >= 1 && action <= 4;

                        if (!isValid)
                        {
                            Console.ForegroundColor = ConsoleColor.Red;
                            Console.WriteLine("Ошибка: введите цифру от 1 до 4!\n");
                            Console.ResetColor();
                        }
                        else if (action == 2 && playerMemory < 2)
                        {
                            isValid = false;
                            Console.ForegroundColor = ConsoleColor.Yellow;
                            Console.WriteLine("Ошибка: недостаточно Оперативной памяти! Нужно 2 единицы.\n");
                            Console.ResetColor();
                        }
                        else if (action == 4 && restoreUses <= 0)
                        {
                            isValid = false;
                            Console.ForegroundColor = ConsoleColor.Yellow;
                            Console.WriteLine("Ошибка: модули восстановления исчерпаны!\n");
                            Console.ResetColor();
                        }
                    }

                } while (!isValid);

                bool playerDefended = false;

                switch (action)
                {
                    case 1:
                        int baseDamage = 25;
                        Console.WriteLine($"\n[АТАКА] Вирус загружен. Урон: {baseDamage}");
                        enemyHp -= baseDamage;
                        break;

                    case 2:
                        playerMemory -= 2;
                        int critDamage = new Random().Next(30, 50);
                        Console.WriteLine($"\n[ВЗЛОМ] Взлом удался! Урон: {critDamage}");
                        enemyHp -= critDamage;
                        break;

                    case 3:
                        playerDefended = true;
                        Console.WriteLine("\n[ОБОРОНА] Протоколы шифрования усилены. Следующий урон снижен вдвое.");
                        break;

                    case 4:
                        restoreUses--;
                        playerMemory += 3;
                        if (playerMemory > 5) playerMemory = 10;
                        Console.WriteLine($"\n[ВОССТАНОВЛЕНИЕ] ОЗУ пополнено. Осталось восстановлений: {restoreUses}");
                        break;

                    // ЧИТ-КОД: Исполнение чита
                    case 777:
                        Console.ForegroundColor = ConsoleColor.DarkMagenta;
                        Console.WriteLine("\n[ЧИТ-АКТИВАЦИЯ] Критическая уязвимость ядра противника...");
                        System.Threading.Thread.Sleep(1000);
                        enemyHp = 0;
                        Console.ResetColor();
                        break;
                }

                if (enemyHp <= 0)
                {
                    Console.ForegroundColor = ConsoleColor.Magenta;
                    Console.WriteLine($"\n[УСПЕХ] {enemyName} деактивирован. Волна {wave + 1} зачищена.");
                    Console.ResetColor();
                    break;
                }

                Console.WriteLine("\nХод противника...");
                int incomingDamage = enemyDamage;
                if (playerDefended)
                {
                    incomingDamage = (int)Math.Ceiling(incomingDamage / 2.0);
                    Console.WriteLine($"Нанесено {incomingDamage} урона (сработало экранирование).");
                }
                else
                {
                    Console.WriteLine($"Нанесено {incomingDamage} урона.");
                }
                playerIntegrity -= incomingDamage;

                if (playerIntegrity <= 0)
                {
                    isPlayerAlive = false;
                    break;
                }

                System.Threading.Thread.Sleep(800);
            }

            if (!isPlayerAlive)
            {
                break;
            }
        }

        if (isPlayerAlive)
        {
            isVictory = true;
        }

        Console.WriteLine("\n=== ИТОГИ СЕАНСА ===");
        if (isVictory)
        {
            Console.ForegroundColor = ConsoleColor.Green;
            Console.WriteLine("ВЗЛОМ ЗАВЕРШЕН УСПЕШНО. Доступ к ядру ИИ получен. Вы победили!");
        }
        else
        {
            Console.ForegroundColor = ConsoleColor.Red;
            Console.WriteLine("СИСТЕМНЫЙ СБОЙ. Ваша целостность исчерпана. Связь прервана.");
        }
        Console.ResetColor();
    }

    static string GetEnemyName(int id)
    {
        return id switch
        {
            1 => "Охранный дрон",
            2 => "Штурмовой киборг",
            3 => "ИИ безопасности"
        };
    }

    static void DrawBar(string label, int current, int max, ConsoleColor color)
    {
        int barSize = 20;
        int filled = (int)((double)current / max * barSize);

        Console.Write($"{label}: [");
        Console.ForegroundColor = color;
        for (int i = 0; i < barSize; i++)
        {
            if (i < filled)
                Console.Write("#");
            else
                Console.Write("-");
        }
        Console.ResetColor();
        Console.WriteLine($"] ({current}/{max})");
    }
  }
```
// Делали Аристов и Белозеров игра (КИБЕРПАНК)
