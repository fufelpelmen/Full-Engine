# Full Engine 🚀

**Описание:**
Full Engine - это мощный и гибкий движок на языке Python, разработанный для создания разнообразных игр и интерактивных приложений. Он построен на основе библиотеки Pygame, обеспечивая множество возможностей для разработчиков.

**Необходимые библиотеки:**
- `pygame`: Библиотека для разработки 2D игр на Python. Предоставляет функции для работы с графикой, звуком, управлением событиями и многим другим.
- `sys`: Встроенная библиотека Python для взаимодействия с операционной системой. Позволяет работать с аргументами командной строки, путями файловой системы и другими системными функциями.
- `os`: Встроенная библиотека Python для работы с операционной системой. Предоставляет функции для управления файлами, директориями и окружением системы.
- `importlib.util`: Стандартная библиотека Python для работы с динамической загрузкой модулей и пакетов. Позволяет загружать модули во время выполнения программы.
- `warnings`: Встроенная библиотека Python для управления предупреждениями. Позволяет управлять выводом предупреждений во время выполнения программы.
- `json`: Библиотека для работы с данными в формате JSON. Предоставляет функции для сериализации и десериализации данных.

**Основные особенности:**
- **Модульность:** Full Engine предлагает модульную архитектуру, позволяя выделять каждый аспект игры в отдельный модуль. Это облегчает управление и расширение функциональности движка.
  
- **Гибкость:** Разработчики могут легко настраивать и модифицировать поведение движка путем добавления новых модулей или изменения существующих. Это обеспечивает высокую гибкость в создании различных игровых сценариев и приложений.
  
- **Простота использования:** Full Engine предоставляет простой и интуитивно понятный интерфейс для разработки игр и приложений на Python. Разработчики могут быстро начать работу благодаря четкой документации и обширному сообществу пользователей.

**Пример использования мода для Full Engine:**

```python
import pygame
import sys
import os
import importlib.util
import warnings
import json
import pygame

class PingPongMod:
    def __init__(self, game):
        self.game = game
        self.player_color = (255, 255, 255)  # Цвет игрока
        self.robot_color = (255, 0, 0)  # Цвет робота
        self.ball_color = (0, 255, 0)  # Цвет мяча
        self.player_size = (10, 80)  # Размер игрока (ширина, высота)
        self.robot_size = (10, 80)  # Размер робота (ширина, высота)
        self.ball_size = 15  # Размер мяча (диаметр)
        self.player_pos = [20, self.game.height // 2 - self.player_size[1] // 2]  # Положение игрока (x, y)
        self.robot_pos = [self.game.width - 30, self.game.height // 2 - self.robot_size[1] // 2]  # Положение робота (x, y)
        self.ball_pos = [self.game.width // 2, self.game.height // 2]  # Положение мяча (x, y)
        self.ball_velocity = [5, 5]  # Скорость мяча по осям (x, y)
        self.player_score = 0  # Счет игрока
        self.robot_score = 0  # Счет робота

    def handle_events(self):
        # Обработка событий клавиатуры для игрока
        keys = pygame.key.get_pressed()
        if keys[pygame.K_w] and self.player_pos[1] > 0:
            self.player_pos[1] -= 5
        if keys[pygame.K_s] and self.player_pos[1] < self.game.height - self.player_size[1]:
            self.player_pos[1] += 5

    def update(self):
        # Обновление состояния
        # Движение мяча
        self.ball_pos[0] += self.ball_velocity[0]
        self.ball_pos[1] += self.ball_velocity[1]

        # Отскок мяча от верхней и нижней границ поля
        if self.ball_pos[1] <= 0 or self.ball_pos[1] >= self.game.height - self.ball_size:
            self.ball_velocity[1] = -self.ball_velocity[1]

        # Отскок мяча от ракеток игрока и робота
        if self.ball_pos[0] <= self.player_pos[0] + self.player_size[0] and self.player_pos[1] <= self.ball_pos[1] <= self.player_pos[1] + self.player_size[1]:
            self.ball_velocity[0] = -self.ball_velocity[0]
        elif self.ball_pos[0] >= self.robot_pos[0] - self.ball_size and self.robot_pos[1] <= self.ball_pos[1] <= self.robot_pos[1] + self.robot_size[1]:
            self.ball_velocity[0] = -self.ball_velocity[0]

        # Проверка забитых голов
        if self.ball_pos[0] <= 0:
            self.robot_score += 1
            self.reset_ball()
        elif self.ball_pos[0] >= self.game.width:
            self.player_score += 1
            self.reset_ball()

        # Движение ракетки робота за мячом
        if self.ball_pos[1] < self.robot_pos[1] + self.robot_size[1] // 2:
            self.robot_pos[1] -= 4.45
        elif self.ball_pos[1] > self.robot_pos[1] + self.robot_size[1] // 2:
            self.robot_pos[1] += 4.45

    def render(self):
        # Отрисовка игровых объектов
        pygame.draw.rect(self.game.screen, self.player_color, (self.player_pos[0], self.player_pos[1], self.player_size[0], self.player_size[1]))
        pygame.draw.rect(self.game.screen, self.robot_color, (self.robot_pos[0], self.robot_pos[1], self.robot_size[0], self.robot_size[1]))
        pygame.draw.circle(self.game.screen, self.ball_color, (self.ball_pos[0], self.ball_pos[1]), self.ball_size)

        # Отрисовка счета
        font = pygame.font.Font(None, 36)
        player_text = font.render("Player: " + str(self.player_score), True, (255, 255, 255))
        robot_text = font.render("Robot: " + str(self.robot_score), True, (255, 255, 255))
        self.game.screen.blit(player_text, (20, 20))
        self.game.screen.blit(robot_text, (self.game.width - 150, 20))

    def reset_ball(self):
        # Сброс положения мяча при забитии гола и его скорости
        self.ball_pos = [self.game.width // 2, self.game.height // 2]
        self.ball_velocity = [5, 5]

def load(game):
    # Функция загрузки мода
    mod = PingPongMod(game)
    return mod


