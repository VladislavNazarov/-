"""
Текстовые крестики-нолики
Игра для двух игроков на одном компьютере
"""

import os
import sys

class TicTacToe:
    def __init__(self):
        """Инициализация игры: создаем пустое поле и определяем текущего игрока"""
        self.board = [' ' for _ in range(9)]  # Игровое поле 3x3
        self.current_player = 'X'  # X всегда ходит первым
        self.winner = None
        self.game_over = False
        self.moves_count = 0
        
    def print_board(self):
        """Отображение игрового поля в консоли"""
        os.system('cls' if os.name == 'nt' else 'clear')  # Очистка экрана
        
        print("\n" + "="*30)
        print("    ТЕКСТОВЫЕ КРЕСТИКИ-НОЛИКИ")
        print("="*30)
        print("\nИгроки: X и O")
        print(f"Текущий ход: Игрок {self.current_player}")
        print()
        
        # Отображение игрового поля с номерами клеток для удобства
        print("    1 | 2 | 3 ")
        print("   -----------")
        print("    4 | 5 | 6 ")
        print("   -----------")
        print("    7 | 8 | 9 ")
        print("\n" + "-"*30)
        print("Текущее состояние игры:")
        print()
        
        # Отображение текущего состояния доски
        for i in range(0, 9, 3):
            print(f"    {self.board[i]} | {self.board[i+1]} | {self.board[i+2]} ")
            if i < 6:
                print("   ---|---|---")
        print()
        
        # Если игра окончена, показываем результат
        if self.game_over:
            if self.winner:
                print(f" Победил игрок {self.winner}! ")
            else:
                print(" Ничья! ")
            print()
    
    def make_move(self, position):
        """
        Выполнение хода игрока
        Возвращает True, если ход выполнен успешно, иначе False
        """
        # Проверка корректности ввода
        if not position.isdigit():
            print("Ошибка: введите число от 1 до 9")
            return False
            
        pos = int(position) - 1  # Преобразуем в индекс (0-8)
        
        # Проверка допустимости хода
        if pos < 0 or pos > 8:
            print("Ошибка: число должно быть от 1 до 9")
            return False
            
        if self.board[pos] != ' ':
            print("Ошибка: эта клетка уже занята")
            return False
        
        # Выполнение хода
        self.board[pos] = self.current_player
        self.moves_count += 1
        
        # Проверка на победу
        if self.check_winner():
            self.winner = self.current_player
            self.game_over = True
        # Проверка на ничью
        elif self.moves_count == 9:
            self.game_over = True
        # Смена игрока
        else:
            self.current_player = 'O' if self.current_player == 'X' else 'X'
            
        return True
    
    def check_winner(self):
        """Проверка всех возможных выигрышных комбинаций"""
        win_combinations = [
            # Горизонтальные линии
            [0, 1, 2], [3, 4, 5], [6, 7, 8],
            # Вертикальные линии
            [0, 3, 6], [1, 4, 7], [2, 5, 8],
            # Диагонали
            [0, 4, 8], [2, 4, 6]
        ]
        
        for combo in win_combinations:
            a, b, c = combo
            if self.board[a] == self.board[b] == self.board[c] != ' ':
                return True
        return False
    
    def reset_game(self):
        """Сброс игры к начальному состоянию"""
        self.__init__()
    
    def play(self):
        """Основной игровой цикл"""
        while True:
            self.print_board()
            
            if self.game_over:
                # Предложение сыграть еще раз
                choice = input("Хотите сыграть еще раз? (да/нет): ").lower()
                if choice in ['да', 'д', 'yes', 'y']:
                    self.reset_game()
                    continue
                else:
                    print("\nСпасибо за игру! До свидания!")
                    break
            
            # Получение хода от игрока
            move = input(f"Игрок {self.current_player}, введите номер клетки (1-9): ").strip()
            
            # Обработка специальных команд
            if move.lower() in ['выход', 'exit', 'quit', 'q']:
                print("\nСпасибо за игру! До свидания!")
                break
            elif move.lower() in ['сброс', 'reset', 'r']:
                self.reset_game()
                continue
            elif move.lower() in ['помощь', 'help', 'h']:
                self.show_help()
                input("\nНажмите Enter чтобы продолжить...")
                continue
            
            # Выполнение хода
            self.make_move(move)
    
    def show_help(self):
        """Показать справку по игре"""
        os.system('cls' if os.name == 'nt' else 'clear')
        print("\n" + "="*50)
        print("СПРАВКА ПО ИГРЕ 'КРЕСТИКИ-НОЛИКИ'")
        print("="*50)
        print("\nЦель игры:")
        print("  Первому игроку (X) и второму игроку (O) нужно")
        print("  выстроить три своих символа в ряд по горизонтали,")
        print("  вертикали или диагонали.")
        print("\nКак играть:")
        print("  - Каждая клетка поля имеет номер от 1 до 9")
        print("  - Чтобы сделать ход, введите номер свободной клетки")
        print("\nСпециальные команды:")
        print("  - 'выход' или 'exit' - завершить игру")
        print("  - 'сброс' или 'reset' - начать новую игру")
        print("  - 'помощь' или 'help' - показать эту справку")
        print("\n" + "="*50)
        print("\nПример игрового поля с номерами клеток:")
        print("    1 | 2 | 3 ")
        print("   ---|---|---")
        print("    4 | 5 | 6 ")
        print("   ---|---|---")
        print("    7 | 8 | 9 ")
        print()

def main():
    """Основная функция для запуска игры"""
    print("Запуск игры 'Крестики-нолики'...")
    
    # Создание экземпляра игры
    game = TicTacToe()
    
    try:
        # Запуск игрового цикла
        game.play()
    except KeyboardInterrupt:
        print("\n\nИгра прервана. До свидания!")
    except Exception as e:
        print(f"\nПроизошла ошибка: {e}")

if __name__ == "__main__":
    main()